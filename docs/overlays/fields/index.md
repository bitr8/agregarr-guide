# Field reference

Every field available as an overlay variable or condition, grouped the same way as the editor.

## How to read an entry

Each entry starts with the field label, followed by:

- **Variable:** the field's internal name in braces. Templates save it under this name, though you pick it by label in the variable picker. `none` means it is condition only.
- **Condition:** the group and label in the condition picker, for example **Status > Days Until Release**. `none` means it is variable only. If the condition picker uses different wording from the variable picker, this line uses the condition wording.
- **Applies to:** the item types that get a value. The field is empty everywhere else.
- A sentence describing what it holds.
- **Example:** the sample value shown in the editor, not a value from your library.
- **Format:** any extra option available for the variable. Date variables get **Date Format**, in groups: **Short (No Year)**, **Short (With Year)**, **Full Month Name**, **Month and Year**, **Numeric** and **With Weekday**. Some counts get **Number Format**: **Plain (5)**, **Pad to 2 digits (05)** or **Pad to 3 digits (005)**.

## Variable picker

Groups are listed in picker order, each with the page that holds it.

- **Ratings**: Ratings (not written yet).
- **Metadata**: Metadata and status (not written yet).
- **Video**: Video and file (not written yet).
- **Audio**: Audio and languages (not written yet).
- **Language**: Audio and languages (not written yet).
- **File Info**: Video and file (not written yet).
- **Playback**: Metadata and status (not written yet).
- **Collection**: Metadata and status (not written yet).
- **Coming Soon**: split two ways.
    - Release Date through **Days Since Release (incl. release day)**: [Release](release.md).
    - Next Episode Air Date through **Episode Number**, including the finale, last aired and downloaded fields: TV dates (not written yet).
- **Status**: split two ways.
    - **Is Placeholder (Coming Soon)**: [Release](release.md).
    - The rest (**Is Monitored**, **In Radarr**, **In Sonarr**, **Downloaded**, **Missing Episode Count (TV)**, **Radarr Tags**, **Sonarr Tags**, **Days Until Maintainerr Action**): Metadata and status (not written yet).
- **Episode Media**: Episode media (not written yet).
- **Show Raw Values**: Episode media (not written yet).

## Condition picker

Groups are listed in picker order, each with the page that holds it.

- **TMDB Data**: Metadata and status (not written yet).
- **Plex Data**: split three ways.
    - Video, HDR and file fields (**Versions** to **Color Transfer**, then **Container** to **Edition**): Video and file (not written yet).
    - **Audio Format** to **Has Subtitles**: Audio and languages (not written yet).
    - **View Count**, **Days Since Added**, **Days Since Last Played**: Metadata and status (not written yet).
- **Ratings**: Ratings (not written yet).
- **Collections**: Metadata and status (not written yet).
- **Status**: split three ways.
    - **Is Placeholder (Coming Soon)**, **Days Until Release** and the movie release date and day count fields up to **Days Since Release (incl. release day)**: [Release](release.md).
    - **Next Episode Air Date (TV)** through **Episode Number**: TV dates (not written yet).
    - **Media Type (movie/show)**, **Is Monitored**, **In Radarr**, **In Sonarr**, **Downloaded**, **Missing Episode Count (TV)**, **Radarr Tags**, **Sonarr Tags**, **Plex Label**, **Days Until Maintainerr Action**: Metadata and status (not written yet).
- **Episode Media**: Episode media (not written yet), including the **(Raw)** show fields.

Related: [Release fields](release.md), [Overlay Variables](../../Overlay-Variables.md), [Overlay Conditions](../../Overlay-Conditions.md)
