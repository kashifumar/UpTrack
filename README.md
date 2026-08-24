# UpTrack

**Track smarter. Bid better.**

UpTrack is a browser extension for freelancers on Upwork, available for Firefox, Chrome, and Edge. It tracks which job listings you have already reviewed and automatically hides them from future search results — so every time you search, you only see fresh, unreviewed jobs.

Built by [Kashif Umar](https://www.linkedin.com/in/kashif-umar/) &nbsp;·&nbsp; [GitHub](https://github.com/kashifumar) &nbsp;·&nbsp; [X](https://x.com/kashif_umar)

---

## Contents

- [The Problem It Solves](#the-problem-it-solves)
- [Features](#features)
- [How It Works](#how-it-works)
- [Search Preferences & URL Mapping](#search-preferences--url-mapping)
- [Installation](#installation)
- [Known Limitations](#known-limitations)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Challenges Solved](#challenges-solved)
- [Privacy & Legal](#privacy--legal)
- [Privacy Policy](#privacy-policy)
- [Terms of Use](#terms-of-use)
- [Disclaimer](#disclaimer)

---

## The Problem It Solves

Freelancers on Upwork often review dozens of job listings in a session. The platform provides no built-in way to mark a listing as reviewed, bid on, or skipped — every search refresh presents the same cards again. This leads to redundant reading, missed opportunities, and decision fatigue.

This extension fixes that by persisting a decision against every job ID you interact with and acting on it automatically on future visits.

---

## Features

### Job Tracking

Mark any job with one of three statuses directly from the job detail page:

| Status | What it does |
|---|---|
| ✓ Bid Done | You submitted a proposal. Job is hidden from future searches. |
| ✗ Skip | You decided not to bid. Job is hidden from future searches. |
| ⏱ Check Later | Still undecided. Job remains visible in search results. |

### Smart Search Filtering

- Already-reviewed jobs are automatically hidden the moment search results load
- Only fresh, unreviewed listings and Check Later jobs remain visible
- Works on both the main search results page and Upwork's slide-out job panel
- Save your preferred Upwork search filters inside the extension
- Open a pre-filtered Upwork search with one click

### Skill Categories

Toggle pre-defined skill categories to focus your search:

- Laravel / PHP
- Database Architecture
- ERP / Business Systems
- API Development
- Node.js / Express
- Python / AI Integration
- Small Scoped Jobs

### Data Portability

- Export your full job history and preferences to a JSON file
- Import that file back into any browser where UpTrack is installed — merges by job ID, keeping the most recently updated record
- Export your job list to Excel at any time

### Privacy First

- All data is stored locally in your browser using `browser.storage.local`
- No account required
- No job titles, search history, or personal data ever leave your machine
- Only one anonymous analytics ping is sent on install — see [Privacy & Legal](#privacy--legal)

### Excel export

- One-click `.xlsx` export of the full tracked list via SheetJS

### Zero dependencies

- Pure vanilla JS — no bundler, no npm, no build step

---

## How It Works

### On the Search Page

Each job card is scanned automatically. Cards you have already marked **Bid Done** or **Skip** are hidden immediately on every future visit. Cards marked **Check Later** stay visible with a status indicator.

### On a Job Detail Page

The same three buttons appear in a toolbar injected into the job listing. Click one to mark the job — the button confirms your choice. The toolbar re-injects itself if Upwork's Vue frontend re-renders the page, and works correctly in Upwork's slide-out detail panel without requiring a full page navigation.

### The Popup

Click the extension icon to open the UpTrack dashboard — five tabs:

**Jobs** — Live stats (total tracked / bid / skipped / later), filterable by status, searchable by title. Each row has a status dropdown and a remove button. Export to Excel or reset everything from the footer.

**Preferences** — Every field auto-saves as you change it. Set your keywords, experience level, job type, rate ranges, proposal count, client filters, location, project length, hours per week, and sort order. Hit **Open Search with These Filters** to open a matching Upwork search URL in one click.

**Categories** — Seven toggleable skill-category chips for your own reference. A suggest-a-category field is included in the code but disabled in this release.

**Export / Import** — Export everything (jobs + preferences + active categories) to a single JSON file, or import a previously exported file. Also a dedicated Excel export for jobs only.

**About** — Version, author links, and legal links.

---

## Search Preferences & URL Mapping

The **Open Search with These Filters** button in the Preferences tab builds a live Upwork search URL from your saved preferences. Confirmed parameter mappings:

| Preference | Upwork Query Param | Notes |
|---|---|---|
| Search Keywords | `q` | |
| Sort Order | `sort` | `recency` = Newest First, `relevance` = Best Match |
| Payment Verified | `payment_verified=1` | Omitted entirely when unchecked |
| Experience Level | `contractor_tier` | `1`=Entry, `2`=Intermediate, `3`=Expert — comma-joined |
| Job Type | `t` | `0`=Hourly, `1`=Fixed — comma-joined if both |
| Hourly Rate Range | `hourly_rate` | Format: `min-max` |
| Fixed Price Minimum | `amount` | Format: `min-` (open-ended) |
| Number of Proposals | `proposals` | e.g. `0-4,5-9,10-14` |
| Client Location | `location` | Comma-separated region and/or country names |

**Not yet mapped** — saved as preferences and functional in the UI, but no confirmed Upwork query parameter exists for them yet: Client History, Project Length, Hours per Week, and Job Duration.

---

## Installation

UpTrack is available on all major browser extension stores:

| Browser | Store |
|---|---|
| Firefox | [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/uptrack/) |
| Chrome | [Chrome Web Store](https://chromewebstore.google.com) |
| Edge | [Microsoft Edge Add-ons](https://microsoftedge.microsoft.com/addons) |

> Store links will be updated as each version goes live. Firefox is available now.

---

## Known Limitations

- **Unmapped filters** — Client History, Project Length, Hours per Week, and Job Duration are saved in preferences but not yet included in the generated search URL. No confirmed Upwork query parameter exists for these yet.
- **URL format assumptions** — The fixed-price and proposals range formats are best-effort and not exhaustively verified against every possible Upwork filter bucket.
- **DOM dependency** — The extension relies on Upwork's current page structure. If Upwork updates their frontend, CSS selectors may need to be updated. The relevant selectors are documented in the content script source.
- **Disabled features** — Suggest-a-Category, Cloud Sync, and AI Job Scoring are built into the code but intentionally disabled (`// DISABLED_V1`). They point at placeholder API endpoints that are not live yet.

---

## Architecture

```
uptrack/
├── manifest.json          # MV3 manifest — host permissions + content script config
├── background.js          # Service worker — anonymous analytics ping on install/startup
├── db/
│   └── db.js              # Storage layer — browser.storage.local wrapper
├── content/
│   ├── search.js          # Search results page — hides already-marked cards
│   ├── detail.js          # Job detail page — injects action toolbar
│   └── content.css        # Injected styles — prefixed with .ujt- to avoid collisions
├── popup/
│   ├── popup.html         # Extension popup — five tabs
│   ├── popup.js           # Popup logic — render, filter, search, export, reset
│   ├── popup.css          # Popup styles
│   └── xlsx.full.min.js   # SheetJS bundled locally (MV3 CSP blocks external CDN scripts)
└── icons/
    ├── icon-48.png
    └── icon-96.png
```

### Storage Layer (`db/db.js`)

All persistence goes through `browser.storage.local` — **not** `indexedDB`. This is a deliberate architectural choice.

Content scripts run in the origin of the host page (`https://www.upwork.com`), so any `indexedDB` call from a content script writes to Upwork's database, not the extension's. The popup runs in the extension origin (`moz-extension://...`), so it would read from a completely separate database and see zero records. `browser.storage.local` is scoped to the extension and accessible from all contexts — eliminating this split-brain problem entirely.

### Content Scripts

Two separate scripts handle two separate responsibilities:

**`search.js`** — runs on `/nx/search/jobs` and `/search/jobs`. Fetches the set of hidden job IDs from storage on load, populates a cache, and uses a `MutationObserver` on `document.body` to watch for new cards as the user scrolls or filters. Cards already in the hidden set are collapsed immediately. A `cacheReady` flag gates all card processing until the async storage fetch completes — preventing a race condition where cards are stamped as processed against an empty cache and never re-evaluated. Does not inject any buttons.

**`detail.js`** — runs on `/jobs/*`. A dedicated `MutationObserver` waits for `section.air3-card-section` to appear, since Upwork's Vue pages hydrate asynchronously and `DOMContentLoaded` fires long before job content exists. Once the container is found, the toolbar is injected. A second guard observer watches for Vue re-rendering removing the toolbar and automatically re-injects it. Also handles Upwork's slide-out detail panel (`/nx/search/jobs/details/~ID`) which changes the URL without a full page navigation.

### SPA Navigation Detection

Upwork is a Vue Router single-page application. Standard browser events like `popstate` and `DOMContentLoaded` are unreliable for detecting route changes. Both scripts use a persistent `MutationObserver` on `document.body` that compares `location.href` on every DOM mutation — catching every client-side navigation regardless of how Vue triggers it.

### URL Pattern Handling

Upwork uses two job URL formats:

- **Short:** `/jobs/~022047269079846210513/`
- **Slug:** `/jobs/Technical-Project-Manager_~022047269079846210513/`

All selectors and regex patterns account for both. The job ID is extracted by matching `/(~[0-9a-zA-Z]+)/` anywhere in the URL.

---

## Tech Stack

| Layer | Choice | Reason |
|---|---|---|
| Extension API | WebExtensions (MV3) | Compatible with Firefox, Chrome, and Edge — single codebase |
| JS | Vanilla ES2020 | No build step, no bundler, no dependencies |
| Storage | `browser.storage.local` | Shared across content scripts, popup, and service worker — no origin-scoping issues |
| DOM observation | `MutationObserver` | Only reliable way to detect Vue SPA route changes and async content rendering |
| Spreadsheet export | SheetJS (`xlsx`) | Bundled locally — MV3 CSP blocks external CDN scripts in extension pages |
| Styles | Plain CSS with `.ujt-` prefix | Avoids collisions with Upwork's class names |

---

## Challenges Solved

Building this extension against Upwork's Vue.js frontend surfaced several non-obvious problems worth documenting.

**Vue SPA navigation** — Upwork's frontend gives no reliable navigation events, and a content script cannot be re-injected by manifest matches alone when the URL changes client-side. Solved with a persistent `MutationObserver` that diffs `location.href` on every DOM mutation, catching every client-side route change regardless of how Vue triggers it.

**Vue async rendering** — The detail page shell loads long before the job content exists in the DOM, so `DOMContentLoaded` is useless for knowing when to inject. Solved with a dedicated `MutationObserver` that waits specifically for the target container to appear before injecting the toolbar.

**Vue re-render wiping injected UI** — Vue's virtual DOM reconciliation can silently remove nodes that external scripts have injected. Solved with a toolbar guard observer that detects removal and automatically re-injects.

**IndexedDB origin split** — Content scripts run in the host page origin (`upwork.com`), so IndexedDB writes go to Upwork's database, not the extension's. The popup runs in the extension origin and would read from a completely separate database, seeing zero records. Solved by using `browser.storage.local` from the start, which is extension-scoped and shared across all contexts.

**Cache-before-DOM race condition** — The `MutationObserver` fires card-processing callbacks before the async storage fetch completes, causing cards to be stamped as already-processed against an empty cache and never re-evaluated. Solved with a `cacheReady` flag that gates all scanning until storage has loaded.

**MV3 Content Security Policy** — Manifest V3 extension pages block scripts loaded from external CDNs. Solved by bundling SheetJS locally rather than loading it from a CDN URL.

**Dual job URL formats** — Upwork uses both short (`/jobs/~ID`) and slug-style (`/jobs/Title_~ID`) URLs interchangeably. All selectors and regex patterns are written to match both without branching logic.

**Double-encoded location names** — Upwork's `location` filter silently drops multi-word country or region names unless the space is pre-encoded as a literal `%20` before the whole query value is URL-encoded, causing it to arrive as `%2520`. Confirmed against live Upwork search and handled in the URL builder accordingly.

---

## Privacy & Legal

UpTrack collects one anonymous ping when the extension is installed or opened. This contains: browser name and version, operating system, country (derived from your IP at request time — the IP itself is not stored), extension version, and an anonymous timestamp.

Everything you track — jobs, statuses, preferences, categories — is stored locally in your browser using `browser.storage.local`. None of it is ever transmitted anywhere.

Full details below.

---

## Privacy Policy

### What we collect

- Browser name and version
- Operating system
- Country — derived from your IP address at request time. The IP address itself is not logged or stored.
- Extension version
- An anonymous session timestamp

### What we do NOT collect

- Job titles or any job content
- Personal data of any kind
- Your search history or saved preferences
- Any information that could identify you as an individual

### How data is used

Collected data is used solely for aggregate analytics — understanding which regions use the extension and which browsers need to be supported. It is never used to identify an individual user and never influences the product in a user-specific way.

### Data retention

Aggregated data is retained for 90 days. Raw request data is discarded immediately after the country is derived.

### Third parties

None. Data is not shared with, sold to, or processed by any third party.

### Contact
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Kashif%20Umar-blue?logo=linkedin)](https://www.linkedin.com/in/kashif-umar/)
[![GitHub](https://img.shields.io/badge/GitHub-kashifumar-181717?logo=github)](https://github.com/kashifumar)
[![X](https://img.shields.io/badge/X-@kashif__umar-black?logo=x)](https://x.com/kashif_umar)

---

## Terms of Use

- UpTrack is provided as-is, free of charge, for personal productivity use.
- The user is solely responsible for complying with Upwork's own Terms of Service while using this extension.
- The developer is not liable for any action taken by Upwork against a user's account as a result of using this extension.
- The extension may stop functioning if Upwork makes changes to their website structure. No guarantee is made regarding continued compatibility.
- There is no guarantee of uptime, feature availability, or continued maintenance of the extension.
- The developer reserves the right to discontinue the extension at any time without notice.

---

## Disclaimer

- UpTrack is an independent tool and is not affiliated with, endorsed by, sponsored by, or connected to Upwork Inc. in any way.
- "Upwork" is a registered trademark of Upwork Inc. All rights belong to their respective owners.
- All job listings and data displayed by this extension are sourced from Upwork's own public website. UpTrack does not generate, modify, or misrepresent any job listing content.

---

*Last updated: August 2026*
