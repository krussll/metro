---
layout: page
title: About Smooth Commutes
description: About Smooth Commutes, our transit research mission, data methodologies, editorial independence, and real-time coverage across NYC Subway, Chicago 'L', and Washington D.C. Metro.
---

Welcome to **Smooth Commutes**, an independent civic technology platform engineered to deliver rapid, zero-friction public transit alerts, planned service disruptions, and line status updates across three of the United States' busiest rail networks: **New York City (MTA)**, **Chicago (CTA)**, and **Washington D.C. (WMATA)**.

---

### Our Mission: Instant, Uncluttered Transit Telemetry

Public transit is the circulatory system of our metropolitan areas. Every weekday, millions of commuters rely on subways and rapid rail to reach work, schools, medical care, and airport terminals.

However, modern transit riders face a persistent challenge: official transit authority portals and commercial trip-planning apps are often encumbered with heavy JavaScript frameworks, bloated analytics trackers, video advertisements, and confusing navigation trees. When riders are standing on an underground platform with one bar of cellular reception, waiting 15 seconds for a 5-megabyte app to initialize simply to check why a train is halted is an unacceptable user experience.

**Smooth Commutes was created to solve this friction:**

1. **Sub-Second Static Delivery**: Every page on Smooth Commutes is pre-compiled into lightweight, static HTML backed by global CDN edge caching (Fastly / GitHub Pages). Pages load in milliseconds, even on spotty underground 3G/LTE connections.
2. **Zero Commercial Distortion**: We do not rewrite, conceal, or commercially sponsor service alerts. When track maintenance, signal failures, or emergency police actions cause disruptions, you see the exact dispatch advisory issued by agency controllers.
3. **Respect for Commuter Privacy**: Smooth Commutes requires no user registration, stores no location history, and runs zero invasive tracking scripts.

---

### About the Author & Developer

**Reece Williams** is a software developer, transit researcher, and dedicated daily commuter. Having navigated daily commutes across the NYC Subway, Chicago 'L', and Washington Metrorail, Reece experienced firsthand the frustration of ambiguous delay announcements and delayed mobile notifications.

Combining practical software engineering with deep empirical familiarity with urban transit layouts—from express bypass tracks in Manhattan to the multi-line CTA Loop elevated structure and Metrorail's intermodal airport extensions—Reece architected Smooth Commutes as a fast, reliable, and distraction-free reference for daily riders.

- **Role**: Founder, Lead Developer & Transit Data Researcher
- **Location & Coverage**: New York City, Chicago, Washington D.C.
- **Email**: [reece@reecewilliams.co](mailto:reece@reecewilliams.co)
- **GitHub**: [github.com/krussll](https://github.com/krussll)

---

### Autonomous Ingestion Engine & Open Data Pipeline

Smooth Commutes is powered by an autonomous, server-side data pipeline that operates around the clock:

```
┌─────────────────────────────────┐
│ Official Transit Agency APIs   │
│ • MTA GTFS-RT (NYC)             │
│ • CTA Customer Alerts (Chicago) │
│ • WMATA Incidents (DC)          │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│ Autonomous Synchronization Bot  │
│ Runs every 15 minutes:          │
│ • Pulls protocol buffer alerts  │
│ • Normalizes Markdown & Badges  │
│ • Updates live JSON timestamps  │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│ Production Static Edge Build    │
│ Sub-150ms TTFB worldwide        │
└─────────────────────────────────┘
```

- **Update Frequency**: Ingestion scripts query official GTFS-RT endpoints **every 15 minutes**, committing fresh service alert states directly to production.
- **Client-Side Verification**: On route pages, a lightweight client script queries `assets/updated.json` to inform riders of the exact local timestamp when data was last synchronized.
- **Open Data Sources**: For complete technical specifications of our agency API feeds, visit our dedicated [Data Sources & Transit Partners]({{ site.baseurl }}/data-partners) reference.

---

### Editorial Independence & Non-Affiliation

Smooth Commutes is an entirely independent digital publication and civic utility:

- **Non-Governmental Entity**: Smooth Commutes is not affiliated with, endorsed by, sponsored by, or operated by the Metropolitan Transportation Authority (MTA), Chicago Transit Authority (CTA), Washington Metropolitan Area Transit Authority (WMATA), or any other municipal or state body.
- **No Sponsored Alerts**: Service alerts, status badges, and planned disruption calendars are never sponsored or subject to commercial alteration.
- **Monetization Transparency**: Site operations and server hosting are sustained via standard display advertising. Any affiliate relationships are transparently disclosed under our [Affiliate Disclaimer]({{ site.baseurl }}/affiliate-disclaimer).

---

### Corrections & Accuracy Policy {#corrections}

We hold our data to the highest standards of reliability. While our automated pipeline mirrors official agency dispatch feeds, transit conditions can evolve rapidly between polling cycles:

1. **Reporting Errors**: If you encounter an out-of-date alert, a mislabeled transfer station, or an inaccurate stop listing, please contact us at [reece@reecewilliams.co](mailto:reece@reecewilliams.co) with the subject line `Data Correction`.
2. **Review Timeline**: Corrections are verified against primary agency API logs within 24 hours.
3. **Safety Notice**: In active platform emergencies, track evacuations, or severe weather shutdowns, **always follow the instructions of station conductors, platform staff, and transit police**.

Thank you for relying on **Smooth Commutes** as your daily transit companion.
