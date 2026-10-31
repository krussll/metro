# Smooth Commutes (`metro`) — Comprehensive SEO Audit & Strategic Improvement Plan

**Target Domain**: [https://www.smoothcommutes.com](https://www.smoothcommutes.com)  
**Repository**: `krussll/metro`  
**Hosting & Framework**: GitHub Pages (`gh-pages` branch) | Jekyll 4.4.1  
**Audit Date**: October 31, 2026  
**Document Location**: `agents/seo_review.md`  

---

## Executive Summary

**Smooth Commutes** is a high-utility static web application providing real-time transit alerts, planned service disruption notices, and route navigation across three major United States metropolitan areas: **New York City (MTA)**, **Chicago (CTA)**, and **Washington D.C. (WMATA)**. 

The site possesses a tremendous structural SEO advantage: **an automated 15-minute background ingestion pipeline** that updates live line conditions directly on the production branch. This creates dynamic content freshness without database overhead. Furthermore, transit search queries (*"[Line] status"*, *"[Train] delays today"*, *"[Line] planned weekend work"*) exhibit immense recurring monthly search demand with strong local user intent.

However, the site currently suffers from critical technical SEO discrepancies, indexing gaps, heading structure defects, geographic intent contamination, thin content liabilities under Google's Helpful Content System (HCS), and an unexploited off-site authority footprint. Addressing these deficiencies will unlock significant organic search visibility, improve Core Web Vitals, and solidify Google E-E-A-T trust signals.

---

## 1. Existing SEO Strengths & Positives in Place

Before detailing areas for improvement, the audit identified multiple architectural and tactical strengths already established on the platform:

| Dimension | Positive Feature Already in Place | SEO & User Impact |
| :--- | :--- | :--- |
| **Hosting & Delivery** | GitHub Pages CDN backed by Fastly & Varnish | Delivers sub-150ms TTFB globally, high uptime, native HTTP/2, and fully automated SSL certificate renewal. |
| **Automated Freshness** | 15-minute background pipeline updating `_includes/` | Static HTML contains up-to-date alert snippets at build time, ensuring search crawlers parse current transit disruptions without executing heavy client-side JavaScript. |
| **Canonical Redirects** | Root apex to `www` redirection (`301 Moved Permanently`) | Prevents duplicate domain indexing (`smoothcommutes.com` properly 301-redirects to `www.smoothcommutes.com`). HTTP requests cleanly 301 to HTTPS. |
| **Search Intent Permalinks** | Clear semantic URL structure (`/:categories/:title`) | Clean, descriptive slug hierarchies (e.g. `/nyc/subway/nyc-1`, `/chi/subway/chi-Blue`, `/dc/subway/dc-Silver`) that match high-intent transit queries. |
| **Targeted Query Titles** | High-intent title stems in post frontmatter | Post titles specifically target user query syntax: *"NYC Subway 1 Train Status And Delays"*, matching exact daily commuter query syntax. |
| **Cross-Line Internal Links** | `related_trains` frontmatter module | Cross-links interconnected lines (e.g., Line 1 cross-linking Lines 2, 3, 4, 5, 6, 7), creating topical transit clusters and passing crawl equity. |
| **Analytics & Verification** | Google Analytics 4 & GSC meta verification | GA4 measurement ID (`G-6LXL5E75PW`) and Google Search Console ownership verification tag (`3hy6rng49LmxKJ53UCGkhh56RmYZTdFZoHEpkktdxOY`) are present. |
| **Robots Baseline** | Permissive `robots.txt` configuration | No accidental blocking of CSS, JavaScript, or core route paths (`User-Agent: * Disallow:`). |
| **Lazy Loading Integration** | Native `lazysizes.min.js` integration | Post box thumbnails and heavy maps utilize `class="lazyload"` with `data-src` to defer off-screen asset loading. |
| **Compliance Infrastructure** | Established legal and monetization declarations | Includes `privacy.md`, `terms.md`, `affiliate-disclaimer.md`, `data-partners.md`, and authorized digital seller declarations (`ads.txt`). |

---

## 2. In-Depth SEO Audit Findings & Gap Analysis

Despite these positives, several technical, on-page, and architectural issues undermine organic visibility and rank potential.

---

### A. Technical SEO Audit

#### 1. Incomplete XML Sitemap (`sitemap.xml`)
- **Current State**: The live sitemap lists only the 37 individual train route posts. 
- **Defects**:
  - The homepage (`/`), `/about`, `/contact`, `/data-partners`, `/terms`, `/privacy`, and `/all-posts` are **completely absent** from `sitemap.xml`.
  - In `sitemap.xml`, `<loc>{{ site.url }}{{ post.url }}</loc>` relies on `site.url`. In `_config.yml`, `site.url` is omitted (only `baseurl: "https://www.smoothcommutes.com"` is specified). While GitHub Pages injects `site.url` in production builds, any local testing, CI build, or secondary domain setup breaks the `<loc>` tags into relative paths, causing Google Search Console rejection.
  - All URLs in the sitemap have identical `<priority>1.0</priority>` and `<changefreq>hourly</changefreq>`. Flat priority structures dilute crawl focus away from the homepage and high-traffic arterial lines.

#### 2. `robots.txt` Missing Sitemap Declaration
- **Current State**:
  ```
  User-Agent: *
  Disallow:
  ```
- **Defect**: Lacks a `Sitemap: https://www.smoothcommutes.com/sitemap.xml` directive. Search engine crawlers (Googlebot, Bingbot) parsing `robots.txt` are not directed to the sitemap endpoint.

#### 3. Title Tag Inflation & Cross-City Intent Pollution
- **Current State**:
  - In `_layouts/default.html`: `<title>{{ page.title }} | {{site.title_suffix}}</title>`.
  - In `_config.yml`: `title_suffix: "Smooth Commutes | NYC Subway Alerts | Real-Time Updates for Smooth Commutes"`.
- **Resulting Title Tag on NYC Posts (105 characters)**:
  `NYC Subway 1 Train Status And Delays | Smooth Commutes | NYC Subway Alerts | Real-Time Updates for Smooth Commutes`
- **Resulting Title Tag on Chicago Posts**:
  `Chicago Subway Blue Train Status And Delays | Smooth Commutes | NYC Subway Alerts | Real-Time Updates for Smooth Commutes`
- **Resulting Title Tag on Washington D.C. Posts**:
  `Washington D.C. Subway Blue Train Status And Delays | Smooth Commutes | NYC Subway Alerts | Real-Time Updates for Smooth Commutes`
- **Defects**:
  - Google SERP title displays truncate at approximately 55–60 characters (580px width). The critical brand and query suffix is aggressively cut off.
  - Repetitive phrases (*"Smooth Commutes"*, *"NYC Subway Alerts"*, *"Real-Time Updates"*) trigger Google's automated algorithmic title rewriting.
  - **Severe Geographic Intent Contamination**: Commuters searching for the Chicago CTA Blue Line or DC WMATA Silver Line see "NYC Subway Alerts" prominently appended to the title. This confuses search engine geographic classifiers and lowers Click-Through Rate (CTR).

#### 4. Meta Description Truncation & String Concat Bug
- **Current State**:
  In `_layouts/default.html` line 19:
  ```html
  <meta name="description" content="{% if page.description %}{{ page.description }} || {% endif %}{{ site.description }}">
  ```
- **Defects**:
  - Appends ` || NYC Subway Alerts | Real-Time Updates for Smooth Commutese` (which contains a literal typo: **`Commutese`** at line 5 of `_config.yml`).
  - Reaches ~130-150 characters with pipe delimiters, displaying fragmented text in search snippets.
  - City mismatch: Chicago and Washington D.C. lines display *"NYC Subway Alerts"* in their search descriptions.

#### 5. Complete Absence of Social Graph Metadata (Open Graph & Twitter Cards)
- **Current State**: There are zero `og:*` or `twitter:*` tags anywhere in the codebase.
- **Defects**:
  - Links shared on iMessage, Slack, Reddit, X (Twitter), Facebook, or Discord render generic or blank cards without thumbnail badges, summaries, or branded styling.
  - Social traffic signals and referral previews are severely compromised.

#### 6. Broken 404 Image Requests on Cards (`_includes/postbox.html`)
- **Current State**:
  ```liquid
  <img class="img-fluid lazyload" data-src="{{ site.baseurl }}/assets/images/{{ post.route }}.jpg" alt="{{ post.title }}">
  ```
- **Defects**:
  - For NYC (`route: "1"`), `assets/images/1.jpg` exists.
  - For Chicago (`route: "Blue"`), the actual asset is `Blue-CHI.jpg`, but the template requests `Blue.jpg` (404 Not Found).
  - For DC (`route: "Blue"`), the actual asset is `BL-DC.jpg`, but the template requests `Blue.jpg` (404 Not Found).
  - On `/all-posts` and any related route modules, 14 line badges fail with HTTP 404 errors, generating broken image icons and wasting crawl requests.

#### 7. Invalid & Inappropriate Structured Data (`BlogPosting` vs Transit Line)
- **Current State**:
  `_layouts/post.html`, `post-chicago.html`, and `post-dc.html` declare:
  `<div itemscope itemtype="http://schema.org/BlogPosting">`
- **Defects**:
  - A real-time transit line status utility is **not a blog posting**.
  - Google's Structured Data guidelines for `BlogPosting` mandate `headline`, `image`, `datePublished`, `dateModified`, and `author`.
  - In `post.html`, the author block is commented out, dates are rendered via client-side JavaScript, and `publisher` is commented out.
  - This results in missing required field warnings in Google Search Console's Rich Results status reports.
  - Missed opportunity to implement `BreadcrumbList`, `FAQPage`, `Service`, or `WebSite` JSON-LD schemas.

#### 8. Duplicate HTML Element IDs
- **Current State**: Both the post header and `_includes/{route_code}-future.html` output `<time id="post-date">`.
- **Defect**: Duplicate DOM IDs violate W3C HTML5 standards and cause JavaScript selector collision when `document.getElementById("post-date")` executes.

#### 9. Legacy Empty / Dead Code Leaks
- **Current State**:
  - `_includes/useful.html`: Links to lawn bowls equipment guides (`jackhighbowls.com`).
  - `_includes/recommended.html`: Links to poker hand variations (`riverpairs.com`).
  - `_includes/newsletter.html`: Hardcoded MailChimp form from WowThemes promising guides to *"selling digital downloads"*.
  - `assets/js/search.js`: Attempts to load non-existent `/search_data.json` and initialize Lunr.js; `_includes/search-form.html` submits to `/search` (404).

---

### B. Content & On-Page SEO Audit

#### 1. Inverted Heading Hierarchy (H2 -> H3 -> H1)
- **Current State**:
  ```html
  <h2 class="posttitle" itemprop="headline name">{{ page.title }}</h2>
  <h3 class="lead">{{ page.first_paragraph }}</h3>
  <h1 class="lead">{{ page.description }}</h1>
  ```
- **Defects**:
  - The page title is wrapped in an `<h2>` tag, while the secondary descriptive text is wrapped in an `<h1>`.
  - Search crawlers expect a single, semantic `<h1>` tag at the top of the content tree containing the primary keyword target.
  - Inside alert includes, `<h2>` tags are nested under `<h3>Planned Issues`</h3> headings, creating a scrambled document outline.

#### 2. Placeholder Text & Thin Content Risk (Google Helpful Content System)
- **Current State**:
  - All 37 Markdown post files end with dummy text: `fwfwwefw`.
  - Post markdown body content is not even rendered in post layouts (`{{ content }}` is omitted).
  - When a train line has `Good Service` (no active delays), the entire page consists of approximately 45 words of text, an image badge, and an empty stops list.
- **Defects**:
  - Pages with minimal text risk being classified as "thin content" or "soft 404s" by automated quality evaluators.
  - Zero search coverage for high-volume informational queries:
    - *"What stations does the 1 train stop at?"*
    - *"Is the Blue line running express right now?"*
    - *"Where can I transfer from the A train to the L train?"*
    - *"Does the WMATA Silver Line go to Dulles Airport?"*

#### 3. Broken / Empty Stop Lists for Chicago and Washington D.C.
- **Current State**:
  - `_includes/BL-0-DC.html`, `_includes/Blue-0-CHI.html`, etc. contain empty `<li> <span></span></li>` tags.
- **Defects**:
  - Chicago and DC commuters looking for route direction itineraries see blank bullets.
  - Major loss of station-level keyword relevance across Chicago and Washington D.C.

#### 4. Orphaned City Architecture on Homepage & Header Navigation
- **Current State**:
  - The homepage (`index.html`) exclusively iterates over `site.categories.nyc`.
  - The header navigation in `_layouts/default.html` has an empty `<li></li>` and only links to Home and About.
- **Defects**:
  - All 8 Chicago CTA lines and all 6 Washington D.C. WMATA lines are **completely orphaned** from the primary user interface and top navigation.
  - Search engine spiders starting from the homepage only discover NYC routes. Chicago and DC routes can only be discovered if the bot crawls `sitemap.xml` or `/all-posts` (which is not linked in navigation).

#### 5. Layout Copy-Paste City Misattributions
- **Current State**:
  - In `_layouts/post-chicago.html` line 43:
    `alt="Map of the {{ page.route }} Train in NYC subway"`
  - In `_layouts/post-dc.html` line 42:
    `alt="Map of the {{ page.route }} Train in NYC subway"`
- **Defects**:
  - Image alt text on Chicago and DC maps explicitly claims to be NYC subway maps.

#### 6. Nofollow Flags on Internal Legal & Trust Pages
- **Current State**:
  In `_layouts/default.html` lines 125–128:
  ```html
  <li><a href="{{ site.baseurl }}/data-partners" rel="nofollow">Data Partners</a></li>
  <li><a href="{{ site.baseurl }}/affiliate-disclaimer" rel="nofollow">Affiliate Disclaimer</a></li>
  <li><a href="{{ site.baseurl }}/terms" rel="nofollow">Terms and Conditions</a></li>
  <li><a href="{{ site.baseurl }}/privacy" rel="nofollow">Privacy Policy</a></li>
  ```
- **Defects**:
  - Using `rel="nofollow"` on internal links wastes crawl equity and signals to Google that the webmaster does not trust or endorse their own legal/transparency pages.
  - Google's Quality Rater Guidelines heavily scrutinize site transparency. Nofollowing internal trust pages is an anti-pattern.

---

### C. E-E-A-T & Google Search Quality Evaluator Alignment

Because public transit alerts impact daily commuters, work arrivals, and travel safety, transit sites must project high levels of **Experience, Expertise, Authoritativeness, and Trustworthiness (E-E-A-T)**:

| E-E-A-T Component | Current Deficiencies | Strategic Requirement |
| :--- | :--- | :--- |
| **Experience** | Post author link in layout points to `/about-us` (which is a **404 page**; the actual page is `/about`). | Fix author URL to point to a legitimate author bio profile detailing hands-on transit commuting background. |
| **Expertise** | `site.authors.reece` in `_config.yml` has empty `gravatar`, empty `email`, and empty `description`. | Provide author bio, transit credentials, local transit rider credentials, and editorial methodology. |
| **Authoritativeness** | `data-partners.md` has frontmatter `title: About` (wrong title) and only mentions MTA, ignoring CTA and WMATA. | Expand `data-partners.md` into an official "Data Sources & GTFS-RT Methodology" page documenting open data feeds from MTA, CTA, and WMATA. |
| **Trustworthiness** | In `about.md`, paragraph 1 refers to "Smooth Commutes", while paragraph 24 refers to "NYC Subway Alerts". | Unify brand identity across all pages. Provide a clear contact email, business address or legal jurisdiction, and corrections policy. |

---

### D. Off-Site SEO & Authority Analysis

Currently, Smooth Commutes operates in an off-site vacuum with minimal inbound authority signals:

1. **Backlink Profile Deficit**:
   - The domain lacks foundational backlinks from municipal, civic, collegiate, and transit advocacy domains (e.g., Riders Alliance, Active Transportation Alliance, regional transit blogs).
2. **Missing Transit Community Citations**:
   - Commuter subreddits (`r/nycrail`, `r/chicago`, `r/washingtondc`, `r/transit`) and City-Data forums frequently discuss line disruptions, track work, and weekend changes, but Smooth Commutes has no community presence or developer showcase links.
3. **Open Transit Data Showcases**:
   - Both MTA, Chicago Data Portal, and WMATA maintain official developer directories and "Built with Transit Data" galleries where apps utilizing GTFS-RT feeds can request official directory listings. Smooth Commutes is not listed.
4. **Social & Entity Grounding**:
   - No verified social profiles (X/Twitter, Mastodon, Bluesky, LinkedIn) associated with the entity or author. Google Knowledge Graph cannot reconcile the site as an authoritative brand entity.

---

## 3. Prioritized SEO Improvement Plan

---

### Phase 1: High-Priority Technical SEO & Quick Wins (Immediate: Days 1–3)

#### 1. Standardize Site URLs & Clean Configuration (`_config.yml`)
- Update `_config.yml` to define `url` and `baseurl` cleanly.
- Fix typos and condense `title_suffix` to avoid truncation.
- Add complete metadata for author Reece.

```yaml
# Site Identity & Canonical Configuration
name: "Smooth Commutes"
url: "https://www.smoothcommutes.com"
baseurl: ""
title: "Real-Time Subway & Metro Alerts"
title_suffix: "Smooth Commutes"
description: "Real-time subway alerts, planned service disruptions, and line status updates for NYC Subway, Chicago 'L', and Washington D.C. Metro."
meta_description: "Real-time subway alerts, planned service disruptions, and line status updates for NYC Subway, Chicago 'L', and Washington D.C. Metro."
logo: "/assets/images/logo.png"
favicon: "/assets/images/favicon.ico"
permalink: /:categories/:title
future: true

# Author Entity Profile
authors:
  reece:
    name: Reece
    display_name: "Reece Williams"
    email: "reece@reecewilliams.co"
    avatar: "/assets/images/logo.png"
    description: "Daily transit researcher and commuter tracking subway alerts, planned maintenance schedules, and rail service across NYC, Chicago, and Washington D.C."
    url: "/about"
```

#### 2. Overhaul `sitemap.xml` with Full Site Coverage
Replace the post-only loop in `sitemap.xml` with a comprehensive sitemap that includes static pages, city hubs, and posts with weighted priorities:

```xml
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <!-- Core Static Pages -->
  <url>
    <loc>{{ site.url }}/</loc>
    <lastmod>{{ "now" | date_to_xmlschema }}</lastmod>
    <changefreq>hourly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>{{ site.url }}/all-posts</loc>
    <lastmod>{{ "now" | date_to_xmlschema }}</lastmod>
    <changefreq>daily</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>{{ site.url }}/about</loc>
    <changefreq>monthly</changefreq>
    <priority>0.7</priority>
  </url>
  <url>
    <loc>{{ site.url }}/data-partners</loc>
    <changefreq>monthly</changefreq>
    <priority>0.7</priority>
  </url>
  <url>
    <loc>{{ site.url }}/contact</loc>
    <changefreq>monthly</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>{{ site.url }}/privacy</loc>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>{{ site.url }}/terms</loc>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>

  <!-- Route Status Pages -->
  {% for post in site.posts %}
  <url>
    <loc>{{ site.url }}{{ post.url }}</loc>
    <lastmod>{{ "now" | date_to_xmlschema }}</lastmod>
    <changefreq>hourly</changefreq>
    <priority>0.9</priority>
  </url>
  {% endfor %}
</urlset>
```

#### 3. Update `robots.txt`
Point web crawlers directly to the sitemap:
```
User-Agent: *
Allow: /

Sitemap: https://www.smoothcommutes.com/sitemap.xml
```

#### 4. Fix Broken Card Images across Cities (`_includes/postbox.html`)
Update `postbox.html` to dynamically calculate the correct badge filename:

```liquid
<!-- begin post -->
{% assign author = site.authors[post.author] %}

<div class="col-md-6 grid-item">
<div class="card">
    <a href="{{ site.baseurl }}{{ post.url }}">
      {% if post.categories contains 'chi' %}
        {% assign img_src = post.route | append: '-CHI.jpg' %}
      {% elsif post.categories contains 'dc' %}
        {% assign img_src = post.route_code | append: '-DC.jpg' %}
      {% else %}
        {% assign img_src = post.route | append: '.jpg' %}
      {% endif %}
      <img class="img-fluid lazyload" data-src="{{ site.baseurl }}/assets/images/{{ img_src }}" alt="{{ post.title }}">
    </a>
    <div class="card-block">
        <h3 class="card-title"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h3>
        <p class="card-text text-muted">{{ post.description }}</p>
    </div>
</div>
</div>
<!-- end post -->
```

#### 5. Remove `rel="nofollow"` on Internal Links
In `_layouts/default.html`, remove `rel="nofollow"` from footer navigation links (`/data-partners`, `/affiliate-disclaimer`, `/terms`, `/privacy`) to permit internal PageRank distribution.

---

### Phase 2: Metadata Optimization & Social Tags (Days 4–7)

#### 1. Dynamic, City-Specific Title Tags
In `_layouts/default.html`, implement contextual title formatting that eliminates geographic cross-contamination:

```liquid
<title>
  {% if page.url == "/" %}
    Real-Time NYC, Chicago & DC Subway Alerts | Smooth Commutes
  {% elsif page.title %}
    {{ page.title }} | Smooth Commutes
  {% else %}
    Smooth Commutes | Real-Time Public Transit Status & Alerts
  {% endif %}
</title>
```

#### 2. Clean Meta Descriptions
Ensure meta descriptions do not exceed 155 characters and never append conflicting city names:

```liquid
<meta name="description" content="{% if page.description %}{{ page.description }}{% else %}{{ site.description }}{% endif %}">
```

#### 3. Open Graph & Twitter Card Implementation
Inject comprehensive social metadata into `_layouts/default.html`:

```html
<!-- Open Graph / Social Sharing -->
<meta property="og:type" content="{% if page.layout contains 'post' %}article{% else %}website{% endif %}">
<meta property="og:site_name" content="Smooth Commutes">
<meta property="og:url" content="{{ site.url }}{{ page.url }}">
<meta property="og:title" content="{% if page.title %}{{ page.title }} | Smooth Commutes{% else %}{{ site.title }}{% endif %}">
<meta property="og:description" content="{% if page.description %}{{ page.description }}{% else %}{{ site.description }}{% endif %}">
{% if page.route_code %}
  {% if page.categories contains 'chi' %}
    <meta property="og:image" content="{{ site.url }}/assets/images/{{ page.route_code }}-CHI.jpg">
  {% elsif page.categories contains 'dc' %}
    <meta property="og:image" content="{{ site.url }}/assets/images/{{ page.route_code }}-DC.jpg">
  {% else %}
    <meta property="og:image" content="{{ site.url }}/assets/images/{{ page.route_code }}.jpg">
  {% endif %}
{% else %}
  <meta property="og:image" content="{{ site.url }}/assets/images/logo.png">
{% endif %}

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{% if page.title %}{{ page.title }} | Smooth Commutes{% else %}{{ site.title }}{% endif %}">
<meta name="twitter:description" content="{% if page.description %}{{ page.description }}{% else %}{{ site.description }}{% endif %}">
```

---

### Phase 3: Semantic Hierarchy & Structured Data (JSON-LD) (Weeks 2–3)

#### 1. Invert Heading Structure to Proper Hierarchy
In `_layouts/post.html`, `post-chicago.html`, and `post-dc.html`:

```html
<!-- Proper Semantic Outline -->
<div class="mainheading">
  <img width="1280" height="240" class="featured-image img-fluid" src="{{ site.baseurl }}/assets/images/{{ img_src }}" alt="{{ page.title }}">
  
  <h1 class="posttitle">{{ page.title }}</h1>
  <p class="lead font-weight-bold">{{ page.first_paragraph }}</p>
  <p class="lead text-muted">{{ page.description }}</p>
  
  {% include {{status_include}} %}
  
  <p>
    <small><span class="post-date"><time class="live-update-timestamp">Last Updated: Loading...</time></span></small>
  </p>
</div>
```

#### 2. Implement Rich JSON-LD Schemas
Replace deprecated Microdata `itemtype="http://schema.org/BlogPosting"` with modern JSON-LD in `_layouts/default.html`:

##### A. Sitewide Organization & WebSite Graph (in `default.html`):
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "{{ site.url }}/#organization",
      "name": "Smooth Commutes",
      "url": "{{ site.url }}",
      "logo": {
        "@type": "ImageObject",
        "@id": "{{ site.url }}/#logo",
        "url": "{{ site.url }}/assets/images/logo.png",
        "caption": "Smooth Commutes"
      }
    },
    {
      "@type": "WebSite",
      "@id": "{{ site.url }}/#website",
      "url": "{{ site.url }}",
      "name": "Smooth Commutes",
      "description": "{{ site.description }}",
      "publisher": { "@id": "{{ site.url }}/#organization" }
    }
  ]
}
</script>
```

##### B. BreadcrumbList & Service Status Schema on Post Layouts:
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "BreadcrumbList",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "name": "Home",
          "item": "{{ site.url }}/"
        },
        {
          "@type": "ListItem",
          "position": 2,
          "name": "{{ page.city }} Transit",
          "item": "{{ site.url }}/#{{ page.categories[0] }}"
        },
        {
          "@type": "ListItem",
          "position": 3,
          "name": "{{ page.title }}",
          "item": "{{ site.url }}{{ page.url }}"
        }
      ]
    },
    {
      "@type": "Service",
      "name": "{{ page.title }}",
      "serviceType": "Public Transit Route Status",
      "provider": { "@id": "{{ site.url }}/#organization" },
      "areaServed": {
        "@type": "AdministrativeArea",
        "name": "{{ page.city }}"
      },
      "url": "{{ site.url }}{{ page.url }}"
    }
  ]
}
</script>
```

