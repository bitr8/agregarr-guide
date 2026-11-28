# Choose a source

A source decides where a collection gets its titles. Match the row you want to the source below, then check what it needs before it can run. Follow the link for the source's full set of options.

## Which source gives me a trending or popular row?

**TMDB Lists** includes **Trending Today**, **Trending This Week**, **Popular** and **Top Rated**. It needs nothing else set up.

**Trakt Lists** includes **Trending Now** and **Popular**. **IMDb Lists** includes **Popular (Meter)** and **Top 250**. The [Trending or Popular how-to](make-trending.md) walks through the setup.

## Which source gives me a curated list?

Use the source that holds the list. IMDb, Letterboxd, Trakt, MDBList and TVDB each have a **Custom List** sub-type that reads a list from its address. Letterboxd and Trakt can also read a watchlist. See [Turn a list into a collection](list-to-collection.md).

**TMDB Lists** can read a TMDB collection or list. **Custom Advanced Filters** builds one from genre, rating or streaming service. See [TMDB filters](sources/tmdb-filters.md).

## Which source follows a streaming service?

**Networks Top 10** follows a streaming service's daily chart for a country. **Networks Originals** lists the service's own productions. See [Add a streaming Top 10 or Originals row](streaming-top-10.md).

## Which source organises my own library?

**Plex Library** makes one collection for each genre, decade, resolution, content rating, director or actor. It can also build one from a Plex label.

**Radarr Tag** and **Sonarr Tag** turn a tag into a collection. **Filtered Plex Hub** makes recently added rows without placeholders. **TMDB Lists** includes **Auto Franchise Collections**. See [Make collections from your own library](from-your-library.md) and [Your servers](sources/your-servers.md).

## Which source covers anime?

**AniList** reads charts, a member's list or a search page. **MyAnimeList** reads its top charts. See [Add an anime collection](anime.md) and [Anime](sources/anime.md).

## Which source shows what people request or watch?

**Seerr Requests** lists the titles people requested. **Tautulli Statistics** or **Tracearr Statistics** lists what people played. See [Show what people request and watch](requests-and-watching.md) and [Activity sources](sources/activity.md).

## Which source shows what's coming?

**Coming Soon** lists unreleased titles and creates placeholders for them. See [Coming Soon](sources/coming-soon.md) and [Placeholders](../Placeholders.md).

## Which source makes a row that changes?

**Multiple Sources** combines several lists into one row. Its **Cycle Lists** mode shows one list at a time, then moves to the next one at each sync.

**TMDB Lists**, **IMDb Lists**, **Letterboxd Lists** and **Trakt Lists** each have a **Random Lists** sub-type. It picks a different list from a pool at every sync.

To swap collections you already have on Home, use [Rotation](../appear/rotation.md). See [Rotate several lists through one row](cycle-sources.md).

## What does each source need?

| Source | Needs |
|--------|-------|
| TMDB Lists | Nothing |
| IMDb Lists | Nothing |
| Letterboxd Lists | Nothing, or a solver if blocked |
| TVDB Lists | Nothing |
| AniList | Nothing |
| Trakt Lists | A Trakt Client ID |
| MDBList Lists | An MDBList API key |
| MyAnimeList | A MyAnimeList Client ID |
| Networks Top 10 | A Cloudflare solver |
| Networks Originals | An MDBList API key |
| Seerr Requests | A Seerr connection |
| Tautulli or Tracearr | That app |
| Radarr Tag, Sonarr Tag | That app |
| Coming Soon | Depends on sub-type |
| Plex Library, Filtered Plex Hub | Your Plex connection |

If a key or app is missing, an orange warning and a **Configure Settings** button appear under **Collection Type**. Coming Soon also needs a placeholder root folder.

## Does the source decide movies or TV?

Your library usually decides. Choose a movie library for the source's movies or a TV library for its shows.

There are a few exceptions:

- **Letterboxd Lists** and **Radarr Tag** are movies only. **Sonarr Tag** is TV only.
- **Box Office**, **Top 250 English** and **Bottom 100** are movies only.
- **Coming Soon** holds one media type per collection.

## Related

- [Your first collection](../get-started/first-collection.md)
- [The Add Collection form](form/basics.md)
