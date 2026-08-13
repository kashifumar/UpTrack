# UpTrack

**Track smarter. Bid better.**

UpTrack is a browser extension for freelancers on Upwork. It tracks which job listings you have already reviewed and automatically hides them from future search results — so every time you search, you only see fresh, unreviewed jobs.

Built by [Kashif Umar](https://www.linkedin.com/in/kashif-umar/) &nbsp;·&nbsp; [GitHub](https://github.com/kashifumar) &nbsp;·&nbsp; [X](https://x.com/kashif_umar)

---

## Contents

- [Features](#features)
- [How It Works](#how-it-works)
- [Search Preferences & URL Mapping](#search-preferences--url-mapping)
- [Installation](#installation)
- [Known Limitations](#known-limitations)
- [Privacy & Legal](#privacy--legal)
- [Privacy Policy](#privacy-policy)
- [Terms of Use](#terms-of-use)
- [Disclaimer](#disclaimer)

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

1. Clone or download this repository
2. Open Firefox and navigate to `about:debugging#/runtime/this-firefox`
3. Click **Load Temporary Add-on**
4. Select the `manifest.json` file from the project directory
5. Navigate to `https://www.upwork.com/nx/search/jobs` to start using the extension

> **Note:** Temporary add-ons are removed when Firefox closes. For a permanent install the extension needs to be signed via the [Mozilla Add-on Hub](https://addons.mozilla.org/developers/).

---

## Known Limitations

- **Unmapped filters** — Client History, Project Length, Hours per Week, and Job Duration are saved in preferences but not yet included in the generated search URL. No confirmed Upwork query parameter exists for these yet.
- **URL format assumptions** — The fixed-price and proposals range formats are best-effort and not exhaustively verified against every possible Upwork filter bucket.
- **DOM dependency** — The extension relies on Upwork's current page structure. If Upwork updates their frontend, CSS selectors may need to be updated. The relevant selectors are documented in the content script source.
- **Disabled features** — Suggest-a-Category, Cloud Sync, and AI Job Scoring are built into the code but intentionally disabled (`// DISABLED_V1`). They point at placeholder API endpoints that are not live yet.

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
[Kashif Umar](https://www.linkedin.com/in/kashif-umar/)
[github.com/kashifumar](https://github.com/kashifumar)

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