---

### Phase 4: Information Architecture & Multi-City Unification (Weeks 3–4)

#### 1. Multi-City Homepage Redesign (`index.html`)
Transform `index.html` from an NYC-exclusive list into a comprehensive multi-city portal:
- **Top Tabbed Navigation / City Anchor Sections**:
  - **NYC Subway** (23 Lines)
  - **Chicago 'L'** (8 Lines)
  - **Washington D.C. Metro** (6 Lines)
- Surfacing all three metropolitan areas ensures Googlebot discovers all 37 routes from the root URL.

#### 2. Navbar Navigation Links (`_layouts/default.html`)
Update the desktop and mobile navbar:
- `Home` (`/`)
- `NYC Subway` (`/#nyc`)
- `Chicago 'L'` (`/#chicago`)
- `DC Metro` (`/#dc`)
- `All Lines` (`/all-posts`)
- `Data & About` (`/about`)

#### 3. Populate Missing Chicago & Washington D.C. Stop Sequences
Complete the stop lists for Chicago CTA lines (`Blue-0-CHI.html`, `Red-0-CHI.html`, etc.) and WMATA lines (`BL-0-DC.html`, `SL-0-DC.html`, etc.). Including stop names directly builds keyword relevance for queries like *"Does the CTA Blue line stop at Logan Square?"* or *"DC Silver Line stops to Reston"*.

