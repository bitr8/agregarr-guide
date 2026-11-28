# Install on Unraid

This gets Agregarr running on your Unraid server so you can sign in with Plex.

## What you need

- Unraid with Docker enabled and the Community Applications plugin installed. The plugin adds the **Apps** tab.
- Plex Media Server 1.43.3.10861 or newer.

There are two ways to install it. The Community Applications template is the quickest. If you manage containers with the Compose Manager plugin, jump to [Install with Compose Manager](#install-with-compose-manager).

## Set it up

The Community Applications template uses the upstream Agregarr image by default. Change it to `bitr8/agregarr`, then add two variables for file permissions.

1. Open the **Apps** tab, search for `Agregarr`, and click **Install** on the listing named `aggregarr` (the template's spelling).

    **Check:** a container settings form opens with a **Config directory** path and a **webUI Port** of `7171`.

2. Change the **Repository** field to `bitr8/agregarr:latest`.

    **Check:** the field contains `bitr8/agregarr:latest` and nothing else.

3. Set **Time Zone** to your time zone, for example `Australia/Sydney`. The template starts with `America/New_York`.

    **Check:** **Time Zone** shows your own value.

4. Click **Add another Path, Port, Variable, Label or Device** at the bottom of the form. Set **Config Type** to **Variable**, **Key** to `PUID`, and **Value** to `99`. Click **Add**.

    `99` is Unraid's `nobody` user. The template doesn't set it, and the image needs it so files written by Agregarr belong to the same user as the rest of your shares.

    **Check:** the new **PUID** variable appears in the form.

5. Repeat step 4 with **Key** set to `PGID` and **Value** set to `100`. This is Unraid's `users` group.

    **Check:** the form lists both **PUID** and **PGID**.

6. Leave **Movies directory (Optional)** and **TV Shows directory (Optional)** empty for now. They're only needed for Coming Soon placeholders, which you can add later.

7. Click **Apply**.

    **Check:** the **Docker** tab lists `aggregarr` as started.

8. Click the container's icon on the **Docker** tab and choose **WebUI**. On the first step of the Setup page, find **Sign in with Plex** and click **Sign In**.

    **Check:** the Plex sign-in window opens.

### Later: placeholder folders

Coming Soon placeholders need folders that both Agregarr and your media apps can access. Agregarr creates the movie and season folders there, then copies a placeholder video into them.

When you're ready, open the container's icon menu, choose **Edit**, and fill in **Movies directory (Optional)** and **TV Shows directory (Optional)** with your own share paths, for example `/mnt/user/media/placeholders/movies`. The template mounts these as **Read Only**, so change each **Access Mode** to **Read/Write**, then click **Apply**. See [Placeholders](../Placeholders.md).

## Install with Compose Manager

Already using the Compose Manager plugin (or Compose Manager Plus)? You can run Agregarr as a stack instead. Pick one install route, not both.

1. Open the **Docker** tab, then **Compose** (added by Compose Manager), and click **Add New Stack**. Name it `agregarr`.

    **Check:** the stack appears in the list.

2. Click the stack's icon, choose **Edit Stack** and paste this into the editor:

    ```yaml
    services:
      agregarr:
        image: bitr8/agregarr:latest
        container_name: agregarr
        volumes:
          - /mnt/user/appdata/agregarr:/app/config
        environment:
          - TZ=Australia/Sydney
          - PUID=99
          - PGID=100
          - UMASK=022
        ports:
          - 7171:7171
        restart: unless-stopped
    ```

    Change `TZ` to your time zone. Leave `PUID=99` and `PGID=100` alone. Agregarr stores its settings in `/mnt/user/appdata/agregarr`.

3. Save the file, then click **Compose Up** on the stack.

    **Check:** the **Docker** tab lists `agregarr` as started.

4. Open `http://YOUR_SERVER:7171`, replacing `YOUR_SERVER` with your Unraid server's address. On the first step of the Setup page, click **Sign In** under **Sign in with Plex**.

    **Check:** the Plex sign-in window opens.

For placeholder folders on this route, add lines under `volumes:` using your own share paths, for example `/mnt/user/media/placeholders/movies:/data/movies`, then run **Compose Up** again.

## What you built

Agregarr runs as one container on port `7171`, with its settings stored in your appdata share. Recreating or updating the container leaves those settings alone.

If the Setup page shows a banner saying the `/app/config` volume mount wasn't configured properly, the config path is missing or wrong. For the template route, check **Config directory**. For the Compose route, check the appdata line under `volumes:`. Apply the template or run **Compose Up** again.

## Next

[Updating and image tags](updating.md)
