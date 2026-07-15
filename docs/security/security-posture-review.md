# Security Posture Review

This review summarizes the security posture model used for the lab.

## Security Goals

- Keep administrative services private.
- Require authentication on user-facing services.
- Use MFA where supported.
- Separate public exposure from internal origin services.
- Monitor both internal and public paths.
- Document rollback paths before making major changes.
- Keep credentials out of documentation and scripts.

## Public Exposure Rules

| Service Type | Public Exposure |
| --- | --- |
| Password vault | No |
| Container administration | No |
| DNS admin | No |
| Monitoring admin | No |
| Identity admin | No |
| Media user portal | Possible with hardening |
| Public portfolio/blog | Yes |
| Demo apps | Yes, if isolated |

## Practical Controls

- Private DNS for management surfaces.
- Reverse proxy allowlists or identity gates where appropriate.
- Service-specific users with least privilege.
- Separate admin accounts from standard user accounts.
- Rate limiting or fail2ban-style protection on public services.
- Regular review of exposed ports.
- Post-change health checks.

## Known Tradeoffs

Homelabs are learning environments. Some controls may start simple and mature over time. The important practice is to document the current risk, compensating controls, and the next improvement.

## Review Cadence

- After public exposure changes.
- After identity/provider changes.
- After Docker or firewall changes.
- After power events or unexpected restarts.
- Before onboarding new users.
