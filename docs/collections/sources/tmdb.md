# TMDB Lists

TMDB Lists turns charts, filters, franchises and public lists from The Movie Database into Plex collections. Use it when you want a row that fills itself without maintaining a list by hand.

**Needs:** nothing. TMDB lists work without an account, API key or anything configured under **Settings**.

**Works with:** movie and TV libraries, except where a sub-type below says otherwise. Agregarr reads the library you choose, then asks TMDB for matching movies or shows.

Every TMDB collection starts the same way:

1. Click **Add Collection**.
2. Set **Collection Type** to **TMDB Lists**.
3. Set **Collection Sub-Type** to one of the sub-types below.
4. Choose a library under **Library Selection**. The rest of the form appears after you've picked one.

Agregarr keeps the titles from TMDB's list that exist in your library. Your missing-item options decide what happens to titles you don't have, not the source. For a first run-through, see [Your first collection](../../get-started/first-collection.md).

## Trending Today

Titles TMDB says people are watching today. This one moves quickly.

**Works with:** movies and TV.

### Use it

1. Set **Collection Sub-Type** to **Trending Today**.
2. Choose your library.
3. Pick a **Collection Title Template**. The first preset is **Trending Today**.

**Check:** after the next sync, the collection holds today's trending TMDB titles that you own.

It has no options of its own. The common collection options still apply, including **Item Order**. Leave it on **Default order (as provided by source)** to preserve TMDB's ranking.

## Trending This Week

The same chart over seven days. It moves more slowly, so the row changes less between syncs.

**Works with:** movies and TV.

### Use it

1. Set **Collection Sub-Type** to **Trending This Week**.
2. Choose your library.
3. Pick a **Collection Title Template**.

**Check:** after the next sync, the collection holds this week's trending TMDB titles that you own.

It has no options of its own.

## Popular

TMDB titles ranked by popularity. This uses TMDB's popularity score, not votes or ratings.

**Works with:** movies and TV. A movie library gets movies, and a TV library gets shows.

### Use it

1. Set **Collection Sub-Type** to **Popular**.
2. Choose your library.
3. Pick a **Collection Title Template**.

**Check:** after the next sync, the collection holds the most popular TMDB titles that you own.

It has no options of its own. If you want a popularity row narrowed by genre, year or streaming service, use **Custom Advanced Filters** instead.

## Top Rated

TMDB titles with the highest average ratings. To stop a film with three perfect votes from topping the chart, Agregarr only considers titles with at least 300 votes for movies or 200 votes for TV.

**Works with:** movies and TV.

### Use it

1. Set **Collection Sub-Type** to **Top Rated**.
2. Choose your library.
3. Pick a **Collection Title Template**.

**Check:** after the next sync, the collection holds the highest rated TMDB titles that you own.

It has no options of its own, and the vote minimums are fixed. To change them, build the same list under **Custom Advanced Filters**. Add **Vote count at least** with the minimum you want, then set **Movie Sort Order** or **TV Sort Order** to **Rating (High to Low)**.

## Custom Advanced Filters

Builds a list from TMDB's Discover search using filters such as genre, rating, streaming service and release date. Use it for lists such as "Australian drama from the last five years" or "free to stream with ads".

**Works with:** movies and TV. The library you choose decides which, not the filters.

### Use it

1. Set **Collection Sub-Type** to **Custom Advanced Filters**.
2. In **Advanced Filters**, click **Add Filter Group**.
3. Pick fields and values. The full field list, along with how groups combine, is on [TMDB filters](tmdb-filters.md).
4. Choose your library and a **Collection Title Template**.
5. Pick **Movie Sort Order** or **TV Sort Order**.

**Check:** **Preview Collection** shows what your filters return from your library.

You can save without adding any filters. Agregarr then asks TMDB for the whole Discover list for that library type, sorted by popularity.

### Advanced Filters

**Default:** no groups.

