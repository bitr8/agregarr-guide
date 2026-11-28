# Placeholders

A placeholder is a short trailer file that Agregarr puts in your Plex library for something you don't have yet. It lets a collection show the item, with an overlay such as a release date, before the real file arrives. When the real movie or show turns up, Agregarr removes the placeholder.

Trailer sources are covered in [Integrations](services/trailers.md#what-agregarr-uses-trailers-for).

## Fork additions

| Feature | What it does |
|---------|--------------|
| TV edition folders | New TV placeholders get their own `{edition-Trailer}` folder. On by default. See [below](#tv-placeholder-folders). |
| `.comingsoon` markers | Ownership proof written beside every placeholder. Cleanup only deletes proven files. See [below](#the-safety-model). |
| Download status checks | Sonarr/Radarr checked before creating or removing placeholders. See [below](#download-status). |
| `.plexmatch` files | Movie and TV placeholders get a `.plexmatch` with the title, year and TMDB ID (TV also gets the TVDB ID when known), to help Plex match them on the first scan. |
| Folder naming options | Optional TMDB ID in movie folder names and TVDB ID in TV folder names. See [below](#folder-naming). |
| Direct Plex deletion | Stale Plex entries deleted via the API instead of scan + empty trash. See [below](#direct-plex-deletion). |
| Retroactive filters | Placeholder filters (genre, year, country, language, keyword) re-evaluated during cleanup. Existing placeholders that no longer pass are removed. See [below](#placeholder-filters). |
| Hub verification | After sync, filtered hubs are checked for placeholders missing the placeholder label. Missing labels are re-applied and logged. |
| Root folder filter | Limit candidates by Radarr/Sonarr root folder. See [Collections](collections/sources/coming-soon.md#coming-soon). |
| Watch state reset | Playing a placeholder trailer resets it to unwatched, for shared users too. See [below](#watch-state). |
| Seerr ignore patterns | Keeps Seerr from counting placeholders as available. See [below](#seerr). |

---

## Turn it on

1. Set a placeholder folder for the library. See [Placeholder folders](#placeholder-folders).
2. Open a collection and tick **Create placeholders for missing items** under **Placeholder Creation**.
3. Set **Days Ahead** and **Released Items** (see [Release windows](#release-windows)), and any **Placeholder Filters**.
4. Save. Placeholders are created on the next collection sync.

**Placeholder Creation** appears on most collection types. It isn't offered for Seerr, Tautulli, Filtered Plex Hub, or the automatic actor and director collections. **Coming Soon** collections always create placeholders: the box is ticked and can't be cleared. **Days Ahead** is shown for every Coming Soon collection.

If the library has no placeholder folder, the form shows **Placeholder Root Folders Required** with a link to **Settings > Downloads**. If YouTube cookies aren't set up, it warns that trailer downloads may fail. **Skip YouTube Trailer Downloads** in **Settings > Downloads** uses a generic video for every placeholder instead.

### Which items get one

A collection creates a placeholder for each item on its source list that isn't in the Plex library, as long as:

- the item's release date falls inside the [release windows](#release-windows),
- it passes the [placeholder filters](#placeholder-filters),
- Sonarr or Radarr doesn't already report it as downloaded (see [Download status](#download-status)),
- there's room in the collection. When **Max Items** is set and the collection is sorted by anything other than the source's own order, placeholders only fill the slots left after real items. With the source order, a missing item keeps its place in the list.

Items with no known release date don't get a placeholder.

**Bulk Edit** has a **Create Placeholders** column for switching this on or off across several collections at once.

---

## Release windows

Two settings decide which release dates qualify. Both sit under **Placeholder Creation**.

- **Days Ahead**: placeholders are made for items releasing within this many days from today. New collections start at 90. The lowest value is 1.
- **Released Items** has two choices:
    - **Include all released items**: any item already released qualifies, however long ago. This is the default.
    - **Only include items released within** _N_ **days**: only items released in the last _N_ days qualify. New collections start at 14. Set 0 to make placeholders only for items that haven't released yet, or release today.

For movies, the release date is the earlier of the digital and physical dates. When TMDB only has a cinema date, Agregarr estimates the release as the cinema date plus 90 days. Coming Soon and Multiple Sources collections also offer **Skip items with an estimated release date**, which leaves those estimated movies out. See [Collections](collections/sources/coming-soon.md#skip-items-with-an-estimated-release-date). For TV, the date is the air date of the next season, or the show's first air date for a new show. For a new show that Sonarr has, Agregarr uses Sonarr's air time for episode 1, so the date is the day it airs in your time zone rather than TMDB's date, which is the day the network aired it.

These windows only decide which items get a new placeholder. An existing placeholder isn't removed because its release date moved outside the window. It's removed when the real item arrives, when it drops off the source list, or when it fails the filters. See [Cleanup](#cleanup).

**Include mid-season episodes (TV)** on Coming Soon collections lists shows whose next episode falls inside **Days Ahead**. Those shows must already be in Plex, so no placeholder is made for them. See [Collections](collections/sources/coming-soon.md#include-mid-season-episodes-tv).

---

## Placeholder filters

**Placeholder Filters** is a collapsible section under **Placeholder Creation**. It's optional. Leave it empty to make placeholders for every missing item.

- **Minimum release year**
- **Minimum IMDb rating**. Items without an IMDb rating pass.
- **Minimum Rotten Tomatoes rating**
- **Minimum RT audience rating**
- **Genre Filter**, **Country Filter**, **Language Filter** and **Keyword Filter**, each with an include or exclude toggle.

A value of 0 means no limit.

Changing the year, genre, country, language or keyword filters also applies to placeholders that already exist. On the next sync, any placeholder that no longer passes is removed. Rating filters only apply when a placeholder is created, because unreleased titles usually have no ratings yet.

---

## Placeholder folders

Set these in **Settings > Downloads**, under **Placeholder Root Folders**. Each movie and TV library has a **Placeholder Folder for** field, with the library name after "for". The path needs to match the Plex library path as the Agregarr container sees it. Pick the folder with **Browse**. To remove a library's folder, clear the field and save.

The **Placeholder Folders** health check warns if a folder isn't on a mounted volume. See [Health Checks](Health-Checks.md).

### Folder naming

The same section has two checkboxes for naming new folders. Both are off by default. Existing folders aren't renamed.

- **Add TMDB ID to movie placeholder folders**: new movie folders use `Title (Year) {tmdb-12345}`. This keeps two movies with the same title and year in separate folders.
- **Add TVDB ID to TV placeholder folders**: new folders for shows outside Sonarr use `Title (Year) {tvdb-12345}`. Shows already in Sonarr keep Sonarr's folder name.

Whatever the folder is called, the movie file inside is named `Title (Year) {tmdb-12345} {edition-Trailer}.mp4`. Plex shows it as a **Trailer** edition of the movie.

---

## TV placeholder folders

TV has no file-level editions, so a TV placeholder is a single special episode, `S00E00.Trailer.mp4`, in a `Season 00` folder. Plex shows it as a **Specials** season with one episode, which Agregarr titles **Trailer (Placeholder)**.

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

---

## Labels and hubs

Every placeholder gets the Plex label `trailer-placeholder`. For TV, the label goes on the show. Plex may display it as `Trailer-placeholder`; the match isn't case-sensitive.

Agregarr uses the label to keep placeholders out of places they don't belong:

- **Filtered Plex Hubs** leave out anything with the label. Plex's own **Recently Added** hubs don't, so turn those off and use a filtered hub instead. The collections page warns when a library has placeholder collections and a default hub still switched on. See [Collections](collections/sources/your-servers.md#filtered-plex-hub).
- The automatic actor and director collections, and label-based collections, never include placeholders.
- For any other collection, add `trailer-placeholder` under **Label Exclusion**. See [Collections](collections/index.md).

After each sync, Agregarr checks every filtered hub. A placeholder found in one is missing its label, so Agregarr adds the label again and logs it.

---

## Overlays on placeholders

Placeholders are ordinary Plex items, so overlay templates apply to them. Templates can test **Is Placeholder (Coming Soon)** in their conditions. Despite the name, it's true for placeholders from any collection. Two presets use it:

- **Trailer**: a full-width **TRAILER** banner on every placeholder.
- **Placeholder Overlay**: darkens every placeholder poster by 25%.

Agregarr recognises a placeholder by its edition: a movie or show whose edition title contains "Trailer", "Placeholder" or "Coming Soon". A TV placeholder's episode is recognised by its title, **Trailer (Placeholder)**. If Sonarr or Radarr reports the item as downloaded, it's never treated as a placeholder.

Coming Soon collections always apply overlays straight after the collection sync. Other collections do this when **Apply item overlays during sync** is ticked. See [Collections](collections/form/artwork.md#apply-item-overlays-during-sync). If placeholders and overlays are both on but the library has no overlay templates, the form warns that placeholders will be created without status overlays.

### The Specials season

A TV placeholder's only season is the **Specials** season holding the trailer. Plex doesn't give that season a poster of its own, and season overlays always start from the season's Plex poster. When season artwork is a target, Agregarr skips the Specials season of a placeholder show unless you've uploaded a poster for it. Skipped seasons aren't reported as errors. If you upload a poster for that Specials season in Plex, it gets season overlays like any other season.

---

## Cleanup

A placeholder is removed when one of these happens:

1. **The real item arrives.** Agregarr deletes the placeholder once Plex has the real file, or once Sonarr or Radarr reports the download complete. See [Download status](#download-status). For TV edition placeholders, see [When the real show arrives](#when-the-real-show-arrives).
2. **It drops off the source list.** Agregarr keeps it for 7 days. For released titles, that period starts on the release date. Otherwise, it starts when the placeholder was created.
3. **It fails the current filters.** See [Placeholder filters](#placeholder-filters).
4. **Placeholders are switched off, or the collection is deleted.** Agregarr forgets the collection's placeholders straight away. The files then go through the 7-day [orphan grace window](#the-safety-model) before they're deleted, unless another collection still wants them.

Removal deletes the file and marker, takes the item out of Plex, and removes the database record. See [Direct Plex deletion](#direct-plex-deletion).

Full syncs and **Plex Collections Quick Sync** both look for real content. Quick sync handles items Plex added recently: it adds them to the collections they were missing from, then removes their placeholders.

### When the real show arrives

Agregarr doesn't delete a TV edition placeholder as soon as it sees the real show. First, it checks that every collection containing the trailer item also contains the real show. It removes the trailer item from those collections, then deletes the trailer files and stale Plex entry. If a collection doesn't contain the real show yet, cleanup waits until the next sync. Quick sync follows the same rule.

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

## Seerr

Seerr sees placeholder files as real media and marks the title as available, so nobody can request it. Turn on **Keep placeholder ignore patterns in Seerr** in the Seerr connection window. Agregarr adds two patterns to Seerr's **Ignored Path Patterns**, one for the movie file (`{edition-Trailer}.mp4`) and one for the TV file (`Season 00/S00E00.Trailer.mp4`), and puts them back on each full collection sync if they go missing. This needs a Seerr build that has that setting. See [Integrations](services/seerr.md).

---

## Watch state

Placeholder trailers stay unwatched. Register the Plex webhook using the URL under **Plex Webhook** in **Settings > Downloads**. Plex Pass is required. When someone plays a placeholder, or finishes enough of it for Plex to count it as watched, Agregarr marks it unplayed. When playback stops or the item is counted as watched, it also clears the resume position. Agregarr does this for both the owner and the shared user who played it. Only items Agregarr knows are its placeholders are reset.

The webhook can't stop scrobbles that Plex has already sent to other services. Exclude placeholder items in your scrobbler's settings for full coverage.
