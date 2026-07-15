# Monitoring That Helps Instead Of Making Noise

Monitoring should answer one question quickly:

```text
What broke, and where should I look first?
```

A monitor that only says "down" can still waste time if it does not distinguish DNS, proxy, authentication, app health, and public-path problems.

## Monitoring Layers

| Layer | Example Check | What It Proves |
| --- | --- | --- |
| Container | `docker ps` / healthcheck | Process is running |
| Local app | `curl http://127.0.0.1:<port>/health` | App responds on host |
| Private proxy | `curl -k https://service.lab.example.internal/health` | DNS/proxy/internal TLS works |
| Public path | `curl https://service.example.com/health` | Public DNS/TLS/edge/origin works |
| User workflow | Login or synthetic transaction | User-facing behavior works |

## Internal Versus Public Monitors

For public services, keep separate monitors:

- Internal origin monitor.
- Public route monitor.

This makes triage faster:

| Origin | Public | Likely Area |
| --- | --- | --- |
| Up | Down | DNS, tunnel, NAT, public proxy, certificate |
| Down | Down | App, host, storage, container |
| Down | Up | Monitor bug or cached/public edge weirdness |

## Grace Windows

Some services restart slowly after power loss or updates. Avoid instant noisy alerts by using:

- Retry intervals.
- Confirmation counts.
- Reasonable timeout values.
- Maintenance windows during planned changes.

## Alert Routing

Not every alert deserves a phone notification.

| Severity | Example | Route |
| --- | --- | --- |
| Info | Planned restart completed | Dashboard/log only |
| Warning | Noncritical service degraded | Notification topic |
| Critical | Password vault down, public service broken, storage full | Phone alert |
| Security | Honeypot hit, suspicious web probes | Security alert topic |

## Post-Outage Checklist

After power loss or random host restart:

```bash
systemctl is-active docker
docker ps --format 'table {{.Names}}\t{{.Status}}'
df -h
```

Then check:

- DNS resolver.
- Reverse proxy.
- Password vault.
- File sync.
- Media server.
- Monitoring service itself.
- Public-edge services.

## Lessons Learned

- A green dashboard is only useful if it checks the right thing.
- Public services need both origin and public-path monitors.
- Management dashboards can load while their backend endpoint is broken.
- Alerts should be actionable, not just loud.

