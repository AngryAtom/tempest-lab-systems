# Portainer Local Endpoint Down After Docker Restart

## Symptom

The Portainer web UI loaded, but the local Docker endpoint showed `Down` and could not be opened.

Browser console errors included messages like:

```text
Dial unix /var/run/docker.sock: connect: connection refused
```

## Impact

The container management UI could not inspect or manage local Docker resources even though containers were still running.

## Root Cause

Docker had restarted, but the Portainer container had remained running from before that restart. Portainer was still holding a stale bind to the Docker socket.

The host Docker socket itself was healthy.

## Diagnosis

Check Portainer:

```bash
docker ps --filter name=portainer
docker inspect portainer --format 'Started={{.State.StartedAt}} Status={{.State.Status}}'
```

Check Docker:

```bash
systemctl status docker --no-pager
curl --unix-socket /var/run/docker.sock http://localhost/_ping
```

Expected Docker socket result:

```text
OK
```

If Docker is healthy but Portainer reports the local endpoint down, the Portainer container may need to be restarted so the bind mount is refreshed.

## Fix

Restart only Portainer:

```bash
docker restart portainer
```

## Verification

```bash
docker ps --filter name=portainer
curl -k -sS -o /dev/null -w 'https_9443=%{http_code}\n' https://127.0.0.1:9443/
curl --unix-socket /var/run/docker.sock http://localhost/_ping
```

Expected:

```text
https_9443=200
OK
```

Refresh the Portainer UI and confirm the local endpoint reconnects.

## Prevention

- Include Portainer endpoint health in routine post-Docker-restart checks.
- Consider a small monitor that checks whether Portainer can query the local Docker endpoint, not just whether the Portainer web page loads.
- Keep the recovery command in the operations runbook.

