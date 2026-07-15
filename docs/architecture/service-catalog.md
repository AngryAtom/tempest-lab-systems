# Service Catalog

This catalog describes the major service roles in the lab and how they are operated.

| Category | Service Role | Access Model | Notes |
| --- | --- | --- | --- |
| Password management | Self-hosted vault | Private only | Never public-facing in this model. |
| Media streaming | Media library | Private and optional public edge | Public edge requires hardening, monitoring, and least-privilege users. |
| File sync | Private cloud storage | Private by default | Also used as temporary media drop zone. |
| DNS filtering | Private DNS and rewrites | LAN/VPN only | Supports clean internal service names. |
| Reverse proxy | HTTPS routing | LAN/VPN/public as needed | Split public/private routes by service risk. |
| Monitoring | Service uptime checks | Private | Monitors internal origin and public paths separately. |
| Container management | Docker admin UI | Private only | Requires Docker socket access; document stale socket recovery. |
| Identity | SSO/MFA/user lifecycle | Private admin, selected user flows | Centralize onboarding where services support it. |
| Remote desktop | Self-hosted relay/ID server | VPN preferred | Useful for support, not an identity system by itself. |
| Notifications | Alert delivery | Private | Used for serious service/security alerts. |
| Cyber range | Contained practice targets and tools | Private lab only | Legal, isolated, nondestructive practice. |
| OSINT desk | Investigation workflow tooling | Private | Must stay legal and avoid account intrusion. |

## Access Matrix Template

| Service | Admin Access | Standard User Access | Public Exposure | Monitor |
| --- | --- | --- | --- | --- |
| `<service>` | VPN/admin group | Optional user group | No/Yes | Internal + external if public |

## Documentation Required For Each Service

- Purpose.
- Data location.
- Deployment path.
- Authentication model.
- DNS name.
- Reverse proxy route.
- Monitoring check.
- Backup/recovery notes.
- Security limitations.
- Known failure modes.
