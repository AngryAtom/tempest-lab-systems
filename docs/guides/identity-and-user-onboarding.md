# Identity And User Onboarding In A Homelab

Once a platform has real users, local accounts scattered across services become painful.

The target model is not "one login magically controls everything." The target model is one identity source that answers who a user is, what groups they belong to, and which front doors they can reach.

Each application may still own its internal permissions.

## Goals

- Central place for users and groups.
- MFA for admin, support, security, and lab access.
- Clear onboarding and offboarding.
- Break-glass accounts for recovery.
- Service-local roles where needed.
- Documentation that maps users to access without storing passwords.

## Reference Groups

| Group | Purpose |
| --- | --- |
| `lab-owners` | Full platform ownership |
| `lab-admins` | Trusted infrastructure administrators |
| `lab-support` | Can help users without owning the whole platform |
| `lab-users` | Standard users |
| `lab-media` | Media service access |
| `lab-cloud` | File-sync service access |
| `lab-soc` | Monitoring and security dashboards |
| `lab-research` | Research/OSINT tools |
| `lab-range` | Cyber range tools |
| `lab-disabled` | Offboarded users retained for audit notes |

Use groups instead of one-off exceptions whenever possible.

## Integration Models

| App Type | Best Integration |
| --- | --- |
| Supports OIDC | Use OIDC for login |
| Supports LDAP | Use LDAP bridge/outpost |
| No SSO support | Put behind forward-auth proxy if appropriate |
| Password manager | Be conservative; keep recovery/break-glass local until thoroughly tested |
| Remote desktop | Track devices/users separately; not usually a normal SSO app |

## Onboarding Workflow

1. Create identity record.
2. Assign groups based on access request.
3. Require MFA for elevated roles.
4. Create or link service-local account if the app needs one.
5. Add VPN/device access only when needed.
6. Store temporary setup secrets in a password manager, not docs.
7. Send the correct user guide.
8. Verify first login.
9. Record completion in access inventory.

## Offboarding Workflow

1. Disable identity record.
2. Remove service-local accounts not controlled by identity provider.
3. Remove VPN devices or ACL permissions.
4. Revoke remote support access.
5. Rotate shared credentials the user knew.
6. Remove password-manager collection access.
7. Record offboarding actions.

## Break-Glass Accounts

Do not lock yourself out by making SSO the only path into everything.

Keep break-glass access for:

- Identity provider admin.
- File sync admin.
- Media admin.
- Container host/admin.
- Password manager recovery.

Break-glass accounts should have:

- Strong unique passwords.
- MFA where supported.
- Recovery codes stored safely.
- Documented reactivation process.

## Lessons Learned

- SSO controls the front door; apps still control app-specific authorization.
- Password managers deserve extra caution.
- User onboarding needs documentation written for non-admins.
- Access inventory matters before you have many users, not after.

