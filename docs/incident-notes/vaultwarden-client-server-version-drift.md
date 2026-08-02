# Password Manager Extension Break From Client/Server Version Drift

## Symptom

A browser password-manager extension could authenticate to a self-hosted vault service, but the extension UI never finished loading the vault list.

The confusing part was that the normal server checks looked healthy:

- Web login worked.
- The extension could authenticate.
- Server logs showed successful sync requests.
- The sync endpoint returned HTTP `200`.
- The same behavior appeared on more than one desktop.

The UI still showed loading placeholders instead of usable vault items.

## Impact

The vault was available through the web app, but browser autofill and extension-based password lookup were effectively unusable.

For a self-hosted platform, that is a high-friction failure because the password manager is part of the recovery path for other services.

## Root Cause

The browser extension had updated ahead of the self-hosted server.

The server was still pinned to an older Vaultwarden release while the browser extension had moved to a newer Bitwarden client version. The API still returned successful responses, but the client and server were no longer compatible enough for the extension UI to render the vault.

This was easy to misread as:

- Browser cache corruption.
- Certificate trust failure.
- Bad reverse-proxy configuration.
- Stale self-hosted URL settings.
- Broken sync state.

Those were reasonable things to check, but the cross-device behavior pointed back to server/client compatibility.

## Diagnosis

Useful checks:

```bash
docker ps --filter name=vaultwarden
docker exec vaultwarden /vaultwarden --version
docker logs vaultwarden --since 30m
```

Client-side checks:

- Confirm the extension is pointed at the correct self-hosted URL.
- Fully log out and back in, not just lock/unlock.
- Test from a second browser or desktop.
- Compare the installed extension version with the self-hosted server version.

Server-side checks:

```bash
curl -sS https://vault.example.local/api/config
curl -sS https://vault.example.local/alive
```

The key clue was that sync returned success while the extension UI still failed on multiple machines.

## Fix

Upgrade the self-hosted server to a release compatible with the newer browser extension.

Safe pattern:

1. Back up the live database using the database engine's backup command.
2. Back up the compose file and any custom image build file.
3. Pin the new server image or digest deliberately.
4. Recreate only the vault container.
5. Verify server version, health endpoint, and config endpoint.
6. Log out and back into the browser extension.
7. Confirm vault items render and autofill works.

## Verification

Expected after the upgrade:

```bash
docker exec vaultwarden /vaultwarden --version
curl -sS https://vault.example.local/api/config
curl -sS https://vault.example.local/alive
```

The browser extension should:

- Authenticate normally.
- Complete sync.
- Render vault items.
- Search and autofill entries.

## Prevention

- Include client/server version checks in the password-manager runbook.
- Treat browser updates as possible extension updates.
- Do not assume HTTP `200` from a sync endpoint means the client can actually use the response.
- When the same extension symptom appears on multiple desktops, check server compatibility early.
- Keep the password manager updated in planned maintenance windows because it supports recovery for the rest of the platform.

## Lesson Learned

Self-hosted services are not isolated from client release cycles.

When a browser, mobile app, or desktop client updates automatically, the server may need to move with it. Monitoring can prove the service is alive, but compatibility checks prove the service is still usable.
