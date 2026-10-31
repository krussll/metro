# Smooth Commutes (`metro`) — Project Overview & Agent Context

**Repository**: `krussll/metro`  
**Live Site**: [https://www.smoothcommutes.com](https://www.smoothcommutes.com)  
**Active Production Branch**: `gh-pages`  
**Hosting Platform**: GitHub Pages  
**Engine**: Jekyll 4.4.x (Static Site Generator)

---

## 1. Executive Summary & Purpose

**Smooth Commutes** is a static web application built on Jekyll designed to deliver real-time public transit alerts, line status updates, planned service disruptions, and interactive route navigation to daily commuters.

### Core Objectives
1. **Real-Time Status Aggregation**: Provide rapid, zero-friction status updates for subway riders without requiring app downloads or bloated interfaces.
2. **Planned Disruption Calendars**: Surface upcoming track work, weekend re-routings, skip-stop schedules, and station closures.
3. **Route Navigation & Itineraries**: Present line maps, terminal destinations, and complete stop-by-stop listings for directional travel.
4. **Monetization & Partnerships**: Monetized via Google AdSense (`ads.txt`), Ezoic display ads (`_layouts/page.html`), affiliate disclosures (`affiliate-disclaimer.md`), and official agency data transparency (`data-partners.md`).

---

## 2. Technology Stack & Architecture

| Component | Technology | Details / Configuration |
| :--- | :--- | :--- |
| **Static Site Generator** | Jekyll 4.4.1 | Liquid templating, SCSS compilation, Markdown parsing |
| **Hosting & Deployment** | GitHub Pages (`gh-pages`) | Custom domain via `CNAME` (`www.smoothcommutes.com`) |
| **Styling Framework** | Bootstrap 4.0.0-alpha.6 | Grid system, tabs, navigation, responsive utilities |
| **Iconography & Fonts** | FontAwesome 5.8.1 & Google Fonts | "Rubik" font family (400, 500, 700 weights) |
| **Client-Side Scripts** | jQuery 3.x, Masonry, Lazysizes | Dynamic time rendering, masonry card grids, lazy image loading |
| **Analytics & Verification** | Google Analytics 4 (GA4) | Measurement ID `G-6LXL5E75PW`, Google Search Console verification |
| **Monetization** | Google AdSense / Ezoic | Publisher ID `pub-8543572621618524` |

---

## 3. Autonomous Data Pipeline & Cron Automation (CRITICAL)

> [!IMPORTANT]
> **Active Background Bot Warning**:
> An external automated scraping/ingestion service commits to the `gh-pages` branch approximately **every 15 minutes** with the commit message `New route data`. 

### Automated Ingestion Files
The 15-minute ingestion job pushes updates directly to:
1. **`assets/updated.json`**:
   ```json
   {"lastUpdated": 1760976010625}
   ```
2. **`_includes/{route_code}-status.html`**:
   Pre-rendered HTML badges indicating whether a route has `Good Service`, delays, or alerts (e.g., `_includes/1-status.html`, `_includes/A-status.html`).
3. **`_includes/{route_code}-future.html`**:
   Pre-rendered HTML listing future planned work, weekend changes, and advisory notices with expected start/end times.
4. **`_data/{route_code}_status.json` / `_data/{route_code}_future.json`**:
   Raw JSON / HTML backup state tracked in `_data/`.

### Client-Side Timestamp Hydration
In `_layouts/post.html`, `_layouts/post-chicago.html`, and `_layouts/post-dc.html`, client-side JavaScript asynchronously fetches `assets/updated.json` on load and updates the `Last Updated:` label in the user's local or city-specific timezone:
- **NYC / DC**: `America/New_York`
- **Chicago**: `America/Chicago`

```javascript
fetch('{{ site.baseurl }}/assets/updated.json')
  .then((response) => response.json())
  .then((alertData) => {
    const date = new Date(alertData.lastUpdated);
    var dateEl = document.getElementById("post-date");
    dateEl.textContent = "Last Updated: " + date.toLocaleString("en-US", {timeZone: "America/New_York"});
  });
```

### Ingestion Contract for Developers & Agents
- **DO NOT** rename or delete `assets/updated.json`.
- **DO NOT** alter the file naming pattern of `_includes/{route_code}-status.html` or `_includes/{route_code}-future.html` without coordinating with the upstream crawler.
- When committing changes, always pull remote changes first (`git pull --rebase` or `git pull`) because new automated commits land constantly on `origin/gh-pages`.

---

## 4. Repository Structure & Key Files

```
metro/
├── .git/                      # Git repository (active branch: gh-pages)
├── CNAME                      # Custom domain pointer (www.smoothcommutes.com)
├── README.md                  # Minimal repo notes
├── _config.yml                # Jekyll site configuration & authors
├── _data/                     # 30 JSON snapshot files for line alerts and statuses
├── _drafts/                   # Draft storage (empty placeholder)
├── _includes/                 # 170+ template partials
│   ├── {code}-status.html     # Live status badge for route {code}
│   ├── {code}-future.html     # Future disruptions for route {code}
│   ├── {code}-0.html          # Stop list for outbound / direction 0
│   ├── {code}-1.html          # Stop list for return / direction 1
│   ├── sidebar.html           # MTA agency description sidebar
│   ├── postbox.html           # Grid card thumbnail for individual lines
│   ├── featuredbox.html       # Featured post card layout
│   └── (boilerplate leaks)    # Legacy includes (newsletter, useful, recommended)
├── _layouts/                  # Master templates
│   ├── default.html           # HTML5 shell, head metadata, navbar, footer, scripts
│   ├── post.html              # NYC subway line detailed status template
│   ├── post-chicago.html      # Chicago CTA 'L' line detailed status template
│   ├── post-dc.html           # Washington D.C. Metrorail detailed status template
│   ├── post-left-sidebar.html # Alternate article layout with sidebar
│   ├── page.html              # Static page layout (About, Partners, etc.)
│   └── archive.html           # Category/tag archive list
├── _posts/                    # 37 Markdown route post files
│   ├── 2023-10-14-nyc-*.md    # 23 NYC Subway routes (1..7, A..Z, SIR)
│   ├── 2023-10-14-chi-*.md    # 8 Chicago CTA routes (Blue, Red, Brown, etc.)
│   └── 2023-10-14-dc-*.md     # 6 Washington D.C. routes (Blue, Orange, Silver, etc.)
├── _sass/                     # SASS partials
│   ├── config.scss            # Theme color variables ($nav-background, $footer-background)
│   └── _syntax.scss           # Code syntax highlighting
├── assets/
│   ├── css/
│   │   ├── custom.scss        # SCSS entry point (compiles to custom.css)
│   │   └── theme.css          # Core theme styles, card styles, stops styling
│   ├── fonts/                 # Local font assets
│   ├── images/
│   │   ├── {code}.jpg         # Route badges for NYC lines (1.jpg, A.jpg, etc.)
│   │   ├── {code}-CHI.jpg     # Chicago route badges (Blue-CHI.jpg, etc.)
│   │   ├── {code}-DC.jpg      # Washington DC route badges (BL-DC.jpg, etc.)
│   │   ├── chi_subway_map.png # Chicago system map
│   │   ├── washington_dc_subway_map.png # DC system map
│   │   ├── logo.png           # Smooth Commutes brand logo
│   │   └── nyc-subway/        # 23 detailed line route maps for NYC
│   ├── js/
│   │   ├── jquery.min.js      # jQuery library
│   │   ├── masonry.pkgd.min.js# Dynamic card masonry layout
│   │   ├── lazysizes.min.js   # Responsive lazy loading
│   │   ├── theme.js           # Header scroll behavior, back-to-top, smooth scroll
│   │   └── search.js          # Lunr.js search implementation (stale/non-functional)
│   └── updated.json           # Live timestamp file updated by bot
├── about.md                   # /about — About Smooth Commutes
├── contact.md                 # /contact — Contact details
├── data-partners.md           # /data-partners — MTA & transit agency acknowledgments
├── affiliate-disclaimer.md    # /affiliate-disclaimer — Amazon Associates disclaimer
├── privacy.md                 # /privacy — Detailed privacy policy
├── terms.md                   # /terms — Terms & Conditions
├── all-posts.md               # /all-posts — Grid index of all posts
├── index.html                 # Homepage (currently displays NYC subway grid)
├── feed.xml                   # RSS 2.0 feed
├── sitemap.xml                # XML sitemap
├── robots.txt                 # Search bot crawling rules
└── ads.txt                    # Google AdSense authorized sellers file
```

---

## 5. Current Transit Coverage & Frontmatter Schema

### Transit Network Coverage
| Metro Area | Agency | Routes Count | Route Identifiers | Status |
| :--- | :--- | :---: | :--- | :--- |
| **New York City** | MTA NYC Subway | **23** | `1`, `2`, `3`, `4`, `5`, `6`, `7`, `A`, `B`, `C`, `D`, `E`, `F`, `G`, `J`, `L`, `M`, `N`, `Q`, `R`, `SIR`, `W`, `Z` | **Full**: Live status, future alerts, maps, stops. |
| **Chicago** | CTA 'L' | **8** | `Blue`, `Brown`, `Green`, `Orange`, `Pink`, `Purple`, `Red`, `Yellow` | **Partial**: Status badge active; stops placeholder; static future text. |
| **Washington D.C.** | WMATA Metrorail | **6** | `Blue` (`BL`), `Green` (`GL`), `Orange` (`OL`), `Red` (`RL`), `Silver` (`SL`), `Yellow` (`YL`) | **Partial**: Status badge active; stops placeholder; static future text. |

### Post Frontmatter Architecture
The posts in `_posts/` do not rely on markdown body content (`{{ content }}` is not rendered in post layouts). Instead, pages are entirely assembled from frontmatter attributes and includes:

```yaml
---
layout: post                          # post | post-chicago | post-dc
title: "NYC Subway 1 Train Status And Delays"
author: reece
categories: [ "nyc", "subway" ]       # Categories control URLs & grouping
route: "1"                            # Public route label
route_code: "1"                       # Key used to locate includes & images
city: "NYC"                           # NYC | Chicago | Washington D.C.
description: "NYC 1 Train Status. Is the 1 train running today?"
first_paragraph: "(Broadway-7 Avenue local)"
direction_out: "Manhattan"            # Tab label for outbound stops
direction_back: "Uptown & The Bronx"  # Tab label for return stops
related_trains: "2,3,4,5,6,7"         # Comma-separated routes for cross-linking
map_ref: "https://commons.wikimedia.org/wiki/File:NYCS_map_1.svg"
---
```

---

## 6. Audit of Technical Debt, Bugs & Immediate Fixes Needed

During an in-depth audit of the repository, the following technical issues, copy-paste artifacts, and structural bugs were discovered:

### 1. Broken Card Images on Archive & Category Pages
- **File**: [`_includes/postbox.html`](file:///home/reecewilliams8/dev%20work/repos/metro/_includes/postbox.html#L7)
- **Problem**: The card image assumes `assets/images/{{ post.route }}.jpg`.
  - For NYC (`route: "1"`), `assets/images/1.jpg` exists.
  - For Chicago (`route: "Blue"`), the actual image is `assets/images/Blue-CHI.jpg`, but the template requests `Blue.jpg` (404 Not Found).
  - For DC (`route: "Blue"`), the actual image is `assets/images/BL-DC.jpg`, but the template requests `Blue.jpg` (404 Not Found).
- **Required Fix**: Update `postbox.html` with conditional logic based on `post.categories`:
  ```liquid
  {% if post.categories contains 'chi' %}
    {% assign img_src = post.route | append: '-CHI.jpg' %}
  {% elsif post.categories contains 'dc' %}
    {% assign img_src = post.route_code | append: '-DC.jpg' %}
  {% else %}
    {% assign img_src = post.route | append: '.jpg' %}
  {% endif %}
  <img class="img-fluid lazyload" data-src="{{ site.baseurl }}/assets/images/{{ img_src }}" alt="{{ post.title }}">
  ```

### 2. Leaked Boilerplate from Unrelated Repositories
Because the initial template was cloned from a multi-purpose publisher theme, several includes contain text from other niche websites:
- **`_includes/useful.html`**: Contains links to lawn bowls equipment guides (`jackhighbowls.com`):
  - *Second Hand Woods Buyers Guide*
  - *Lawn Bowls Rules for Beginners*
- **`_includes/recommended.html`**: Contains poker hand combinations (`riverpairs.com`):
  - *AA vs AA*, *AQs vs AA*, *AKs vs AKs*, *All Hand Combinations*
- **`_includes/newsletter.html`**: Contains a form pointing to `wowthemes.us11.list-manage.com` promising a *"weekly free guide to selling digital downloads"*.
- **Post Markdown Bodies**: All 37 post files end with dummy text `fwfwwefw`.

### 3. XML Sitemap & Domain Config Inconsistency
- **File**: [`_config.yml`](file:///home/reecewilliams8/dev%20work/repos/metro/_config.yml#L9) and [`sitemap.xml`](file:///home/reecewilliams8/dev%20work/repos/metro/sitemap.xml#L8)
- **Problem**: In `_config.yml`, `baseurl` is configured with the full origin (`https://www.smoothcommutes.com`), while `url` is undefined.
  In `sitemap.xml`:
  ```xml
  <loc>{{ site.url }}{{ post.url }}</loc>
  ```
  Since `site.url` is empty, `<loc>` outputs relative URLs (e.g. `/nyc/subway/nyc-subway-1-train-status-and-delays`), which violates the XML Sitemap protocol and causes Google Search Console indexing rejections.
- **Problem**: Root pages (`/`, `/about`, `/contact`, `/data-partners`, `/terms`, `/privacy`) are not included in `sitemap.xml`.
- **Problem**: `robots.txt` does not reference the sitemap (`Sitemap: https://www.smoothcommutes.com/sitemap.xml`).

### 4. Layout Copy-Paste Errors for Chicago & D.C.
- In [`_layouts/post-chicago.html`](file:///home/reecewilliams8/dev%20work/repos/metro/_layouts/post-chicago.html#L43) and [`_layouts/post-dc.html`](file:///home/reecewilliams8/dev%20work/repos/metro/_layouts/post-dc.html#L42):
  - `title="Map of the {{ page.route }} Train in NYC subway"`
  - `alt="Map of the {{ page.route }} Train in NYC subway"`
  - These should read `in Chicago 'L'` and `in Washington D.C. Metro` respectively.

### 5. Incomplete Chicago & DC Stop Lists
- `_includes/BL-0-DC.html`, `_includes/Blue-0-CHI.html`, etc. contain empty `<li> <span></span></li>` tags. Commuters looking for stop itineraries in Chicago or DC see empty lists.

### 6. Orphaned City Content from Homepage & Navbar
- In [`index.html`](file:///home/reecewilliams8/dev%20work/repos/metro/index.html#L55), the homepage strictly loops over `site.categories.nyc`. Visitors landing on the homepage cannot see or navigate to Chicago or Washington D.C. subway status pages.
- The top navigation bar in [`_layouts/default.html`](file:///home/reecewilliams8/dev%20work/repos/metro/_layouts/default.html#L67) has an empty `<li></li>` tag and lacks dropdowns or direct links to cities or line directories.

### 7. Non-Functional Search Infrastructure
- `assets/js/search.js` attempts to load `/search_data.json` and initialize `lunr.js`.
- No `search_data.json` exists in the repo, `lunr.js` is not loaded in `default.html`, and `_includes/search-form.html` submits to `/search`, which returns a 404.

### 8. HTML Validity & Duplicate IDs
- Both the post header and `_includes/{route}-future.html` output `<time id="post-date">`. Multiple identical IDs on the same page violate HTML standards and cause DOM ambiguity in client-side scripts.

### 9. Branch Synchronization (`main` vs `gh-pages`)
- `gh-pages` is the true live branch containing all site code and 64,500+ automated commits.
- `main` is completely stale and abandoned. Any work should be branched from or committed directly to `gh-pages`.
- There is currently no `.gitignore` file, causing risk of committing `.jekyll-cache/` or `_site/` during local builds.

---

## 7. Context & Guardrails for Making Improvements

When implementing changes or new features on this codebase, follow these established rules:

### A. Guarding the Ingestion Pipeline
- **Never rename** include files following the `{route_code}-status.html`, `{route_code}-future.html`, or `{route_code}-0.html` / `{route_code}-1.html` patterns.
- If styling or markup of alert boxes is updated, ensure CSS classes match the classes injected by the bot:
  - `.line-alerts`
  - `.line-alert`
  - `.text-success`, `.text-warning`, `.text-danger`
  - SVG check and warning icons

### B. Safe Local Development & Verification
- Jekyll 4.4.1 is installed on the local system.
- Build locally to verify syntax without creating uncommitted build artifacts:
  ```bash
  jekyll build -d /tmp/metro-test-build
  rm -rf /tmp/metro-test-build
  ```
- Always ensure `.gitignore` excludes `_site/`, `.sass-cache/`, `.jekyll-cache/`, and `.jekyll-metadata`.

### C. Git Workflow
- Work on branch `gh-pages`.
- Before pushing or committing, run `git fetch origin gh-pages` to incorporate the 15-minute route updates.

---

## 8. Prioritized Strategic Roadmap

### Phase 1: High-Priority Bug Fixes & Hygiene (Immediate)
1. **Fix Postbox Images**: Update `_includes/postbox.html` to resolve image file paths correctly across NYC, Chicago, and DC routes.
2. **Standardize URLs in `_config.yml` & `sitemap.xml`**:
   - Set `url: "https://www.smoothcommutes.com"` and `baseurl: ""` in `_config.yml`.
   - Update `sitemap.xml` to include static pages with proper absolute URLs.
   - Reference `sitemap.xml` in `robots.txt`.
3. **Purge Leaked Boilerplate**: Replace lawn bowls and poker links in `_includes/useful.html` and `_includes/recommended.html` with real transit resources (MTA Weekender, CTA Bus Tracker, WMATA Trip Planner, MetroCard/OMNY guides).
4. **Fix Copy-Paste City Labels**: Correct image alt/title tags in `post-chicago.html` and `post-dc.html`.
5. **Create `.gitignore`**: Ignore `_site/`, `.jekyll-cache/`, and temporary build outputs.

### Phase 2: Information Architecture & City Hubs (Short Term)
1. **Multi-City Homepage**: Reorganize `index.html` with tabbed or sectioned access to **NYC Subway**, **Chicago 'L'**, and **Washington D.C. Metrorail**.
2. **Navbar Navigation**: Add a city dropdown or header links: "NYC Subway", "Chicago 'L'", "DC Metro", and "Transit Maps".
3. **Populate Missing Stop Lists**: Fill in the stop sequence HTML for Chicago and Washington D.C. lines (`BL-0-DC.html`, `Blue-0-CHI.html`, etc.).

### Phase 3: E-E-A-T & SEO Enhancement (Medium Term)
1. **Structured Data Modernization**:
   - Implement `TransitLine` / `Service` / `FAQPage` JSON-LD schema instead of generic `BlogPosting`.
2. **Author Profile & Expertise**:
   - Complete `site.authors.reece` with a transit-focused bio, avatar, and experience credentials.
3. **High-Intent FAQs & Route Context**:
   - Replace empty `fwfwwefw` post bodies with substantive route guides: transfer points, operating hours, express vs local patterns, and accessibility notes.

### Phase 4: Modern Features & Search (Long Term)
1. **Functional Transit Search**: Implement client-side instant search for stations, lines, and terminal points.
2. **Notification & Favorite Line Bookmarking**: Enable localStorage-based "Save My Line" for quick alert checks on return visits.
3. **Dark Mode & Performance Audit**: Optimize CSS and modernize Bootstrap to reduce render-blocking resources.
