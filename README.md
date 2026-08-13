# UpTrack

A browser extension that brings workflow discipline to Upwork job hunting. Mark listings as **Bid Done**, **Skip**, or **Check Later** directly from the search results page or job detail view, build a reusable Upwork search-filter URL from a real preferences panel, and never waste time reading the same listing twice.

Available for three browsers, built from the same core codebase:

| Browser | Folder | Status |
|---|---|---|
| 🦊 Firefox | [`firefox/`](firefox/) | Original build |
| 🟢 Chrome | [`chrome/`](chrome/) | Port — `chrome.*` via a compatibility shim |
| 🔵 Edge | [`edge/`](edge/) | Port — identical to Chrome (same Chromium platform) |

Each folder is a complete, independent, installable extension with its own `README.md` covering full feature details, architecture, and install instructions for that browser. Start there.

---

## What It Does

- **Auto-hides** jobs you've already marked *Bid Done* or *Skip* on future search visits.
- **Inline action bar** on every search card and the job detail page — mark a job without leaving where you are.
- **Preferences panel** that builds a real Upwork search URL (keywords, experience level, job type & rate, proposals, client info/history/location, project length, hours/week, sort) — see each browser's README for the exact query-param mapping.
- **Popup dashboard** with live stats, filtering, search, inline status editing, and Excel/JSON export-import.
- **Toggleable skill categories** for your own reference.
- All data — jobs, preferences, categories — stays local to your browser. Nothing you track is ever transmitted anywhere.

---

## Privacy & Legal

Every build ships its own `pages/privacy.html` (identical content across all three), linked from the extension's About tab.

**What's collected**: one anonymous install/startup ping — browser name/version, OS, country derived from IP at request time (the IP itself isn't stored), extension version, and an anonymous session timestamp. Used only for aggregate analytics (regional usage, browser compatibility), retained 90 days aggregated with raw data discarded immediately, never shared or sold to third parties.

**What's never collected**: job titles, personal data, search history, or anything else that could identify you.

**Terms of Use**: provided as-is for personal productivity use; you're responsible for complying with Upwork's own Terms of Service; the developer isn't liable for any account action Upwork takes as a result of using this extension; it may stop working if Upwork changes their site structure; no guarantee of uptime or continued maintenance.

**Disclaimer**: UpTrack is an independent tool, not affiliated with, endorsed by, or connected to Upwork Inc. in any way. "Upwork" is a trademark of Upwork Inc. All job data displayed comes from Upwork's own public website.

Contact: github.com/kashifumar

---

## Repository Layout

```
upwork-search-filter/
├── firefox/    # Firefox extension (MV3, browser.* API) — see firefox/README.md
├── chrome/     # Chrome extension (MV3, chrome.* via shim) — see chrome/README.md
├── edge/       # Edge extension (MV3, chrome.* via shim) — see edge/README.md
└── docs/       # Original build prompts / working notes
```

Each extension folder also has its own `CONTEXT.md` documenting build/porting decisions in detail, for anyone picking this project back up later.

---

## Author

**KASHIF UMAR**
[LinkedIn](https://www.linkedin.com/in/kashif-umar/) · [GitHub](https://github.com/kashifumar) · [X](https://x.com/kashif_umar)
© 2025 All rights reserved. Unauthorized reproduction is not permitted.
