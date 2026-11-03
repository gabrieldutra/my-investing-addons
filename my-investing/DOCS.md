# My Investing

The app listens on `127.0.0.1` only. To open it from your devices over HTTPS, publish it with the
**Tailscale** app as a Tailscale Service.

## One-time setup

0. The image is private. On GitHub, create a personal access token (classic) with only `read:packages`, then in
   Home Assistant add it under **Settings → Apps → App store → ⋮ → Registries**: registry `ghcr.io`, your GitHub
   username, and the token as the password.
1. In the Tailscale admin console, **DNS** page: make sure MagicDNS and HTTPS certificates are on.
2. **Access controls**: add a tag owner for the tag the Home Assistant machine will use, e.g.
   `"tagOwners": { "tag:homeassistant": ["autogroup:admin"] }`.
3. **Services** page: define a service named `investing` (port 443, HTTPS).
4. In the **Tailscale** app's configuration, set:
   ```yaml
   advertise_tags:
     - tag:homeassistant
   services:
     - name: svc:investing
       target: http://127.0.0.1:8742
       protocol: https
       port: 443
   ```
   and restart it. Then approve this machine as the service's host on the **Services** page.
5. In this app's configuration, set `public_url` to the service address, e.g.
   `https://investing.<your-tailnet>.ts.net`, and start it.
6. Open the **Log** tab: it shows a one-time setup code. Open the address above, enter the code and create
   your account.

## Data and backups

Everything lives in this app's data folder, so it's part of Home Assistant backups. The app also keeps its
own daily database copies (the newest 14). Keep Home Assistant backups somewhere other than the SD card.
