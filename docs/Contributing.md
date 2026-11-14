# Contributing

You can edit these docs. If something's wrong, out of date, or harder to follow than it should be, send a fix. Small changes are welcome too: a typo, a clearer sentence, or a step I skipped because I've done it a hundred times.

## The quick way

You don't need to install anything.

1. Open the page you want to change and click the edit icon at the top right of the page.
2. GitHub opens the page's Markdown file. Sign in if it asks, then make your change.
3. Click **Commit changes** and then **Propose changes**. GitHub makes a copy of the docs under your account and opens a pull request from it.

I read every pull request. If I change something before merging it, I'll explain why.

## What helps most

- Corrections. If the page says one thing and the app does another, that's the most useful thing you can tell me.
- Missing steps, especially ones that only crop up on a setup different from mine.
- Examples from a real library: a condition that worked, a schedule you settled on, or a layout worth copying.
- Screenshots. Crop out server names, usernames, and anything else you wouldn't post publicly.

Not sure whether a change is wanted? Open the pull request anyway and mention it in the description. A half-finished fix gives me more to work with than a report saying something's wrong somewhere.

## Bigger changes

For a new page or a rewrite, preview it locally first. You'll need [uv](https://docs.astral.sh/uv/).

```bash
git clone https://github.com/YOUR-USERNAME/agregarr-guide
cd agregarr-guide
uv run zensical serve
```

Open `http://localhost:8000`. Pages are Markdown files under `docs/`, and the navigation lives in `zensical.toml`. Every pull request builds automatically, so broken links and pages missing from the navigation show up as failed checks.

A few conventions keep the pages consistent:

- Buttons, fields, and menu items should match the app exactly and appear in bold.
- Give the setting's location before explaining what it does.
- Describe what this app does. Leave other projects' behaviour to their own docs.

## Bugs in the app

These docs describe what the app should do. If it does something else, the app has a bug. Report it at [bitr8/agregarr-dev issues](https://github.com/bitr8/agregarr-dev/issues).

## Licence

The docs use the [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) licence. By sending a pull request, you agree that your change will be published under the same licence.
