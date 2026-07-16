# Быстрый старт

За пять шагов вы отправите первый тестовый платёж через PayFlow API.

!!! warning "Тестовый режим"
    Все запросы в этом руководстве используют тестовые ключи (`pf_test_...`).
    В тестовом режиме деньги не списываются и не поступают — это изолированная
    песочница. Подробнее о режимах — на странице [«Аутентификация»](authentication.md).

## Шаг 1. Создайте аккаунт

Зарегистрируйтесь на [dashboard.payflow.example](https://dashboard.payflow.example)
(демонстрационный адрес). После регистрации PayFlow автоматически создаёт
для вас пару тестовых ключей — публичный и секретный.

## Шаг 2. Найдите свои API-ключи

В **Dashboard → Developers → API keys** доступны четыре ключа:

| Ключ | Префикс | Где используется |
|---|---|---|
| Publishable test key | `pf_test_pk_...` | На фронтенде, в браузере |
| Secret test key | `pf_test_sk_...` | На бэкенде, никогда не в браузере |
| Publishable live key | `pf_live_pk_...` | Продакшн-фронтенд |
| Secret live key | `pf_live_sk_...` | Продакшн-бэкенд |

Для этого руководства понадобится **Secret test key**.

## Шаг 3. Отправьте первый запрос

=== "curl"

    ```bash
    curl https://api.payflow.example/v1/payment_intents \
      -u pf_test_sk_51H8x...: \
      -d amount=2000 \
      -d currency=usd \
      -d "payment_method_types[]"=card
    ```

=== "Python"

    ```python
    import requests

    response = requests.post(
        "https://api.payflow.example/v1/payment_intents",
        auth=("pf_test_sk_51H8x...", ""),
        data={
            "amount": 2000,
            "currency": "usd",
            "payment_method_types[]": "card",
        },
    )
    print(response.json())
    ```

`amount` указывается в минимальных единицах валюты — то есть `2000` для
USD означает **$20.00**, а не $2000. Это частый источник ошибок в
интеграциях, поэтому вынесено в первый же пример, а не спрятано в
справочнике параметров.

Успешный ответ:

```json
{
  "id": "pi_3N8x2ZKq...",
  "object": "payment_intent",
  "amount": 2000,
  "currency": "usd",
  "status": "requires_payment_method",
  "client_secret": "pi_3N8x2ZKq..._secret_..."
}
```

## Шаг 4. Подтвердите платёж тестовой картой

Используйте номер тестовой карты `4242 4242 4242 4242`, любой будущий
срок действия и любой CVC. PayFlow распознаёт диапазон `4242...` как
тестовую карту, которая всегда завершается успехом.

| Номер карты | Сценарий |
|---|---|
| `4242 4242 4242 4242` | Успешная оплата |
| `4000 0000 0000 0002` | Карта отклонена (`card_declined`) |
| `4000 0000 0000 9995` | Недостаточно средств (`insufficient_funds`) |

Полный список кодов отказа — на странице [«Ошибки»](errors.md).

## Шаг 5. Проверьте статус

```bash
curl https://api.payflow.example/v1/payment_intents/pi_3N8x2ZKq... \
  -u pf_test_sk_51H8x...:
```

Если `status` равен `succeeded` — платёж прошёл. Чтобы не опрашивать API
вручную, подключите [вебхуки](webhooks.md): PayFlow сам пришлёт событие
`payment_intent.succeeded`.

## Дальше

- [Аутентификация](authentication.md) — как устроены ключи и идемпотентность
- [Вебхуки](webhooks.md) — асинхронные уведомления о статусах
- [Ошибки](errors.md) — полный справочник кодов
