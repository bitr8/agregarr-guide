# Connect services

Agregarr connects to other apps for three jobs: reading and writing your Plex library, pulling lists and statistics for collections and grabbing items you don't own yet. This section covers each connection and its settings.

Plex is the only required connection. Everything else is optional and only matters for the features that use it.

The setup wizard handles the first connections. If you haven't run it yet, see [First run: the setup wizard](../get-started/setup-wizard.md).

## Plex

Required. Agregarr reads your movie and TV libraries from Plex, then builds collections, labels, sort titles and posters in it.

- [Plex](plex.md): the **Settings > Plex** fields, the libraries Agregarr sees and everything Agregarr writes to your server.
- [Reverse proxy, API key and debugging export](reverse-proxy-api.md): running Agregarr behind a reverse proxy, the API key, the `/api-docs` page and **Export Debugging Information**.

## Where collections get their titles

These connections live under **Settings > Sources**. They're all optional.

- TMDB, IMDb, Letterboxd and AniList need no setup.
- Trakt, MDBList, MyAnimeList, Tautulli and Tracearr each need a key or server address. The [source pages](../collections/index.md#reference) say what each source needs.
- Networks Top 10 collections need a Cloudflare solver. See [Cloudflare Solver](../services/cloudflare-solvers.md).
- [Tracearr](../services/tautulli-tracearr.md#connect-tracearr) can stand in for Tautulli.
- [Maintainerr](../services/maintainerr.md) feeds deletion countdowns to poster overlays. It doesn't build collections.

## Grabbing missing items

These connections live under **Settings > Downloads**. They're all optional.

You only need Seerr (recommended) or Radarr and Sonarr to grab missing items. Seerr Requests collections also need Seerr. It's connected here, not under **Settings > Sources**.

- [Seerr](../services/seerr.md) for requesting items you don't own.
- **Radarr** and **Sonarr** servers for adding them directly. Each server needs its address, port and API key (in Radarr or Sonarr, under Settings > General).
- **Placeholder Root Folders** and the trailer settings, covered by [Placeholders](../Placeholders.md) and [TMDB Trailer Resolution](../services/trailers.md#what-agregarr-uses-trailers-for).
- **Plex Webhook**, covered under [Placeholders](../Placeholders.md#watch-state).
- **Plex Watchlist Sync**, which needs Seerr connected first. It adds watchlisted items to your Radarr and Sonarr servers.
<!-- OPEN: in flux: Plex webhook, documented separately once that work lands -->

## Related

- [Collections](../collections/index.md)
- [Finding your way around](../get-started/finding-your-way.md)
