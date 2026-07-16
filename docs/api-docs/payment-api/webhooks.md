# Вебхуки

Опрашивать API на предмет изменения статуса платежа (polling) — рабочий,
но неэффективный способ. Вебхуки решают ту же задачу асинхронно: PayFlow
сам отправляет POST-запрос на ваш эндпоинт, когда происходит событие.

## Какие события отправляет PayFlow

| Событие | Когда происходит |
|---|---|
| `payment_intent.succeeded` | Платёж успешно завершён |
| `payment_intent.payment_failed` | Попытка оплаты отклонена |
| `charge.refunded` | Оформлен возврат средств |
| `payment_intent.requires_action` | Нужна дополнительная аутентификация (например, 3-D Secure) |

## Настройка эндпоинта

1. Создайте публично доступный HTTPS-эндпоинт, например
   `POST /webhooks/payflow`.
2. Зарегистрируйте его в **Dashboard → Developers → Webhooks**, указав
   нужные события.
3. PayFlow выдаст **webhook signing secret** (`whsec_...`) — он нужен для
   проверки подлинности запросов (см. ниже).

Пример тела запроса:

```json
{
  "id": "evt_1N8y3Kq...",
  "type": "payment_intent.succeeded",
  "created": 1719840000,
  "data": {
    "object": {
      "id": "pi_3N8x2ZKq...",
      "amount": 2000,
      "currency": "usd",
      "status": "succeeded"
    }
  }
}
```

## Проверка подписи

!!! danger "Обязательный шаг, а не опция"
    Любой, кто узнает URL вашего эндпоинта, может отправить на него
    поддельное тело запроса, похожее на событие PayFlow. Без проверки
    подписи ваш сервер не отличит настоящее уведомление об оплате от
    подделки — а значит, злоумышленник может, например, инициировать
    выдачу товара без реальной оплаты.

PayFlow подписывает каждый запрос HMAC-SHA256 и передаёт подпись в
заголовке `PayFlow-Signature`:

```python
import hashlib
import hmac

def verify_signature(payload: bytes, signature_header: str, secret: str) -> bool:
    expected = hmac.new(secret.encode(), payload, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, signature_header)
```

```python
# Пример обработчика на Flask
@app.route("/webhooks/payflow", methods=["POST"])
def handle_webhook():
    payload = request.get_data()
    signature = request.headers.get("PayFlow-Signature", "")

    if not verify_signature(payload, signature, WEBHOOK_SECRET):
        return "Invalid signature", 400

    event = json.loads(payload)
    if event["type"] == "payment_intent.succeeded":
        mark_order_as_paid(event["data"]["object"]["id"])

    return "", 200
```

## Идемпотентная обработка на вашей стороне

PayFlow гарантирует доставку **как минимум один раз** — то есть одно и то
же событие теоретически может прийти дважды (например, если ваш сервер
ответил `200 OK`, но ответ потерялся по пути). Обрабатывайте события
идемпотентно: сохраняйте `id` события и проверяйте, не обработан ли он
уже, прежде чем менять состояние заказа.

## Повторные попытки доставки

Если ваш эндпоинт не отвечает `2xx` в течение 10 секунд, PayFlow повторит
отправку по расписанию: через 5 минут, 30 минут, 2 часа, 6 часов, затем
раз в 12 часов — всего до 72 часов. После этого событие помечается как
недоставленное и доступно только вручную через Dashboard.

## Тестирование локально

Локальный сервер недоступен из интернета напрямую, поэтому для теста
вебхуков используйте туннель (например, `payflow-cli listen` — аналог
`stripe listen`), который перенаправляет события на `localhost`:

```bash
payflow-cli listen --forward-to localhost:4000/webhooks/payflow
```

## Дальше

- [Ошибки](errors.md) — что означают синхронные ошибки API
- [База знаний → Вебхук PayFlow не приходит](../../knowledge-base/kb-webhook-troubleshooting.md) —
  troubleshooting-статья для частых причин недоставленных событий
