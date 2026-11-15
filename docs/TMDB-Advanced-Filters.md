# TMDB Custom Advanced Filters

## What it does

Custom Advanced Filters builds a collection from TMDB's Discover search. Pick filters such as genre, rating, streaming service, or release date, and Agregarr asks TMDB for every matching movie or show. It then keeps the ones in your library, the same as any other list.

Movie libraries search TMDB movies. TV libraries search TMDB shows. The library you choose decides which one, not the filters.

## Set it up

1. Open the collections page and click **Add Collection**.
2. Set **Collection Type** to **TMDB Lists**.
3. Set **Collection Sub-Type** to **Custom Advanced Filters**.
4. The **Advanced Filters** section appears. Click **Add Filter Group** and build your filters (see below).
5. Choose a library under **Library Selection**. The rest of the form appears.
6. Set **Collection Title Template**, then choose **Movie Sort Order** or **TV Sort Order** (see [Sorting](#sorting)).
7. Set **Max Items**, then save.

You can save without adding any filters. Agregarr then fetches TMDB's whole Discover list for that library type, which is every movie or show sorted by popularity.

## As a source in Multiple Sources

1. Set **Collection Type** to **Multiple Sources**.
2. Under **Sources**, click **Add Source**.
3. Set **Source Type** to **TMDB Lists** and **Collection Sub-Type** to **Custom Advanced Filters**.
4. The **Advanced Filters** section appears inside that source. Build the filters the same way as you would for a standalone collection.

Each source has its own filters. Two Custom Advanced Filters sources in one collection fetch separately. See [Cycle Lists](Cycle-Lists.md) for combine modes and the rest of the Multiple Sources form.

Changing a source's **Collection Sub-Type** to something else clears its filters.

## Groups and filters

Filters sit inside groups. A collection can have several groups, and each group can contain several filters.

### Inside a group

Every filter in a group must match. The group header reads "Match all of the following". Agregarr sends the entire group to TMDB as one search.

A field can appear only once in each group. To use the same field twice, put it in another group.

Some fields accept several values. A second dropdown appears next to these fields:

- **Match all**: an item must have every value you picked.
- **Match any**: an item needs at least one of them.

The dropdown appears for **Include Genres**, **Cast**, **Crew**, **People**, **Companies**, **Keywords**, **Release Type**, **Streaming Service**, **Watch Monetization Types**, **Networks (TMDB IDs)** and **Origin Country**.

Some multi-value fields don't have the dropdown:

- **Status**, **Show Type** and **Certification** always match any of the values you pick. A show has one status and one type, and a movie has one certification, so matching all of them could never return anything.
- **Exclude Genres** and **Exclude Keywords** leave out anything with any of the values you pick.

### Between groups

From the second group onwards, a dropdown at the top of the group controls how it joins the groups above it:

- **AND**: keep only items found by both.
- **OR**: add this group's items to the results.
- **NOT**: remove this group's items from the results. The header changes to "Exclude results matching the following".

The first group doesn't have a dropdown. It's always the starting set.

Each group runs as a separate TMDB search. Agregarr runs them all, then joins the results from top to bottom. Each search stops at the item limit under [How many results](#how-many-results). This matters for **AND** and **NOT**:

- **AND** keeps only items that appeared in both groups' fetched results. Two broad groups might have very little overlap. If you want "drama and rated 7+", put both filters in the same group.
- **NOT** removes only the items that the NOT group fetched.

A group with only empty filters, or only filters that don't apply to the library type, searches TMDB without any filters. As an **OR** group, it adds TMDB's most popular titles. As a **NOT** group, it can remove most of your results.

## Filter reference

The field dropdown adds "(Movie)" or "(TV)" after fields that work with only one library type. Agregarr skips a movie-only filter in a TV library and a TV-only filter in a movie library. It doesn't show a warning.

There are two exceptions. In a TV library, **Release Year** is sent as **First Air Year**, while **Released after** / **Released before** are sent as **First Aired after** / **First Aired before**.

### Streaming

| Field | Works for | TMDB parameter | Value |
|-------|-----------|----------------|-------|
| **Region/Country** | Both | `watch_region` | One country |
| **Streaming Service** | Both | `with_watch_providers` | Pick from a list |
| **Watch Monetization Types** | Both | `with_watch_monetization_types` | **Flatrate**, **Free**, **Ads**, **Rent**, **Buy** |

- The **Streaming Service** list shows services available in the **Region/Country** you picked, or the US if you haven't picked one.
- If you use **Streaming Service** without **Region/Country**, Agregarr sends the US as the region.
- One **Region/Country** applies to the entire collection. If you set it in more than one group, the first one wins.

### Genres, people, companies, keywords

| Field | Works for | TMDB parameter | Value |
|-------|-----------|----------------|-------|
| **Include Genres** | Both | `with_genres` | Pick genres |
| **Exclude Genres** | Both | `without_genres` | Pick genres |
| **Cast** | Movie | `with_cast` | Search people |
| **Crew** | Movie | `with_crew` | Search people |
| **People** | Movie | `with_people` | Search people |
| **Companies** | Both | `with_companies` | Search companies |
| **Keywords** | Both | `with_keywords` | Search keywords |
| **Exclude Keywords** | Both | `without_keywords` | Search keywords |
| **Networks (TMDB IDs)** | TV | `with_networks` | Type TMDB network IDs |

- The genre list combines movie and TV genres. Genres that exist on only one side are marked "(Movie)" or "(TV)". A TV-only genre in a movie library matches nothing.
- The search fields show "Search or enter ID...". Type a name and pick a result, or type a TMDB ID and press Enter or click **Add**. Agregarr saves the ID.
- **Networks (TMDB IDs)** doesn't have search. Type the numeric ID and press Enter or click **Add**.

### TV

| Field | Works for | TMDB parameter | Value |
|-------|-----------|----------------|-------|
| **First Air Year** | TV | `first_air_date_year` | Year |
| **First Aired after** | TV | `first_air_date.gte` | Date |
| **First Aired before** | TV | `first_air_date.lte` | Date |
| **Aired after** | TV | `air_date.gte` | Date |
| **Aired before** | TV | `air_date.lte` | Date |
| **Include shows with no air date** | TV | `include_null_first_air_dates` | Yes / No |
| **Screened Theatrically** | TV | `screened_theatrically` | Yes / No |
| **Status** | TV | `with_status` | **Returning Series**, **Planned**, **In Production**, **Ended**, **Cancelled**, **Pilot** |
| **Show Type** | TV | `with_type` | **Documentary**, **News**, **Miniseries**, **Reality**, **Scripted**, **Talk Show**, **Video** |
| **Timezone** | TV | `timezone` | Free text |

Selecting **Returning Series** and **Ended** under **Status** finds shows with either status.

### Movies

| Field | Works for | TMDB parameter | Value |
|-------|-----------|----------------|-------|
| **Release Year** | Movie | `primary_release_year` | Year |
| **Released after** | Movie | `primary_release_date.gte` | Date |
| **Released before** | Movie | `primary_release_date.lte` | Date |
| **Release Date after** | Movie | `release_date.gte` | Date |
| **Release Date before** | Movie | `release_date.lte` | Date |
| **Release Type** | Movie | `with_release_type` | **Premiere (1)** to **TV (6)** |
| **Include Video Content** | Movie | `include_video` | Yes / No |

### Ratings, runtime, language

| Field | Works for | TMDB parameter | Value |
|-------|-----------|----------------|-------|
| **Rating at least** | Both | `vote_average.gte` | 0 to 10, steps of 0.1 |
| **Rating at most** | Both | `vote_average.lte` | 0 to 10, steps of 0.1 |
| **Vote count at least** | Both | `vote_count.gte` | Number |
| **Vote count at most** | Both | `vote_count.lte` | Number |
| **Runtime at least (minutes)** | Both | `with_runtime.gte` | Minutes |
| **Runtime at most (minutes)** | Both | `with_runtime.lte` | Minutes |
| **Original Language** | Both | `with_original_language` | One language |
| **Origin Country** | Both | `with_origin_country` | Pick countries |
| **Include Adult Content** | Both | `include_adult` | Yes / No |

- Year fields run from 1900 to five years from now.
- **Include Adult Content** and **Include Video Content** default to No unless you set them.

### Certification

| Field | Works for | TMDB parameter | Value |
|-------|-----------|----------------|-------|
| **Certification Country** | Movie | `certification_country` | **United States**, **United Kingdom**, **Canada** |
| **Certification** | Movie | `certification` | **G**, **PG**, **PG-13**, **R**, **NC-17** |
| **Certification at least** | Movie | `certification.gte` | One rating |
| **Certification at most** | Movie | `certification.lte` | One rating |

The rating options are US ratings, regardless of the country you pick. Pair them with **United States**. Selecting several ratings under **Certification** finds movies with any of them.

## Sorting

A standalone Custom Advanced Filters collection doesn't show **Item Order**. Select a movie library to get **Movie Sort Order**, or a TV library to get **TV Sort Order**. Both default to **Popularity (High to Low)**.

- **Movie Sort Order**: popularity, **Random (from TMDB results)**, release date, revenue, rating, vote count, title and original title, each high-to-low and low-to-high.
- **TV Sort Order**: popularity, **Random (from TMDB results)**, first air date, rating, vote count, name and original name, each both ways.

Agregarr sends the selected sort to TMDB for each group, then sorts the joined results the same way. Popularity breaks ties.

Revenue works differently. TMDB sorts by revenue but doesn't return the figures, so Agregarr keeps TMDB's order. When several groups are joined with **OR**, each group's results follow those from the groups before it.

**Random (from TMDB results)** fetches by popularity, then shuffles the matched items on every sync.

In **Multiple Sources**, sources don't have a sort setting. Each Custom Advanced Filters source fetches by popularity. Use the collection's **Item Order** to sort the combined list.

## How many results

TMDB returns 20 results per page. Agregarr fetches five pages at a time for each group and stops when:

- it has at least ten times **Max Items**,
- a page returns fewer than 20 results, or
- it reaches page 500, TMDB's limit.

After joining and sorting the groups, Agregarr keeps the first ten times **Max Items**. It then matches those results to your library and stops at **Max Items**. With the default of 50, it can check up to 500 TMDB results against your library. Raise **Max Items** if a narrow library leaves the collection short.

In **Multiple Sources**, each Custom Advanced Filters source uses the collection's **Max Items** for this limit.

If TMDB has a temporary problem, Agregarr retries a page up to three times. If TMDB rejects a request, Agregarr stops fetching that group and keeps whatever it already has. The collection can end up empty without a sync error.

## Language and region

- Agregarr asks TMDB for results in English (en-US).
- The only region used is the one set with **Region/Country**. It affects streaming filters only.
- Use **Original Language** or **Origin Country** to filter by language or country of origin.

## Preview

**Preview Collection** runs the same filters against your library. Change a filter, sort, or **Max Items**, and the next preview runs a fresh fetch.

- **Random (from TMDB results)** isn't shuffled in the preview. It appears in popularity order.
- In **Multiple Sources**, a source that fails shows no items in the preview and no error. During a sync, the same failure is logged as an error.

## Standalone vs Multiple Sources

| | Standalone | Multiple Sources source |
|---|---|---|
| Sort | **Movie Sort Order** / **TV Sort Order** | Popularity only; use **Item Order** |
| Fetch limit | Ten times **Max Items** | Ten times the collection's **Max Items** |
| Dynamic title | n/a | "Custom Advanced Filters" for every such source |

With **Cycle Lists** and **Dynamic Title from Active Source**, every Custom Advanced Filters source gets the title "Custom Advanced Filters". Use **Custom** if you cycle more than one.
