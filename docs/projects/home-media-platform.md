# Home Media Platform Case Study

The media platform started as a simple goal: make personal media available on the devices people actually use.

That turned into a bigger engineering project than expected. The final shape includes a media server, clean service names, remote access, user accounts, public access planning, media import automation, metadata cleanup, monitoring, and recovery notes for when the platform behaves differently on phones, desktops, TVs, and remote networks.

## The Goal

The target experience is straightforward:

- Watch from a phone, desktop, tablet, or TV.
- Use the media server app where it makes sense.
- Support users without giving them admin access.
- Keep the library clean as new media is added.
- Make remote access reliable enough that it does not feel like a science project.
- Know quickly whether a problem is the media server, DNS, routing, proxy, storage, or the client device.

That last point became one of the biggest lessons. Streaming is not just “the app is up.” It is a chain.

## The Access Problem

The first version worked from the local network, then became confusing offsite.

Common failure modes:

- A phone could reach some lab services but not the media server.
- A service name resolved on one device but timed out on another.
- VPN-connected devices had DNS but not the right route.
- Public access introduced a second path that needed separate monitoring.
- Smart TVs were easier to support through the native app than through a browser.

The fix was to stop treating access as one thing.

The platform now separates:

- Local LAN access.
- VPN access.
- Public user-facing access.
- Admin-only management access.

Each path has different expectations, different risk, and different troubleshooting steps.

## The Library Problem

Getting media into the library was not as simple as dropping files into a folder.

Real imports include:

- Loose movie files.
- Full folders from disc rips.
- Featurettes and extras.
- Anime movies and series.
- TV seasons with naming quirks.
- Files arriving slowly through a sync client.
- Duplicate imports after a failed attempt.
- Bad rips that need to be removed and redone.

The important design decision was to use Veldora as a staging area instead of uploading directly into the Shuna media library.

The workflow:

1. Upload media into a Veldora file-sync drop folder.
2. Wait until uploads settle.
3. Move completed media into the right library.
4. Handle loose files, folders, duplicates, and extras.
5. Rescan the sync source so the upload UI stays accurate.
6. Refresh the Shuna media server library.
7. Log what happened.

That turned media import from a manual chore into an operational workflow.

Technical guides:

- [Veldora to Shuna media ingest workflow](../guides/nextcloud-drop-folder-to-media-library.md)
- [Veldora Nextcloud operations](../guides/veldora-nextcloud-operations.md)
- [Shuna Jellyfin operations](../guides/shuna-jellyfin-operations.md)
- [Shuna public edge and reverse proxy](../guides/shuna-public-edge-reverse-proxy.md)

## Metadata Lessons

Metadata problems are where media servers get messy.

Examples:

- TV episodes can appear as different shows when folder layout or naming is wrong.
- Extras can appear as standalone movies.
- Anime needs different metadata expectations than a normal movie library.
- A movie with broken audio is better removed and re-imported than endlessly patched around.
- A library can look wrong even when the files are technically present.

The platform handles this by separating media types into clearer Shuna libraries and treating imports as reviewable events instead of blind file moves.

## Automation Lessons

Automation helped, but only after it became cautious.

Useful behaviors:

- Refuse concurrent runs with a lock.
- Skip files that are too new.
- Quarantine conflicts instead of overwriting.
- Quarantine common extras folders for movie imports.
- Wrap loose movie files into a clean folder.
- Keep ownership and permissions consistent.
- Provide a manual desktop trigger for “run the import now.”
- Log every move, skip, duplicate, and quarantine decision.

The lesson: automation should make the normal path easy and the weird path recoverable.

## Monitoring Lessons

The media server needs more than one health check.

Useful checks include:

- Is the container running?
- Does the origin respond locally?
- Does the internal service name resolve?
- Does the reverse proxy route correctly?
- Does the public URL work from outside?
- Is disk usage close to danger?
- Are imports failing or quarantining too much?

The most useful monitor is the one that tells you where to look first.

## User Experience Lessons

The platform improved when it was tested like a user would actually use it:

- Try the phone off Wi-Fi.
- Try a TV app instead of assuming browser casting will work.
- Try a non-admin user.
- Try a remote user.
- Try uploading media from another computer.
- Try recovering after power loss.
- Try restarting only part of the stack.

Those tests exposed the gaps that normal “container is green” checks miss.

## What This Demonstrates

This project is worth showing because it covers several real operational skills:

- Service exposure design.
- DNS and reverse proxy troubleshooting.
- User access separation.
- Storage pressure management.
- File automation.
- Media metadata cleanup.
- Monitoring design.
- Recovery after failed imports and power events.

It is a media server, but the lessons are broader than media. It is a practical example of turning a hobby service into something closer to a supported platform.
