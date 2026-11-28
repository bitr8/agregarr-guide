# Release fields

Release dates, day counts, and Coming Soon state for movies and shows. TV air dates, finales, and seasons are on the TV dates page (not written yet). See [how to read an entry](index.md#how-to-read-an-entry).

Movie dates follow **Watch Provider Region** under **Settings > General**. If that country has no date of a type, Agregarr uses the earliest date of that type from any country. Season and episode artwork don't get the release dates or day counts on this page.

## Release dates

### Release Date {#release-date}

- Variable: `{releaseDate}`
- Condition: none
- Applies to: movie / show
- The item's release date. For a movie, it's the earlier of the digital and physical dates, theatrical plus 90 days when neither exists, or TMDB's main release date when TMDB lists none of the three. On a Coming Soon placeholder, Radarr's digital or physical date wins when it has one. For a show, it's the first air date.
- Example: `JAN 15`
- Format: **Date Format**, default **DEC 20**

### Release Date Is Estimated {#release-date-is-estimated}

- Variable: `{isEstimatedReleaseDate}`
- Condition: **Status > Release Date Is Estimated**
- Applies to: movie
- Whether the release date is a guess (theatrical plus 90 days) rather than a published digital or physical date.
- Example: `false`

### Theatrical Release Date (Movies) {#theatrical-release-date}

- Variable: `{theatricalReleaseDate}`
- Condition: **Status > Theatrical Release Date (Movies)**
- Applies to: movie
- The cinema release date.
- Example: `OCT 25`
- In a condition, the value is the plain date, `YYYY-MM-DD` (for example `2026-12-25`), not the formatted text. Use begins with `2026-` or equals `2026-12-25`. The `OCT 25` hint in the value box won't match.
- Format: **Date Format**, default **DEC 20**

### Digital Release Date (Movies) {#digital-release-date}

- Variable: `{digitalReleaseDate}`
- Condition: **Status > Digital Release Date (Movies)**
- Applies to: movie
- The digital release date. On a Coming Soon placeholder, Radarr's digital date wins when it has one.
- Example: `DEC 25`
- In a condition, the value is the plain date, `YYYY-MM-DD` (for example `2026-12-25`), not the formatted text. Use begins with `2026-` or equals `2026-12-25`. The `DEC 25` hint in the value box won't match.
- Format: **Date Format**, default **DEC 20**

### Physical Release Date (Movies) {#physical-release-date}

- Variable: `{physicalReleaseDate}`
- Condition: **Status > Physical Release Date (Movies)**
- Applies to: movie
- The physical (disc) release date. On a Coming Soon placeholder, Radarr's physical date wins when it has one.
- Example: `JAN 14`
- In a condition, the value is the plain date, `YYYY-MM-DD` (for example `2026-12-25`), not the formatted text. Use begins with `2026-` or equals `2026-12-25`. The `JAN 14` hint in the value box won't match.
- Format: **Date Format**, default **DEC 20**

### Release Date Source (digital/physical/estimate) {#release-date-source}

- Variable: `{releaseDateSource}`
- Condition: **Status > Release Date Source (digital/physical/estimate)**
- Applies to: movie
- Where the release date came from: `digital`, `physical`, `estimate` (theatrical plus 90 days), or `tmdb` (TMDB's main release date, used when TMDB lists no digital, physical, or theatrical date for the movie).
- Example: `digital`

## Day counts

### Days Since Theatrical Release (Movies, negative before the date) {#days-since-theatrical-release}

- Variable: `{daysSinceTheatricalRelease}`
- Condition: **Status > Days Since Theatrical Release (Movies), negative before the date**
- Applies to: movie
- Whole days since the theatrical date. `0` is the day itself, and a negative number counts down to it.
- Example: `61`

### Days Since Digital Release (Movies, negative before the date) {#days-since-digital-release}

- Variable: `{daysSinceDigitalRelease}`
- Condition: **Status > Days Since Digital Release (Movies), negative before the date**
- Applies to: movie
- Whole days since the digital date, counted the same way.
- Example: `0`

### Days Since Physical Release (Movies, negative before the date) {#days-since-physical-release}

- Variable: `{daysSincePhysicalRelease}`
- Condition: **Status > Days Since Physical Release (Movies), negative before the date**
- Applies to: movie
- Whole days since the physical date, counted the same way.
- Example: `-20`

### Days Since Home Release (Digital or Physical, negative before the date) {#days-since-home-release}

- Variable: `{daysSinceHomeRelease}`
- Condition: **Status > Days Since Home Release (Digital or Physical), negative before the date**
- Applies to: movie
- Whole days since the earlier of the digital and physical dates, counted the same way. On a Coming Soon placeholder, it uses Radarr's home date when Radarr has one.
- Example: `0`

### Days Until Release {#days-until-release}

- Variable: `{daysUntilRelease}`
- Condition: **Status > Days Until Release**
- Applies to: movie / show
- Days from today to the release date. Empty from the release day onwards.
- Example: `14`
- Format: **Number Format**, default **Plain (5)**

### Days Since Release (incl. release day) {#days-since-release}

- Variable: `{daysAgo}`
- Condition: **Status > Days Since Release (incl. release day)**
- Applies to: movie / show
- Days since the release date. `0` on the release day itself, and empty before it.
- Example: `3`
- Format: **Number Format**, default **Plain (5)**

## Coming Soon state

### Is Placeholder (Coming Soon) {#is-placeholder}

- Variable: `{isPlaceholder}`
- Condition: **Status > Is Placeholder (Coming Soon)**
- Applies to: movie / show
- Whether the item is a Coming Soon placeholder. It's never true once Radarr or Sonarr reports a file for the item.
- Example: `true`

Related: [Field reference](index.md), [Overlay Variables](../../Overlay-Variables.md), [Overlay Conditions](../../Overlay-Conditions.md)
