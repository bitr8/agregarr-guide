# Performance

Agregarr keeps syncs quick by not writing to Plex when nothing has changed. The defaults work well for most libraries. Read this if a sync feels slow or you want to check what it changed. The other settings mentioned here are covered in [Settings](Settings.md).

## Writes to Plex

Before writing anything, Agregarr checks what Plex already has.

- **Collection details.** Agregarr only writes a collection's title, sort title, content sort order, and Home or Recommended visibility when the current Plex value is different. `skipUnchangedPlexWrites` under `main` in `settings.json` controls this. It's on unless you set it to `false`, and there isn't a screen for it. Only set it to `false` if you think Plex has drifted from Agregarr and you want every value written again. Stop Agregarr before editing the file.
- **Labels.** If a collection already has its Agregarr label, it's left alone.
- **Collection posters.** Agregarr only regenerates a poster when one of its inputs changes: the poster template, the items shown, the collection name, or the collection type. If nothing changed and Plex still has the right poster, Agregarr skips the whole poster step. The log shows `Poster unchanged, skipping regeneration and reapplication`.
- **Overlays.** Agregarr checks each item's overlay against the last applied version before downloading the poster. If the overlay inputs haven't changed, it skips that item. The **Poster Overlay Application** job description says the same thing: every item is checked again, and unchanged ones are skipped.

## Seeing what a sync did

Each full Collections Sync ends with two lines in the log:

- `Plex writes this sync: N (title: 0, sortTitle: 0, ...)` shows the number of writes, split by kind. After a sync with no changes, this should be low or zero.
- `Sync phase timings:` shows how long the source fetch, content update, metadata update, hub sync, and ordering stages took in milliseconds.

Set **Log Level** to **Info** or lower to see these lines. See [Settings](Settings.md#log-level).

## Sync schedule

By default, **Plex Collections Sync** runs every 12 hours and **Collections Quick Sync** runs every 30 minutes. Quick sync only adds newly downloaded items to collections they're missing from and removes their placeholders.

**Poster Overlay Application** runs daily. **Overlay Quick Sync** runs every 30 minutes and only checks items added to Plex since its last run that don't have an overlay yet.

To change a frequency, use **Edit** on **Settings > Jobs**. The full list is in [Settings](Settings.md#jobs).

## Overlays

- **Only the data your templates use.** Agregarr only fetches IMDb ratings and TMDB release dates for a library when one of its templates uses them. Otherwise, the log says the fetch was skipped.
- **Batched Plex reads.** Agregarr asks Plex for item details in groups of 50 rather than one at a time. If a group breaks, it's retried once, then split in half and tried again. One bad item doesn't take out the rest.
- **Parallel Items.** **Parallel Items** in **Settings > General** controls how many items Agregarr processes at once, from 1 to 10. The default is 1. Higher values are faster but use more memory, so bump it up one step at a time if overlay runs are slow.

## Caches

Agregarr caches lookups so it doesn't fetch the same data on every sync. Some of the main caches are listed below. Unless noted, they're kept in memory and start empty after a restart.

Caches for external APIs and list data appear under **Cache** on **Settings > Jobs**. Each one has a **Flush Cache** button.

| What | How long |
|------|----------|
| IMDb and Rotten Tomatoes ratings, and TMDB release dates | Depends on the title's age (see below) |
| Pages fetched through a Cloudflare solver | 5 minutes |
| Tautulli dashboard statistics, Tracearr statistics | 5 minutes |
| Letterboxd title matches (kept in the database, so they survive restarts) | 1 to 30 days (see below) |

### Ratings and release dates

Newer titles change more often, so Agregarr keeps them for less time. With the default maximum of 30 days:

| Title | Kept for |
|-------|----------|
| Released this year | 12 hours |
| Released last year | 3 days |
| 2 to 9 years old | 7 days |
| 10 or more years old | 30 days |
| Release year unknown | 3 days |

A title with a release date within 3 days of today is refreshed at least every 2 hours. Within 7 days, it's refreshed at least every 4 hours because TMDB dates can move. If a lookup finds no rating, Agregarr remembers that for less time, between 2 and 24 hours, so it can try again soon.

For a show with an upcoming episode, the cache time depends on how close that episode is. It's 3 days when the episode is more than 14 days away, 24 hours at 7 to 14 days, 12 hours at 1 to 7 days, and 6 hours inside a day. If the episode has aired and TMDB hasn't moved on yet, Agregarr tries again after 6 hours. Ended or cancelled shows are kept for 7 days. Other shows with nothing upcoming are kept for 24 hours.

The 30 day maximum comes from `ratingsCacheMaxDays` under `main` in `settings.json`. The age-based times above scale with that value. There isn't a screen for it. Stop Agregarr before editing the file.

### Letterboxd matches

Letterboxd lists provide titles and years. Agregarr remembers the TMDB title matched to each one, so later syncs don't need to search again.

A confident match is kept for 30 days. A looser match stays around for 7 or 14 days, and a weak match for 3 days. An unmatched title is remembered for 1 day, or 7 days when the title is more than 2 years old.

By default, Letterboxd pages are fetched over plain HTTP, which is the fastest route. See [Integrations](services/cloudflare-solvers.md#use-cloudflare-solver-for-letterboxd).

## Placeholders

- **Skip YouTube Trailer Downloads** on **Settings > Downloads** makes placeholder creation much faster by using the built-in video instead of fetching trailers. See [Integrations](services/trailers.md#what-agregarr-uses-trailers-for).
- If Plex can't match a placeholder, Agregarr won't recreate it for 7 days. That stops the same trailers being downloaded on every sync. See [Placeholders](Placeholders.md#the-safety-model).