The filter builder uses groups of filters joined by **AND**, **OR** or **NOT**. See [TMDB filters](tmdb-filters.md#how-groups-and-filters-combine).

### Movie Sort Order and TV Sort Order

**Default:** **Popularity (High to Low)**.

This replaces **Item Order** for this sub-type. Movie and TV libraries each get their own dropdown. If you pick both library types, you get both dropdowns. See [Sorting](tmdb-filters.md#sorting).

This sub-type also works as a source in **Multiple Sources**. There it fetches by popularity, and the collection's **Item Order** sorts the combined list. See [Cycle Lists](../cycle-sources.md).

## Auto Franchise Collections

Creates one collection for every movie franchise in your library. You get a "Die Hard" collection without building it yourself. TMDB groups the movies into franchises, and Agregarr reads those groups.

**Works with:** movie libraries. It reads TMDB IDs from the movies in your library, so it isn't intended for TV libraries.

**Needs:** Plex movies matched with TMDB IDs. A movie without a TMDB ID in its Plex metadata isn't counted.

### Use it

1. Set **Collection Sub-Type** to **Auto Franchise Collections**.
2. Choose your movie library.
3. Set **Minimum movies in library** (see below).
4. Keep `{franchiseName}` in **Collection Title Template**. The preset **Franchise Name (from TMDB)** adds "Collection" after the name.
5. Click **Create Collection**.

**Check:** after the next sync, each franchise where you own enough movies has its own collection in Plex.

One entry in Agregarr creates multiple Plex collections, one for each franchise. This sub-type has no **Preview Collection** button. The form still shows an **Item Order** dropdown, but Agregarr doesn't apply it here. Items within each franchise always use release-date order, oldest first, and movies without a TMDB date go at the end.
<!-- OPEN: Item Order dropdown is shown for Auto Franchise but the franchise sync never reads it (tmdb.ts processFranchiseCollections has no orderItems call) -->

### Minimum movies in library

**Default:** 2.

The number of movies from a franchise you must own before Agregarr creates its collection. The lowest value is 1.

If a franchise falls below the minimum, Agregarr removes its collection on a later sync. It only does that after a fresh read of the whole library confirms the count. If a read comes back incomplete, for example because a movie has no TMDB ID, Agregarr leaves the collection alone until it gets a clean read.

A franchise still needs at least two movies in TMDB, regardless of the value set here.

Agregarr only grabs missing movies for franchises that meet this minimum. Setting the minimum to 1 with **Grab Missing Items** turned on can send a lot more requests, so raise it before enabling that option. Your year and rating filters still apply.

### Count only released movies

**Default:** off.

When ticked, unreleased or undated TMDB entries don't count towards the franchise size. A movie you already own always counts, regardless of its TMDB date. If this makes a franchise fall below two released movies, its collection is also removed once Plex shows fewer than two items in it.

### Collection Title Template

**Default:** **Franchise Name (from TMDB)**.

Sets the name of each collection. Saving fails unless the template contains `{franchiseName}`.

TMDB names franchises "Something Collection". Agregarr removes the trailing "Collection" from `{franchiseName}`, so the preset produces "Die Hard Collection" instead of "Die Hard Collection Collection".

### Use TMDB Franchise Poster

**Default:** off.

Uses TMDB's official poster for each franchise instead of generating one. When TMDB has a poster, this takes priority over **Auto-generate Collection posters**. When it doesn't, Agregarr falls back to the generated poster. The option sits with the other poster settings.

### Hide Individual Items in Collection

**Default:** off.

Hides the individual movies from Plex's **Library** tab and shows only the collection. A movie that's also part of another collection can still appear through that collection. The option sits with the poster settings.

### Visibility

Franchise collections only appear on the **Library** tab. **Users Home**, **Server Owner Home** and **Library Recommended** are switched off so dozens of franchises don't fill your Home screens.

They also can't target individual users and don't appear as choices in **Collection Mutual Exclusion**.

## Custom Collection/List

Mirrors one specific TMDB page: a collection, list, network or company. Use it when you've found the exact page you want.

**Works with:** depends on the page. Collections are movies only. Lists can contain both movies and shows. Networks are TV only. Companies follow the `/movie` or `/tv` at the end of the address.

### Use it

1. Set **Collection Sub-Type** to **Custom Collection/List**.
2. Paste the page address into **Custom TMDB Collection/List/Network/Company URL**.
3. Click **Validate**.
4. Choose your library.

**Check:** **Collection Title Template** offers the page's name instead of **Validate URL**.

The rest of the form stays hidden until the URL box contains something.

### Custom TMDB Collection/List/Network/Company URL

**Default:** empty. Required.

The address must start with `https://` and use `themoviedb.org`. These forms work:

- Collection: `https://www.themoviedb.org/collection/12345`
- List: `https://www.themoviedb.org/list/310`
- Network: `https://www.themoviedb.org/network/213`
- Company: `https://www.themoviedb.org/company/7505/movie` or `/tv` on the end

Anything after the page is ignored. If the address has the wrong shape, saving shows an error naming the four allowed kinds.

Each kind returns something different:

- **Collection:** the movies TMDB lists in it.
- **List:** every item on the list, including movies and shows, in the list's order.
- **Network:** TV shows from that network, most popular first.
- **Company:** movies or shows from that company, most popular first, depending on whether the address ends in `/movie` or `/tv`.
<!-- OPEN: NEW: TV company URL (/company/ID/tv) queries Discover with the company ID as a network (with_networks) instead of with_companies, tmdb.ts company branch -->

Match the library to the address. Network addresses return shows, so use a TV library. Collection addresses return movies, so use a movie library.

### Validate

Checks the address with TMDB, reads the page name and works out whether it contains movies, shows or both. The name then appears as a choice in **Collection Title Template**.

If the page only contains one kind of title, a warning appears under **Library Selection**. For example: "Detected Movies only. TV Shows collections will be empty until matching content is added."

Until you click **Validate**, the first title template choice is **Validate URL**.

If TMDB doesn't recognise the address, you get "Invalid TMDB collection/list/network/company ID or not found".

In **Multiple Sources**, this sub-type is called **Custom Collection**, and its address goes in the **Custom URL** box.

## Random Lists

Picks a different TMDB collection for the row each time it syncs. One week it might be "Mission: Impossible", the next "Harry Potter". Use it for a "what should I watch" row you don't want to curate.

**Works with:** movie libraries only. A TV library has nothing to pick from, so the collection stays empty.

### Use it

1. Set **Collection Sub-Type** to **Random Lists**.
2. Choose your movie library.
3. Set **Collection Title Template** to **Dynamic Title from Random List** so the row uses the name of the collection it picked.

**Check:** after the next sync, the row takes the name of the collection it picked.

### Dynamic Title from Random List

The row takes the picked collection's name. The other presets use fixed names: **Random TMDB Collection**, **Random TMDB {mediaType}s** and **Curated {mediaType}s from TMDB**.

Using the dynamic title adds a box for a prefix. Agregarr puts the prefix before the name with a space between them, so don't end the prefix with a space. A prefix containing an emoji and a pick called "Musical" gives the emoji, a space, then "Musical".

### Where the choices come from

Agregarr only picks a collection where you own at least two of its movies, wherever the pick comes from. It looks in three places, in this order:

1. **Your own list.** The file `random-lists/tmdb.txt` in your Agregarr config folder, for example `/mnt/user/appdata/agregarr/random-lists/tmdb.txt`. Agregarr creates it with `enabled=false` and a few example addresses. Those addresses aren't commented out, so they join the rotation when you set `enabled=true`. Delete any you don't want. Set `enabled=true`, then add one TMDB collection address per line. Only collection addresses work (`.../collection/1570`). Agregarr skips list, network and company addresses. If the file has no usable collection addresses, the row stays empty.
2. **Franchises in your library.** Without your own list, Agregarr samples up to 150 movies from your library, checks which TMDB collections they belong to and keeps collections where you own at least two of their movies. It rebuilds this pool about once a week, or every few hours while the pool remains empty.
3. **TMDB's published collections.** If your library returns nothing, Agregarr uses TMDB's daily export of collection IDs, refreshed about monthly, and checks each pick against your library. If that fails too, it uses a short built-in list of well-known franchises.

To exclude a collection from the rotation, add `exclude=` followed by its address to `tmdb.txt`. Exclusions apply to all three places and still work when `enabled` is false.

Two random collections in the same sync won't choose the same collection while other choices remain. A single random collection avoids repeating its previous pick.

## How TMDB lists refresh and their limits

Each sync asks TMDB for fresh data. Agregarr keeps a saved copy for up to 7 days, but that copy only speeds up **Preview Collection**. Preview fetches fresh data again after you change the filters, sort order or **Max Items**.

For chart-style sub-types, **Custom Advanced Filters** and network or company addresses, Agregarr reads five pages, or 100 results, at a time. It stops after reading ten times **Max Items**, when TMDB runs out of results, or at page 500, which is TMDB's own limit.

A collection with **Max Items** set to 50 can therefore check up to 500 TMDB results before matching them against your library. If a narrow library leaves the row short, raise **Max Items**.

Trending, Popular and Top Rated don't send a region to TMDB, so they use its worldwide ranking.

A sync can only use titles that exist in your library or that your missing-item options handle. [Placeholders](../../Placeholders.md) covers what Agregarr creates for missing titles.

## If it didn't work

- **"TMDB API is not accessible":** Agregarr couldn't reach TMDB at all. Check the internet connection and DNS on the machine running Agregarr.
- **The collection is empty:** the list had nothing you own. Open **Preview Collection** to check what TMDB returned. For Custom Advanced Filters, loosen the filters or raise **Max Items**.
- **A network collection is empty in a movie library:** networks contain shows. Use a TV library.
- **"Please enter a valid TMDB URL":** the address doesn't match a collection, list, network or company page. Copy it from your browser while viewing that TMDB page.
- **A Random Lists row is empty:** it only works with a movie library and needs at least one collection where you own two movies. If `enabled=true` is set in `random-lists/tmdb.txt`, check that every line uses a collection address.
- **A franchise collection you expected is missing:** it needs at least the configured minimum in your library and at least two movies in TMDB. Plex movies without a TMDB ID aren't counted.

## Related

- [TMDB filters](tmdb-filters.md) for every filter field and how groups combine
- [Cycle Lists](../cycle-sources.md) for using TMDB sources in **Multiple Sources**
- [Your first collection](../../get-started/first-collection.md)
- [Placeholders](../../Placeholders.md)
