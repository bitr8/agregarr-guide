# Health Checks

There are 18 diagnostics for connections, configuration, and background jobs. You'll find them under **Health Checks** on the **About** tab of **Settings**. For general setup, see the [upstream docs](https://agregarr.org).

## The checks

Every check finishes as OK, warning, error, or skipped. Skipped means the check doesn't apply to your setup, for example a check for a service you haven't configured.

| Name | What it checks | Warning or error |
|------|----------------|------------------|
| Plex Connection | Plex answers with the admin token. Reports the server name and version. | Error if Plex is unreachable or no admin user is set. The message hints at the cause (refused, timeout, expired token, SSL). Skipped until Plex is set up. |
| Plex Version | The Plex Media Server version. | Error on a build before 1.43.3.10861, which has known exploited vulnerabilities. Update Plex. Skipped when the version cannot be read. |
| Sonarr Connection | Each Sonarr server with sync turned on. | Warning if some are unreachable, error if all are. Skipped when there are none. |
| Radarr Connection | The same, for Radarr. | Same as Sonarr. |
| TMDB Connection | TMDB answers. | Warning. |
| Ratings Proxy | The ratings service (api.agregarr.org) answers with a valid reply. | Warning. |
| Cloudflare Solver Connection | Each configured solver answers. See [Integrations](services/cloudflare-solvers.md). | Warning if some are unreachable, error if all are. A solver that is busy with a challenge counts as reachable. Skipped when none is configured. |
| Cloudflare Solver | Networks collections have a working solver. | Error if you have a Networks collection and no solver. Also error for 24 hours after a solver reached FlixPatrol and was still refused. Skipped without Networks collections. See [below](#the-cloudflare-solver-check). |
| Letterboxd Cloudflare | Letterboxd has not been blocked by Cloudflare in the last 8 hours. | Warning, with the setting to change. Skipped when **Use Cloudflare Solver for Letterboxd** is on. |
| Maintainerr Connection | The Maintainerr API answers with a collection list. | Error if it fails, warning on an unexpected reply. Skipped when Maintainerr is not configured. |
| Seerr Placeholder Patterns | Seerr holds the two placeholder ignore patterns. See [below](#the-seerr-check). | Warning. Skipped when Seerr is not connected or no collection creates placeholders. |
| Orphaned Collection Keys | A collection config points at a Plex collection that no longer exists. See [below](#orphaned-collection-keys). | Warning. Error if Plex is unreachable. |
| Plex Libraries | Library IDs used by collections and overlays still exist in Plex. | Warning. |
| Overlay Templates | Enabled overlays use templates that still exist. | Warning, naming up to three libraries. |
| Data Directory | The config folder and its database folder are writable. A real write and delete is tried. | Error. The message points at volume mounts and PUID/PGID. |
| Placeholder Folders | Each placeholder folder is on a mounted volume, not inside the container. | Warning, naming the folders. Quiet outside Docker, or when no placeholder folders are set. |
| Timezone | `TZ` is set, valid, and not UTC when Sonarr is connected. See [below](#the-timezone-check). | Error if invalid, warning if unset or UTC. |
| Job Health | Collections Sync and Quick Sync have run lately and last succeeded. See [below](#job-health). | Warning. Skipped when no collections exist. |

---

## How it works

- **Overall status**: any error makes the status red. A warning makes it yellow, otherwise it's green. Muted checks don't count.
- **The sidebar badge**: when something's wrong, the sidebar shows **1 health issue** or **N health issues** and links to the About tab. Muted checks aren't included in the count.
- **Schedule**: checks run every 6 hours, about a minute after startup, and when you save Plex, Sonarr, or Radarr settings. Saving solver settings doesn't trigger another check. Press **Run Now** on the About tab to run everything at once. You can change the 6 hour schedule through the **Health Checks** job on **Settings > Jobs**.
- **Turning it off**: clear **Enable Health Checks** in **Settings > General**. That removes the section from the About tab.
- **Time limit**: a check that doesn't finish in time counts as failed. Most checks get 10 seconds. Ratings Proxy and Cloudflare Solver Connection get 15 seconds, while their own requests give up after 12 seconds and 10 seconds per solver. Orphaned Collection Keys gets whichever is larger: 30 seconds or half a second per collection key.
- **Connection flakes**: connection checks for Plex, Sonarr, Radarr, TMDB, Ratings Proxy, Cloudflare Solver Connection, and Maintainerr warn after the first failure. Two failures in a row make them errors. One successful check resets the count.
- **Muting**: press **Mute** to dim a check and leave it out of the overall status and sidebar badge. The check still runs. Press **Unmute** to put it back. Mutes are saved.

---

## Fix hints

When a Sonarr, Radarr, Cloudflare Solver Connection, or Maintainerr check breaks, its message starts with a hint based on the failure, followed by the raw error:

- **Rejected key or proxy (401, 403)**: "Access denied. Check the API key or any reverse proxy in front of the service"
- **404**: "Got a 404. Check the URL base"
- **Server error (5xx)**: "The service returned an error. Check its logs"
- **SSL**: "SSL error. Check "Use SSL" and the service's certificate"
- **`localhost` address**: "If Agregarr runs in Docker, localhost points back to the Agregarr container. Use the server IP or container name"
- **Nothing listening**: "Nothing answered at that address. Check the host, port, and that the service is running"
- **Name won't resolve**: "Hostname won't resolve. Use the IP, or put both containers on the same Docker network"
- **Timeout**: "No reply in time. Check the network or firewall between Agregarr and the service"

TMDB and Ratings Proxy failures point you at the container's internet access and DNS. After a timeout or server error, they also mention that it may just be a short outage. Orphaned Collection Keys, Plex Libraries, and Job Health include the next step, as described below.

---

## The Cloudflare Solver check

Networks collections read FlixPatrol, which sits behind a Cloudflare challenge. Without a solver, this check reports an error because the built-in browser can't reliably get through that challenge. Add FlareSolverr, Byparr, or Trawl under **Settings > Sources**.

The check also turns red if a solver reaches FlixPatrol but Cloudflare still refuses it. That covers HTTP 403, 429, or 503 responses, along with challenges the solver couldn't solve. It stays red for 24 hours after the last refusal, or until a fetch works.

The message suggests sending the solver through a VPN, since FlixPatrol may be challenging your server's IP address. It also points to MDBList lists as another option. This state only lives in memory, so restarting Agregarr clears it.

---

## The Seerr check

This check runs only when Seerr is connected and at least one collection creates placeholders for missing items. It's tied to **Keep placeholder ignore patterns in Seerr** in the Seerr settings. See [Integrations](services/seerr.md).

- **Warning, Seerr does not have the setting**: shown only when the checkbox is on. The message names the Seerr build that added it.
- **Warning, patterns missing**: Seerr rejected one or both patterns while the checkbox was on, or Seerr has the setting but the patterns aren't there while the checkbox is off. Tick the box or add them by hand.
- **OK**: both patterns are present.

The check is skipped if Seerr doesn't answer within 8 seconds or the request breaks.

---

## The timezone check

Sonarr reports air dates in UTC. Agregarr uses the server timezone to turn them into calendar days for "airs today", placeholder dates, and overlay boundaries. If `TZ` isn't set, Docker defaults to UTC. That shifts an 8pm US episode to 1am the next day in UTC, and the dates move with it.

The check warns when `TZ` is unset or UTC and a connected Sonarr server has sync turned on. Set `TZ` in your container environment to fix it.

```yaml
environment:
  - TZ=Australia/Sydney
```

An invalid `TZ` is an error because a typo quietly falls back to UTC. Use IANA names such as `America/New_York`.

---

## Orphaned collection keys

A config's stored rating key points to a Plex collection that no longer exists, or to something that isn't a collection. This usually happens after you delete the collection in Plex or rebuild a library.

The message lists up to five collection names. The next sync puts them back. If you deleted them on purpose, delete the config in All Collections as well.

Configs marked missing are skipped. So are inactive configs with **Remove from Plex when inactive**. If Plex can't be reached, the check errors with "orphan status not assessed" instead of pretending everything's clean.

---

## Job health

Job Health watches two jobs: Collections Sync, expected at least every 24 hours, and Quick Sync, expected every 2 hours. It warns for each job when:

- the job has been running longer than its expected window. This only reports the long run. It doesn't stop the job.
- the last finished run failed. Skipped runs don't count. The message includes the start of the error and points to the full error in **Settings > Logs**.
- the last successful run finished longer ago than the expected window. The message says how long ago it ran and points to **Settings > Jobs**.

A job that's never run isn't flagged. These windows are fixed, so you'll get a warning if you schedule a job less often than that.

### Job history

Every job run is saved with its start time, duration, outcome (success, error, skipped or running), and error text. Agregarr keeps the last 50 runs for each job, and the history survives restarts.

- **Settings > Jobs** shows a **Last Run** badge for each job: **Success**, **Error**, **Skipped**, **Running** or **Never**. It also shows how long ago the run finished. Hover over the badge for the error text or run time.
- The dashboard's **Job Status** card shows the latest run for each job except the two full syncs. Quick syncs also show how many items were matched and added, or processed.

A skipped run means there was nothing to do, or another job was already running.
