# OFP Briefing — Offline iPad App (PWA)

An easy-reading flight briefing generated from a Thai AirAsia OFP PDF.
Everything runs **on your iPad**. After the first load it works **fully offline** —
no servers, no internet, no data ever leaves the device.

---

## What's in this folder

```
index.html        the whole app (UI + PDF reader + parser)
sw.js             service worker — makes it work offline
manifest.json     home-screen app metadata
icon-192.png      app icons
icon-512.png
pdfjs/            the PDF-reading engine (Mozilla pdf.js, vendored locally)
```

Total size ~1.4 MB. Nothing here calls the internet.

---

## How to get it onto your iPad

A PWA has to be *served* once (it won't run from a `file://` path because the
service worker needs a real origin). You have three easy options — pick one.

### Option 1 — GitHub Pages (free, permanent, recommended)
1. Create a free GitHub account.
2. Make a new repository, e.g. `ofp-brief`.
3. Upload **all** the files in this folder (keep the `pdfjs/` folder structure).
4. Repo → **Settings → Pages** → Source: `main` branch, `/root`. Save.
5. After a minute you get a URL like `https://yourname.github.io/ofp-brief/`.
6. Open that URL in **Safari on the iPad**.

### Option 2 — Any static host
Netlify Drop (drag the folder onto app.netlify.com/drop), Cloudflare Pages,
or any web host. Upload the folder, open the URL in Safari.

### Option 3 — Run on your own Mac for a moment
On a Mac in this folder: `python3 -m http.server 8000`, then on the iPad
(same Wi-Fi) open `http://<mac-ip>:8000/`. Add to Home Screen (next step) and
it stays offline afterward — you won't need the Mac again.

---

## Install as an app (the offline magic)

1. Open the URL in **Safari** (must be Safari, not Chrome, on iOS).
2. Tap the **Share** button → **Add to Home Screen** → **Add**.
3. An "OFP Brief" icon appears on your home screen.
4. Open it once while online so it can cache itself.
5. **Now turn on Airplane Mode and open it again — it still works.**

From then on: tap the icon → choose a flight-plan PDF → read the briefing.
Offline, every time.

---

## Using it

- Tap the app icon.
- Tap **"choose a flight-plan PDF"** and pick your OFP (from Files, email,
  Crew app — anywhere on the iPad).
- Swipe between sectors using the tabs at the top.
- Tap **New PDF** to load a different packet.

Per sector you get: route, EET, STD/STA, ground time to next sector
(red if ≤30 min), turbulence from EDR with named hotspots, company NOTAMs,
and departure/arrival NOTAMs only — plain-English and colour-coded
(red = runway/ILS closures, amber = U/S or restrictions).

---

## Honest limitations

- **Format-specific.** Tuned to this Thai AirAsia LIDO OFP layout. A different
  dispatch system's PDF would need parser tweaks.
- **EDR turbulence is advisory.** It reads the EDR column only; it does **not**
  read the SIGWX charts (those are images). Always cross-check SIGWX/SIGMET.
- **NOTAM filtering** drops permanent AIRAC trigger notams to cut noise. For
  anything official, verify against the full packet.
- **Plain-English decoding** expands common ICAO contractions; a few rare ones
  pass through unchanged.

This is a personal briefing aid, not an approved operational document.

---

## Updating the app later

If I improve the parser, you replace `index.html`, and bump the cache name in
`sw.js` (change `ofp-brief-v1` to `-v2`). The app auto-updates next time it's
opened online.
