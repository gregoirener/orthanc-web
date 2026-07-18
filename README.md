# Orthanc Web Console

The browser-based operator console for [Orthanc](https://github.com/) — a
single-target, gated WiFi audit rig built on an **ESP32-S2**. This is a
**zero-backend static site**: it talks to the ESP32 directly over USB using the
[Web Serial API](https://developer.mozilla.org/docs/Web/API/Web_Serial_API), so
everything runs locally in your browser. No data leaves the machine.

> **Responsible use.** Orthanc is an **authorized-testing** tool. Active WiFi
> transmission (deauth) and handshake/PMKID capture are off until you explicitly
> arm them, single-target only, and never broadcast / jam / spam. Deauth is an
> illegal denial-of-service except on networks you own or are authorized to test.
> Follow the law in your jurisdiction.

## What it does

- Live WiFi scan table — pick one AP as the target.
- Two-gate arming (AUTHORIZED acknowledgement + exact-BSSID confirm) mirrored in
  the UI before any active command is enabled.
- `auto` (wifite-style chain) and `advanced` (one primitive at a time) attack
  menus driving the ESP over the Orthanc Wire Protocol.
- Live client list for the target, raw-log console with capture highlighting, and
  in-browser PCAP assembly (`.pcap`, `LINKTYPE_IEEE802_11`) for Wireshark /
  `hcxpcapngtool`.

## Requirements

- A **Chromium** browser (Chrome / Edge / Brave) — Web Serial is not in Firefox
  or Safari.
- **HTTPS** (or `localhost`). Web Serial only works in a secure context — Vercel
  serves HTTPS by default, so a deployed build just works.
- An Orthanc ESP32-S2 on USB.

## Run locally

It's a single file — open it any way you like over a secure context:

```bash
# simplest: just open index.html in Chrome (file:// is a secure context)
open -a "Google Chrome" index.html

# or serve it (localhost is also a secure context)
python3 -m http.server 8000   # then visit http://localhost:8000
```

Click **Connect**, choose the ESP32 serial port, and go.

## Deploy to Vercel

This is a static site with no build step.

**Via the dashboard:** push this repo to GitHub, then in Vercel *Add New →
Project → Import* it. Framework Preset **Other**, Build Command **empty**, Output
Directory **`.`** (root). Deploy.

**Via the CLI:**

```bash
npm i -g vercel
vercel          # preview deploy
vercel --prod   # production
```

## Keeping it in sync

`index.html` here is a deploy mirror of `web-dashboard/index.html` in the main
Orthanc repo. When the console changes there, copy the file over:

```bash
cp ../orthanc/web-dashboard/index.html ./index.html
```

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).
