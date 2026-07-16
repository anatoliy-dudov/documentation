# New employee onboarding

**Who this is for:** TaskFlow workspace administrators onboarding a new
employee.

**What you'll get:** the new employee gets access to the right projects
with the right role on day one, without extra "just in case" permissions.

## Before you invite: decide on a role

| Role | When to assign it |
|---|---|
| Workspace Administrator | A manager responsible for billing and security settings. Usually 1–2 people per workspace |
| Member | A full-time employee who works with projects daily |
| Guest | A contractor or employee who only needs access to one specific project |

!!! tip "Default rule of thumb"
    When in doubt, assign **Guest** with access to a single project rather
    than Member with access to everything. It's easier to expand access
    later than to later figure out why a departed contractor still has
    access to a finance project.

## Steps

1. **Workspace Settings → Members → Invite.** Enter the employee's work
   email — if your company has SSO enabled (see
   [Setting up Single Sign-On](kb-sso-setup.en.md)), the email domain must
   match the domain configured for SSO.
2. **Choose a role** according to the table above.
3. **Add them to the relevant projects.** A workspace invitation by itself
   grants no access to any project — project access is granted separately,
   even for the Member role.
4. **Set up notifications.** By default, a new member is subscribed to
   notifications for every project they have access to — if that's too
   much, turn some off right away instead of having to retrain the
   employee to unsubscribe later.
5. **Send the welcome guide link.** TaskFlow automatically emails an
   invitation to the new member; it helps to also share it in your
   company chat along with a link to your internal TaskFlow usage
   guidelines, if you have one.

## After a week

Check **Workspace Settings → Members → Activity**: if the new employee
has never logged in, it usually means the invitation email landed in
spam rather than that they forgot about TaskFlow — it's worth messaging
them directly with a sign-in link.

## When an employee leaves

Use a separate offboarding runbook (revoking access, reassigning tasks) —
it's intentionally not covered here, so onboarding and offboarding — which
have different audiences and different urgency — aren't mixed into a
single article.

## Related articles

- [Setting up Single Sign-On (SSO)](kb-sso-setup.en.md)
- [FAQ](kb-faq.en.md)
