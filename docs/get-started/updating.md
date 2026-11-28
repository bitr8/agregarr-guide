# Updating and image tags

Pull a newer image, recreate the container, and choose which release line you want to follow.

## Before you start

- Agregarr is running from a Compose file (see [Install with Docker Compose](install-docker.md)).
- Your config folder is mounted. An update replaces the container, not that folder.

## Update

1. Open the folder containing your `compose.yml` and pull the image.

    ```sh
    docker compose pull agregarr
    ```

2. Recreate the container.

    ```sh
    docker compose up -d agregarr
    ```

On Unraid, pull the image and bring the stack up again from its Compose Manager entry.

<!-- OPEN: NEW: Compose Manager update button labels need a live check -->

## Check the update

The sidebar badge shows **Agregarr Stable** with the version number, or **Agregarr Develop** with the short commit ID. It has a grey background and no arrow. Click the badge to open **Settings > About**.

When a newer build is available, the badge turns orange and shows an up arrow.

On **Agregarr Develop**, the badge shows how far behind the current build is (for example **3 commits behind**). It reads **Out of Date** when the build has dropped out of the recent commit list Agregarr checks.

On `:develop`, a build you've just pulled can still show a few commits behind, because changes that only touch docs don't produce a new image. If the short commit ID under **Settings > About** changed after `up -d`, the update worked.

## Image tags

All image tags are under `bitr8/agregarr`.

- `:latest` follows stable releases. Use this unless you have a reason to follow another tag.
- `:2.13.0` (or any other release number) stays on that release until you change the `image:` line.
- `:develop` is rebuilt from the develop branch as code changes land. It can break and is amd64 only. Release tags support amd64 and arm64.

To switch tags, change the `image:` line in your Compose file, then run both update commands again.

This guide tracks `:develop`. If you use `:latest`, some options covered here may not have reached your release yet.

## What happens at start-up

Each time the container starts, Agregarr upgrades its own data. There's nothing to run by hand.

- The database is migrated to the new version.
- Old settings are converted to the new layout when a release changes them.
- Source colours and the default poster template are added if they're missing.
- Preset overlay templates are rewritten from the release on every start. Agregarr won't save edits to a preset at all, so copy the preset into a custom template before changing it.

Agregarr also keeps the last good copy of `settings.json` as `settings.json.last-good` in the config folder.

## What it doesn't do

- Agregarr doesn't update itself. Nothing changes until you pull a newer image.
- The badge doesn't handle rollbacks. To go back, set a pinned tag such as `:2.13.0` and recreate the container. Start-up migrations change the database, so an older release may not work after a newer one has run.

## If the update didn't work

- **The badge still shows the old build.** Compare the commit ID under **Settings > About** with the one from before the update. If it hasn't changed, the container wasn't recreated, so run the `up -d` command again.
- **No update arrow ever appears.** Agregarr checks GitHub for new releases or commits. If the container can't reach GitHub, it can't report them. Check the container's internet access.
- **Settings disappeared after the update.** The config folder isn't mounted. See [Install with Docker Compose](install-docker.md).

## Related

[Install with Docker Compose](install-docker.md) · [Health Checks](../Health-Checks.md)
