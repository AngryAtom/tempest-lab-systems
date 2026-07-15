# Nextcloud Drop Folder To Media Library

This guide describes a self-hosted media-ingest workflow:

1. Upload media into a temporary Nextcloud-style drop folder.
2. Wait until uploads are old enough to avoid grabbing partial files.
3. Move completed media into a Jellyfin-style library path.
4. Rescan the source folder so the file-sync UI knows the items moved.
5. Let the media server detect and index the new library item.

The goal is convenience without keeping duplicate copies in cloud storage.

## Problem

Uploading media directly into a media server library is awkward from phones, desktops, and remote devices. A file-sync service is a nicer user interface, but it should not become the final media library.

The workflow needs to handle:

- Incomplete uploads.
- Loose movie files.
- Full movie folders.
- Duplicate titles.
- Extras, featurettes, trailers, and sample files.
- Nextcloud file-cache updates after files are moved out from under it.
- Clear logs when something was skipped or quarantined.

## Reference Layout

```text
/srv/media/
  Movies/
  TV/
  Music/
  Anime/
    Movies/
    Series/
```

Temporary drop folders:

```text
Files/
  Media-Movies/
  Media-TV/
  Media-Music/
  Media-Anime-Movies/
  Media-Anime-Series/
```

## Ingest Behavior

The ingest job runs on a timer and processes one pass at a time.

Core behaviors:

- Uses a lock file so concurrent runs do not step on each other.
- Waits for a minimum file age before processing.
- Moves completed top-level folders into the matching media library.
- Wraps loose movie files into a folder named after the file stem.
- Keeps drop folders owned by the file-sync service user.
- Normalizes imported media ownership and permissions.
- Rescans the file-sync source path after moving items.
- Removes duplicate source items only when the destination already exists and the largest media file hash matches.
- Quarantines non-identical conflicts instead of overwriting.
- Quarantines common extras folders for movie imports.

## Extras Handling

Movie rip folders often contain extras such as:

```text
Featurettes/
Extras/
Deleted Scenes/
Interviews/
Sample/
Samples/
Trailers/
```

For a clean first import, the ingest job moves those folders into quarantine and imports the main movie folder only.

This avoids a common failure mode where the media server indexes featurettes as separate movies or where a partial extras upload makes the whole folder look strange.

## Why The Safety Delay Matters

File-sync services can create folder skeletons before the large media file arrives. If automation immediately processes the folder, it may see:

```text
Movie Name/
  Featurettes/
```

but no actual movie file yet.

The ingest job should skip the item as too new until the upload has settled.

Example log shape:

```json
{"event":"skip_too_new","kind":"movies","path":".../Media-Movies/Movie Name","age_seconds":118,"min_age":600}
```

## Recovery: Drop Folder Looks Missing

One real failure mode: a drop folder was deleted during an upload retry, then recreated by automation with the wrong owner. The file-sync service could not see it correctly even though it existed on disk.

Recovery pattern:

```bash
sudo mkdir -p "/path/to/files/Media-Movies"
sudo chown file-sync-user:file-sync-group "/path/to/files/Media-Movies"
sudo chmod 775 "/path/to/files/Media-Movies"
docker exec --user file-sync-user nextcloud php occ files:scan --path="/example-user/files/Media-Movies" --no-interaction
```

The actual user, container name, and path should be adapted to the environment.

## Manual Run Button

A small desktop launcher can start the ingest job immediately:

```powershell
ssh homelab-node "sudo systemctl start media-ingest.service"
```

The manual trigger should still respect the upload safety delay by default. A force mode can exist, but it should only be used after verifying uploads are complete.

## Verification

Check the ingest logs:

```bash
sudo tail -50 /var/log/media-ingest.log
```

Check media storage:

```bash
find /srv/media/Movies -maxdepth 2 -type f
```

Check disk space:

```bash
df -h /srv/media /
```

Check the media server:

```bash
curl -I http://localhost:8096
```

## Lessons Learned

- A file-sync tool is a great upload interface, but it is not the media library.
- Moving files behind a file-sync app requires a rescan.
- Upload folders need service-compatible ownership.
- Extras should be handled intentionally.
- Automation should log every skip, move, quarantine, and duplicate decision.
- Recovery notes are as important as deployment notes.
