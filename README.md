# Afghan Telecom Pro — live data

Live package data for the **Afghan Telecom Pro** app (AWCC, Roshan, e&, ATOMA, Salaam).
The app's in-app **Update (تازه کول)** button downloads `packages.json` — price and
code changes reach users **without a new APK**.

## Live file

- https://raw.githubusercontent.com/ajmalbrk/afghan-telecom-pro-data/main/packages.json

## Current version

- **Version 1** — last checked **2026-11-22**
- **224 packages:** AWCC 84, Roshan 15, e& 67, ATOMA 23, Salaam 35
- 203 verified, 15 secondary-reported (Roshan monthly GB↔price mapping, from roshan.af),
  6 unverified (price not published — the app shows «معلومات نشته», never an invented price).
- Salaam 30GB monthly = **999 AFN** (`*740*30*1#`). There is no 30GB package for 250 AFN on any network.

## How to update a price (admin — no new APK needed)

1. Edit `packages.json`: change the price / code, bump `"version"` by 1,
   set `"last_checked"` to today's date.
2. Upload the new file (from the project folder):
   `python3 github_data.py put packages.json data/packages.json "v2: <what changed>"`
3. Users open Afghan Telecom Pro and tap **تازه کول / Update** — the new data
   downloads and replaces the old prices. The app checks the file first and
   keeps the old data if the new file is invalid or the internet is off.

Rules for the data file — never break these:

- **Nothing hypothetical.** Only prices/codes from the networks' official sites.
  Anything unverified stays `"status": "unverified"` with no price/code.
- **Names:** `e&` (not Etisalat), `ATOMA` (not MTN).
- **ATOMA 13GB / 25GB monthly are excluded** — both share the code `*344*37#`
  (conflict). Show them only as a conflict note, never guess which one the code buys.
- **e& data is SMS-to-3378 only** — e& has no USSD data-activation codes.
- The app never uploads anything: favourites and history stay on each phone only.
  Update is download-only.

## File format

`packages.json` is one JSON object:

```json
{
  "app": "Afghan Telecom Pro",
  "version": 1,
  "last_checked": "2026-11-22",
  "verified_label": "22 نومبر 2026",
  "sources": { "awccNet": "https://afghan-wireless.com/...", "...": "..." },
  "networks": [ { "id": "awcc", "en": "AWCC", "full": "Afghan Wireless",
                  "ps": "افغان وایرلس", "color": "#0057b8", "dark": "#003f86",
                  "logo": "bundled" } ],
  "packages": [ { "id": 1, "net": "awcc", "type": "internet", "cat": "daily",
                  "label": "110MB", "gb": 0.11, "price": 10, "val": "1d",
                  "act": "*477*1*1#", "cancel": "*477*1*2#", "remain": "*477*1*4#",
                  "sms": null, "smsCancel": null, "remainSms": null,
                  "scope": null, "combo": false, "note": null, "actNote": null,
                  "src": "https://afghan-wireless.com/...", "status": "verified" } ],
  "services": { "awcc": [ { "k": "balance", "code": null, "note": "noteAwccBal" } ] },
  "compare": { "sizes": [1, 5, 10, 13, 25, 30], "data": { "1": {...}, "...": {...} } },
  "coverage": [ { "net": "roshan", "ps": "...", "fa": "...", "en": "...",
                  "src": "roshan.af/about-us", "url": "https://roshan.af/about-us" } ],
  "notes": { "pkg": {...}, "list": {...}, "sms": {...} }
}
```

Package `status` is one of `verified` | `secondary` | `unverified`.
`null` for price/act/cancel/remain always renders as «معلومات نشته» in the app.