---

### Phase 5: Content Depth Expansion & Helpful Content Fortification (Weeks 4–6)

To defend against thin content penalties and capture long-tail search traffic, expand post templates to render helpful commuter guides below the live alerts:

```
┌────────────────────────────────────────────────────────┐
│ [Route Badge] Title: NYC Subway 1 Train Status         │
├────────────────────────────────────────────────────────┤
│ Live Service Alert Badge (Good Service / Delays)       │
│ Last Updated Timestamp                                 │
├────────────────────────────────────────────────────────┤
│ Planned Maintenance & Weekend Work Table               │
├────────────────────────────────────────────────────────┤
│ Route Map & Complete Directional Stop Lists            │
├────────────────────────────────────────────────────────┤
│ ─── NEW HIGH-VALUE COMMUTER CONTENT SECTIONS ───       │
│ 1. Operating Hours & Typical Headways                  │
│    (Rush hour 4-6 min, Middays 8 min, Late night 20min)│
│ 2. Key Transfer Points & Connecting Transit Lines      │
│ 3. Payment Methods (OMNY contactless / MetroCard)      │
│ 4. Commuter FAQs (Frequently Asked Questions Accordion)│
│    - Is the 1 train 24 hours?                          │
│    - What is the difference between 1 and 2/3 trains?  │
│    - Where does the 1 train start and end?             │
└────────────────────────────────────────────────────────┘
```

