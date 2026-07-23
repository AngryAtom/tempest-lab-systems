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
| SIEM | Security telemetry and analyst dashboard | Private | Collects host, Docker, proxy, honeypot, and custom telemetry. |
| Cyber range | Contained practice targets and tools | Private lab only | Legal, isolated, nondestructive practice. |
| OSINT desk | Investigation workflow tooling | Private | Must stay legal and avoid account intrusion. |

## Example Service Entries

These examples use sanitized Tempest service names. Replace hostnames, paths, user IDs, and network ranges with values from your own environment.

| Service | Role | Access Model | Example Name | Key Operations |
| --- | --- | --- | --- | --- |
| Veldora | File sync and media drop folders | Private/VPN user access | `files.lab.example.internal` | Upload staging, ownership repair, `occ files:scan`, quota review. |
| Shuna | Media streaming and library indexing | Private and optional hardened user edge | `media.lab.example.internal` | Library scan, metadata review, user access, client validation. |
| Media ingest job | Automation bridge between Veldora and Shuna | Host-local service | `media-ingest.service` | Settled-upload checks, moves, duplicate detection, quarantine, logging. |
| Dedicated SOC node | SIEM manager, indexer, and dashboard | Private/VPN admin access | `siem.lab.example.internal` | Agent enrollment, event search, dashboard/API checks, index storage review. |
| Endpoint agent | Host and container telemetry forwarder | Host-local agent to private SIEM | `wazuh-agent` | Auth logs, system logs, Docker events, proxy logs, and custom lab telemetry. |

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

## Related

- [Telemetry and SIEM architecture](telemetry-and-siem-architecture.md)
- [Building a homelab SIEM with Wazuh](../guides/homelab-siem-with-wazuh.md)
