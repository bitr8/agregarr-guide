# Home, Recommended and Library explained

Agregarr has three pages named **Home**, **Recommended** and **Library**. They match three places in Plex, with one wrinkle: two share the same order.

## What are the three places in Plex?

- **Your Home.** The server owner's Home screen. In Agregarr, this is the **Server Owner Home** box.
- **Other people's Home.** The Home screen for everyone you share the server with. This is the **Users Home** box.
- **The library's Recommended tab.** This is the **Library Recommended** box.

Each library also has a Collections tab listing all its collections. A collection appears there regardless of which boxes you tick. Plex sorts this tab by sort title. Agregarr gives a promoted collection a sort title such as `!001_Trending Today`, placing it ahead of the rest. Everything else sorts by name.

## How do Agregarr's pages map onto them?

- **Home** lists rows with **Users Home** or **Server Owner Home** ticked.
- **Recommended** lists rows with **Library Recommended** ticked.
- **Library** lists every collection and imported Plex collection in the library, using the Collections tab's order. Plex's own default hubs don't appear here.

## Why is the order shared but not visibility?

Plex keeps one ordered list of rows for each library. Agregarr stores one position per row to match it. **Home** and **Recommended** both read and write that position, so they share the same order.

Visibility works differently. Each row has three separate boxes, and each page filters using its own box. A row with only **Library Recommended** ticked appears on **Recommended**, where you can drag it into position. Tick **Users Home** as well and it also appears on **Home**. **Recommended** then greys it out as **Ordering controlled in Home tab**, so you reorder it on **Home** instead.

**Library** keeps its own order. Dragging a collection there never moves anything on **Home**.

## How does a Home for one person work?

Set **Target User** on a collection, then tick **Users Home**. Agregarr adds label filters to every other user's sharing restrictions, so only the target user sees the collection.

Plex doesn't apply those restrictions to the owner's account, so the owner still sees every collection. To check what a user sees, sign in with a second Plex account that you share the server with. Plex Media Server versions before 1.43.1 ignore label restrictions on Home, so other users can still see the row there.

## What is the difference between Plex Default, collections and Pre-Existing?

- **Collections** are made by Agregarr from a source. Agregarr owns their contents, name, artwork and position.
- **Plex Default** hubs are Plex's own rows, such as Recently Added. Agregarr controls where they appear, their order and when they show. You can hide one, but you can't delete it.
- **Pre-Existing** collections already exist in Plex and weren't made by Agregarr. Agregarr controls where they appear, their order, artwork, sort title and any **Collection Suffix** added to the name. It doesn't change which titles they contain.

All three use the same list for each library, so you can place a collection between two hubs.

## What does Agregarr own?

For every row it manages, Agregarr owns the visibility boxes and order. It writes both to Plex during each full sync, which undoes changes made through Plex's own Home settings. Agregarr places its rows after the row Plex keeps at the top, then puts any unmanaged rows below them.

A Pre-Existing collection with a label listed under **Exclude from Ordering (Plex Label)** is the exception. Agregarr leaves its order, visibility and sort title alone.

## Related

- [Order the rows on Plex Home](order-home.md)
- [Shuffle the Home order](shuffle-home.md)
- [Collection pages](../collections/pages.md)
- [Visibility and time restrictions](../collections/form/visibility.md)
- [Adopt collections and hubs you already have](../collections/adopt-existing.md)
- [Promote collections above the A-Z list](promote-library.md)
- [Show a different set of collections on Home](rotation.md)
