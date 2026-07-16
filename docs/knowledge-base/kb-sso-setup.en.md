# Setting up Single Sign-On (SSO)

**Who this is for:** administrators of a TaskFlow workspace on the
Business or Enterprise plan.

**What you'll get:** employees will sign in to TaskFlow through your
corporate identity provider (Okta, Azure AD, Google Workspace) instead of
a separate TaskFlow password.

## Before you start

- SSO is available on **Business** and **Enterprise** plans. On Free and
  Pro plans, the SSO menu item is hidden.
- You'll need admin rights in both TaskFlow and your identity provider
  (IdP).
- The supported protocol is **SAML 2.0**.

## Steps

1. In TaskFlow, go to **Workspace Settings → Security → Single Sign-On**.
2. Click **Set up SSO** — TaskFlow generates a **Metadata URL** and an
   **ACS URL**, which you'll need on the IdP side.
3. In your IdP, create a new SAML application and enter the values from
   step 2 (field names differ by provider — see the mapping table below).
4. In your IdP, configure the `email` attribute to be passed — TaskFlow
   matches users by email on their first SSO sign-in.
5. Back in TaskFlow, paste your **IdP's Metadata URL** (or upload the
   metadata XML file) into the **Identity Provider metadata** field.
6. Click **Send test sign-in** — TaskFlow opens a test SSO window. If it
   succeeds, click **Enable SSO for everyone**.

## Field mapping by provider

| TaskFlow | Okta | Azure AD |
|---|---|---|
| ACS URL | Single sign-on URL | Reply URL |
| Entity ID | Audience URI (SP Entity ID) | Identifier (Entity ID) |
| Metadata URL | — (enter Entity ID and SSO URL manually) | App Federation Metadata Url |

## What happens to existing passwords

Once SSO is enabled for everyone, regular TaskFlow password sign-in is
disabled for all users except the workspace owner — the owner always
keeps a password-based fallback in case of IdP issues.

## If something doesn't work

- **"Test sign-in" shows an `invalid_signature` error** — this usually
  means TaskFlow has outdated IdP metadata cached (e.g., after a
  certificate rotation on the IdP side). Re-upload the current metadata
  file.
- **User can sign in but gets "Access denied"** — check that the email in
  the SAML attributes matches the user's email in TaskFlow (case doesn't
  matter, but the domain must match).
- **SSO works for some employees but not others** — check whether those
  users have individual exceptions set under **Security → SSO
  exceptions**.

## Related articles

- [FAQ](kb-faq.en.md)
- [New employee onboarding](kb-onboarding.en.md)
