# Placeholders

This fork changes how placeholders work. For the base concept, see the [upstream docs](https://agregarr.org). Trailer sources are covered in [Integrations](Integrations.md#tmdb-trailer-resolution).

## Fork additions

| Feature | What it does |
|---------|--------------|
| TV edition folders | New TV placeholders get their own `{edition-Trailer}` folder. On by default. See [below](#tv-placeholder-folders). |
| `.comingsoon` markers | Ownership proof written beside every placeholder. Cleanup only deletes proven files. See [below](#the-safety-model). |
| Download status checks | Sonarr/Radarr checked before creating or removing placeholders. See [below](#download-status). |
| `.plexmatch` files | Movie and TV placeholders get a `.plexmatch` with the title, year and TMDB ID (TV also gets the TVDB ID when known), to help Plex match them on the first scan. |
| Folder naming options | Optional TMDB ID in movie folder names and TVDB ID in TV folder names. See [below](#folder-naming). |
| Direct Plex deletion | Stale Plex entries deleted via the API instead of scan + empty trash. See [below](#direct-plex-deletion). |
| Retroactive filters | Placeholder filters (genre, year, country, language, keyword) re-evaluated during cleanup. Existing placeholders that no longer pass are removed. |
| Hub verification | After sync, filtered hubs are checked for placeholders missing the placeholder label. Missing labels are re-applied and logged. |
| Root folder filter | Limit candidates by Radarr/Sonarr root folder. See [Collections](Collections.md#coming-soon). |
| Watch state reset | Playing a placeholder trailer resets it to unwatched, for shared users too. See [below](#watch-state). |

Placeholder items use the Plex label `trailer-placeholder`. Filtered hubs use that label to leave them out.

---

## Placeholder folders

Set these in **Settings > Downloads**, under **Placeholder Root Folders**. Each movie and TV library has a **Placeholder Folder for** field, with the library name after "for". The path needs to match the Plex library path as the Agregarr container sees it. Pick the folder with **Browse**. To remove a library's folder, clear the field and save.

The **Placeholder Folders** health check warns if a folder isn't on a mounted volume. See [Health Checks](Health-Checks.md).

### Folder naming

The same section has two checkboxes for naming new folders. Both are off by default. Existing folders aren't renamed.

- **Add TMDB ID to movie placeholder folders**: new movie folders use `Title (Year) {tmdb-12345}`. This keeps two movies with the same title and year in separate folders.
- **Add TVDB ID to TV placeholder folders**: new folders for shows outside Sonarr use `Title (Year) {tvdb-12345}`. Shows already in Sonarr keep Sonarr's folder name.

Whatever the folder is called, the movie file inside is named `Title (Year) {tmdb-12345} {edition-Trailer}.mp4`.

---

## TV placeholder folders

Each new TV placeholder gets its own folder:

```
Show (Year) {edition-Trailer}/
  .plexmatch
  Season 00/
    S00E00.Trailer.mp4
    .comingsoon
```

If the show is in Sonarr, the folder name starts with Sonarr's folder name. Otherwise, it starts with `Title (Year)`, plus the TVDB ID when that option is on. Agregarr then adds the `{edition-Trailer}` suffix. This puts the trailer in a separate folder beside the real show's folder, rather than inside it.

Without editions, the file stays in `Show (Year)/Season 00/`. Shows already in Sonarr use Sonarr's folder.

### When editions apply

- **Plex 1.43.3 or newer.** Older versions keep the previous TV placeholder layout without the suffix.
- **Plex version known.** If Agregarr can't read the Plex version while creating placeholders, it skips TV placeholders for that run and tries again next time.
- **Not switched off.** Editions are on unless `"placeholderEditions": false` is set under `main` in `settings.json`. There isn't a web UI setting for this. Stop Agregarr before editing the file.

### The migration

Agregarr automatically moves placeholders created before editions to the new layout. The migration runs at the start of a full **Plex Collections Sync**, whether scheduled or started by hand. It runs once per Agregarr start when editions are on and Plex reports version 1.43.3 or newer. If Agregarr can't read the Plex version, it tries again during the next full sync.

For each tracked TV placeholder still using the old layout, it:

1. Creates `Show (Year) {edition-Trailer}/Season 00` beside the show's folder.
2. Moves `S00E00.Trailer.mp4` and `.comingsoon` into it. Anything else in the old Season 00 stays put, so real specials aren't touched.
3. Copies `.plexmatch` to the new folder.
4. Saves the new path and forgets the old Plex item. Agregarr asks Plex about the placeholder again during the next sync.

A show is skipped if the new folder already exists or Agregarr can't work out the placeholder root. If the run stops after moving the files, the next start finishes saving the new path. The old folder stays where it is.

!!! warning "The migration cannot be undone"
    Nothing in Agregarr moves placeholders back. Switching editions off later only changes where new placeholders are created. Releases before 2.13.0 don't know about the edition folders, so they'd leave them orphaned and may create duplicate placeholders. If you'd rather keep the old layout, skip the migration instead: stop Agregarr, set `"placeholderEditions": false` as above, then start the new version.

### When the real show arrives

Agregarr doesn't delete the trailer as soon as it sees the real show. First, it checks that every collection containing the trailer item also contains the real show. It removes the trailer item from those collections, then deletes the trailer files and stale Plex entry. If a collection doesn't contain the real show yet, cleanup waits until the next sync.

---

## The safety model

Placeholder files live inside real media libraries, so deletion has strict checks. **No proof of ownership, no delete.**

- Agregarr writes a `.comingsoon` marker beside every placeholder when it's created. The marker is JSON containing the title, year, TMDB ID, and creation time.
- Deletion needs one of three proofs: a database record, creation during the current sync, or a matching marker on disk. Agregarr skips and logs any file it can't prove belongs to it.
- Creation follows the same rule. If an unmarked file already exists at the target path, Agregarr treats it as real media. It won't overwrite, adopt, or manage the file.
- If Plex doesn't index a placeholder, the file remains on disk and discovery tries again next sync. Agregarr never deletes it just because it didn't appear.
- When Plex has indexed a placeholder but hasn't matched it to a TMDB ID, Agregarr keeps it for 24 hours from creation and checks again every sync. After 24 hours, it deletes the placeholder and won't recreate it for 7 days.
- Marked files without a database record get a 7-day grace window before the orphan sweep. The marker stores the grace period's start time, so restarting Agregarr doesn't reset it.
- If a placeholder item drops off its collection's source list, Agregarr removes it after 7 days. For released titles, that period starts on the release date. Otherwise, it starts when the placeholder was created.
- Every sync reads the full library for placeholders that have lost their database record. Agregarr adopts them again if they still belong to a collection's source. It never adopts one without a marker.
- File cleanup is skipped for that sync if a placeholder root is unreachable or unexpectedly empty while database records still exist.
- If one source in a multi-source collection breaks, placeholder cleanup for that collection is skipped during that sync.
- Database-tracked placeholders from before markers were added get markers automatically. Untracked files without markers are never deleted automatically. One log line per library lists the files Agregarr skipped.

---

## Download status

Before creating or removing placeholders, Agregarr checks Sonarr and Radarr:

- **Creation** skips anything Sonarr or Radarr reports as downloaded. This avoids creating a placeholder after the download finishes but before Plex scans it.
- **Discovery** treats either of these as proof that real content has arrived: Plex metadata showing real files, or the *arr reporting a completed download. Either one starts cleanup. Queued and still-downloading items don't, so the placeholder stays.

---

## Direct Plex deletion

The fork deletes stale placeholder entries directly through the Plex API.

Agregarr only deletes an entry when every file part sits under a configured placeholder root and no longer exists on disk. It refuses deletion if any part still exists, belongs to a merged item, sits outside the managed roots, or has a placeholder root that's empty or unreadable.

If deletion is refused, Agregarr asks Plex to scan that item's folder only. When the folder is outside the library paths Plex can see, such as when Agregarr and Plex mount the placeholder root at different paths, no scan is sent. The entry then waits for Plex's folder watcher or scheduled scan. The log records this. New placeholder folders outside Plex's library paths work the same way.

After a scoped scan, Agregarr may empty the whole library's trash when Plex's own "Empty trash automatically after every scan" setting is off and **Auto Empty Trash** is on. This also removes unrelated items already sitting in Plex's trash, so an unavailable mount can cause data loss. **Auto Empty Trash** is under **Settings > Plex** and is on by default. Turn it off to stop the purge. See [Settings](Settings.md#plex).

---

## Watch state

Placeholder trailers stay unwatched. Register the Plex webhook using the URL under **Plex Webhook** in **Settings > Downloads**. Plex Pass is required. When someone plays, stops, or marks a placeholder as watched, Agregarr marks it unplayed. On stop or watched events, it also clears the resume position. Agregarr does this for both the owner and the shared user who played it.
