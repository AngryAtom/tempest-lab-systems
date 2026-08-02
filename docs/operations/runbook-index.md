# Operations Runbook Index

This index collects recovery notes and operational checklists from the lab.

## Recovery Notes

- [Portainer local endpoint down after Docker restart](../incident-notes/portainer-stale-docker-socket.md)
- [Password manager extension break from client/server version drift](../incident-notes/vaultwarden-client-server-version-drift.md)

## Operational Checklists

### Service Health Check Routine

Use this when a hosted service appears unavailable:

1. Check whether the container or service process is running.
2. Check whether the origin responds locally.
3. Check whether the reverse proxy can reach the origin.
4. Check whether private DNS resolves to the expected route.
5. Check whether the public route, if any, fails differently than the private route.
6. Check recent logs for restarts, permission errors, storage pressure, or failed upstream connections.
7. Record the fix in the private runbook before closing the issue.

### Public Edge Rollback

Use this when a public-facing service behaves unexpectedly:

1. Disable or pause the public route.
2. Confirm the private origin still works.
3. Review authentication, rate limiting, proxy headers, and upstream health.
4. Restore the public path only after the private and public monitors agree.
5. Document the trigger, impact, fix, and any follow-up hardening.

### User Onboarding

Use this when adding a new user:

1. Create the identity record or service-local account.
2. Assign the minimum required group or role.
3. Enable MFA where supported.
4. Store recovery notes in the private password manager or operations notebook.
5. Validate login from the user's expected device path.
6. Record what services the user can access.

### Post-Outage Validation

Use this after power loss, host restart, or network changes:

1. Confirm the container runtime is healthy.
2. Confirm critical containers restarted.
3. Confirm DNS, reverse proxy, monitoring, and notifications are working.
4. Confirm user-facing services are reachable from their expected access paths.
5. Review storage pressure before resuming media or file automation.
6. Capture any service that required manual recovery.

### Client/Server Compatibility Check

Use this when a browser extension, mobile app, or desktop client can log in but behaves strangely:

1. Confirm the service web UI and API health endpoint work.
2. Confirm the client is pointed at the correct self-hosted URL.
3. Check the server version.
4. Check the client or extension version.
5. Search release notes or known issues for that version pair.
6. Test from a second browser or device.
7. Upgrade the server in a backed-up maintenance window if the failure follows the client across devices.
