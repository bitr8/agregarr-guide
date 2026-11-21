# Overlay Examples

Worked examples combining templates, variables, and [conditions](Overlay-Conditions.md) to do more than a single preset can on its own.

## One banner that changes its wording

A single template can show different text on different items. This example uses one red banner that reads `JUST RELEASED` for the first few days after release, then switches to `RELEASED 4 DAYS AGO`, `RELEASED 5 DAYS AGO`, and so on.

![The same banner on release day and on day 4](images/example-release-banner-result.png)

1. Open the **Overlay Templates** tab and **Duplicate** the **Released Days Ago (Monitored)** preset. The copy already has the red banner and the `RELEASED {daysAgo} DAYS AGO` text.
2. Open the copy. Under **Application Condition**, click **Edit Conditions**. In both sections, change `Days Since Release (incl. release day)` `greater than` `1` to `greater than or equal` `0`. That includes release day. Leave the other rules alone.
3. Click **Text** under **Layers** to add a text element, then enter `JUST RELEASED`. Give it the same **X Position**, **Y Position**, **Width**, **Height**, and font settings as the existing `RELEASED {daysAgo} DAYS AGO` element. Both elements should sit in the same spot.
4. Select `JUST RELEASED`, scroll to the bottom of **Properties**, and click **Edit Conditions** next to **Show conditionally**. Tick **Only show when:**, set the rule to `Days Since Release (incl. release day)` `less than or equal` `3`, then click **Save**.
5. Select `RELEASED {daysAgo} DAYS AGO` and repeat the process, this time using `Days Since Release (incl. release day)` `greater than` `3`.
6. Save the template. In **Library Configuration**, switch on your copy. Switch off **Just Released (Monitored)**, **Released Yesterday (Monitored)**, and **Released Days Ago (Monitored)**.

![JUST RELEASED selected, with its condition under Show conditionally](images/example-release-banner-editor.png)

![The condition on the JUST RELEASED element](images/example-release-banner-condition.png)

The canvas only shows the element whose condition matches the sample poster, and fades the selected element. Don't worry if one seems to be missing. Use **Test Specific Media** with a recent release to check it.

Items up to 3 days old should show `JUST RELEASED`. From day 4 to day 7, they should show the day count on the same banner. Change the `3` in both conditions to move the switch-over day.

The same setup works for wording that changes as a date gets closer. A ribbon could read `COMING NOV 2026`, then `IN 5 DAYS`, then `RELEASING TODAY`.

**Why not use two templates?** Each template is checked separately. If two templates in the same spot both match an item, both get drawn, one on top of the other. The preset banners use 75% **Fill Opacity**, so text from the lower banner shows through the top one:

![Two overlapping banners: the lower one's text shows through](images/example-release-banner-ghosting.png)

If you prefer separate templates, make sure their day ranges don't overlap. Otherwise, set the top banner's **Fill Opacity** to 100.

---

## Episode code on episode cards

This puts the season and episode code, for example `S01E05`, at the bottom right of each episode's title card. It is the same badge Kometa's Episode Info overlay draws.

### With the preset

1. Open the **Overlay Templates** tab and check that **Episode Info - Episode Card** is there. You don't need to change it.
2. Open **Library Configuration**, open your TV library with **Configure Overlays**, and switch on **Episode Info - Episode Card**.
3. In the **Sync scope** table, tick **Episode cards** under **Full sync**, **Quick sync**, or both. Episode cards are off by default, and nothing is drawn on them until you tick this. This is the step people miss.
4. Click **Save Configuration**. Then on the **Library Configuration** tab click **Full Overlays Sync**, then **Confirm Full Sync?**. Or use **Test Specific Media** to try a few episodes first.

Each episode card gets a rounded black badge, about 60% opaque, with the code in white. A season 1 episode 5 shows `S01E05`, and the first Special shows `S00E01`. An episode with no season or episode number from Plex gets no badge.

Every episode in the library gets its own card, and each one is drawn and uploaded to Plex. On a big library the first full sync takes a long time. Try a few episodes with **Test Specific Media** first.

If you use Posterizarr title cards, the badge is drawn over them, the same as the episode rating presets.

### By hand, or in a different spot

Presets can't be edited, so to move the badge or change its look, **Duplicate** **Episode Info - Episode Card**, switch the preset off for the library, and switch your copy on. Or start from a new template:

1. Create a template, tick **Episode card** under **Artwork Targets**, then untick **Main poster**. Tick **Episode card** before you add any elements, so the canvas switches to 1920 x 1080.
2. Add a **Tile** and place it where you want the badge. The preset uses **X Position** 1600, **Y Position** 945, a width of 305 and a height of 105, with a corner radius of 30.
3. Add a **Variable** element. Set **Width** to 305, **Height** to 105, and **X Position** and **Y Position** to match the tile. **Text Align** is already **Center**. Set **Font Size** to 55 and **Font Weight** to **Normal** to match the preset. A new Variable starts with an **IMDb Rating** segment: delete it with its bin icon. Then click **Add Text** and change `TEXT` to `S`, click **Add Variable** and pick **Season Number**, click **Add Text** and type `E`, then click **Add Variable** and pick **Episode Number**.
4. On both variable segments, set **Number Format** to **Pad to 2 digits (05)**. Use **Pad to 3 digits (005)** on **Episode Number** for shows with hundreds of episodes.
5. Turn on **Check Layout** to make sure the code fits inside the badge, then save and switch the template on for the library as above.

With **Number Format** left on **Plain**, the same template shows `S1E5`. See [Number Format](Overlay-Variables.md#number-format).