#### FAQ Schema (`FAQPage` JSON-LD)
Adding 3–4 structured FAQs per line unlocks Google's expanded SERP accordion snippets, boosting organic Click-Through Rates.

---

### Phase 6: Off-Site Authority, Digital PR & E-E-A-T Strategy (Months 2–3)

#### 1. Official Open Data Registries & Transit API Galleries
- **MTA Open Data Directory**: Submit Smooth Commutes to the MTA developer showcase.
- **City of Chicago Data Portal**: Register the application in the Chicago transit developer ecosystem.
- **WMATA Developer Portal**: Add Smooth Commutes to the WMATA 3rd-party application showcase.
- *Value*: High-authority `.gov` and transit agency inbound referral citations.

#### 2. Community & Civic Engagement
- **Reddit Transit Communities**:
  - Participate in weekly travel/commute threads in `r/nycrail`, `r/AskNYC`, `r/chicago`, and `r/washingtondc`.
  - Share outage analyses during major planned disruptions (e.g., weekend tunnel shutdowns or Marathon reroutings).
- **Transit Blog Outreach**:
  - Connect with transit blogs (e.g., *Second Ave Sagas*, *Streetsblog NYC*, *Streetsblog Chicago*, *Greater Greater Washington*) as a clean, fast, zero-tracker alternative to ad-bloated official apps.

#### 3. Author Entity Building (Reece Williams)
- Link the author profile to active GitHub transit repositories, professional LinkedIn, and transit discussions.
- Create an official `/editorial-policy` explaining how automated alert data is scraped, parsed, cached, and validated.

---

## 4. Key Performance Indicators (KPIs) & Monitoring Plan

Track the success of this SEO improvement plan through the following weekly and monthly metrics:

| Metric | Measurement Tool | Target (90 Days Post-Implementation) |
| :--- | :--- | :--- |
| **Total Indexed Pages** | Google Search Console (Page Indexing) | 100% of routes (37) + static pages (7) + city hubs cleanly indexed with zero errors. |
| **Rich Results Eligibility** | GSC Rich Results Status | Zero errors in Breadcrumb, Service, and FAQ schemas. |
| **Organic Search Clicks** | Google Search Console (Performance) | +45% increase across long-tail transit status keywords. |
| **Average Position for Core Terms** | GSC Queries Filter | Top 3 ranking for *"[Line] train status"*, *"[Line] delays today"*. |
| **Zero 404 Crawl Errors** | GSC Crawl Stats & Server Logs | 0 internal 404 errors across card thumbnails and author links. |
| **Core Web Vitals** | PageSpeed Insights / Chrome UX Report | LCP < 1.8s, CLS < 0.05, INP < 150ms on mobile devices. |

---

## 5. Summary Action Checklist

- [x] **Step 1**: Fix `_config.yml` (add `url: "https://www.smoothcommutes.com"`, clear `baseurl`, fix "Commutese" typo, shorten `title_suffix`).
- [x] **Step 2**: Rebuild `sitemap.xml` with homepage, static pages, and differentiated priorities.
- [x] **Step 3**: Add `Sitemap:` directive to `robots.txt`.
- [x] **Step 4**: Fix broken card image paths in `_includes/postbox.html` for Chicago and DC routes.
- [x] **Step 5**: Remove `rel="nofollow"` from footer internal links in `_layouts/default.html`.
- [x] **Step 6**: Invert post heading tags so `{{ page.title }}` is the single `<h1>`.
- [x] **Step 7**: Inject Open Graph, Twitter Cards, and JSON-LD schemas (`BreadcrumbList`, `WebSite`, `Service`).
- [x] **Step 8**: Unify the homepage (`index.html`) to expose Chicago and DC lines alongside NYC.
- [x] **Step 9**: Fix author link pointing to `/about-us` (update to `/about`) and fill in author credentials.
- [x] **Step 10**: Clean out legacy boilerplate files (`useful.html`, `recommended.html`, `newsletter.html`).
