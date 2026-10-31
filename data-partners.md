---
layout: page
title: Data Sources & Transit Partners
description: Open transit data sources, GTFS-RT feeds, and APIs powering real-time subway and metro status on Smooth Commutes.
---

Smooth Commutes ingests, processes, and displays real-time transit alerts, planned service changes, and rail schedules published directly by official transit agencies. We utilize open GTFS-Realtime (GTFS-RT) feeds and developer APIs to ensure riders receive accurate, vendor-neutral service alerts.

---

### Metropolitan Transportation Authority (MTA) — New York City

The **Metropolitan Transportation Authority (MTA)** oversees the New York City Subway, Staten Island Railway, and Long Island Rail Road / Metro-North commuter networks. 

- **Data Sources**: MTA Open Data Portal & GTFS-Realtime Service Status Feeds.
- **Coverage**: All 23 NYC subway routes (1, 2, 3, 4, 5, 6, 7, A, B, C, D, E, F, G, J, L, M, N, Q, R, SIR, W, Z).
- **Update Frequency**: Live alert feeds polled every 15 minutes to reflect emergency delays, signal malfunctions, reroutes, and weekend maintenance.

---

### Chicago Transit Authority (CTA) — Chicago

The **Chicago Transit Authority (CTA)** is the nation's second-largest public transportation provider, operating the iconic Chicago 'L' rapid transit train system.

- **Data Sources**: CTA Customer Alerts API & Chicago Open Data Portal.
- **Coverage**: All 8 rail lines (Blue, Brown, Green, Orange, Pink, Purple, Red, and Yellow Lines).
- **Update Frequency**: Ingests real-time planned disruptions, track maintenance, platform closures, and station event notices.

---

### Washington Metropolitan Area Transit Authority (WMATA) — Washington D.C.

The **Washington Metropolitan Area Transit Authority (WMATA)** operates the Metrorail system serving the District of Columbia, Northern Virginia, and suburban Maryland.

- **Data Sources**: WMATA Developer Portal (Rail Incidents & Delays API).
- **Coverage**: All 6 Metrorail lines (Red, Blue, Orange, Silver, Green, and Yellow Lines).
- **Update Frequency**: Regular synchronization of track work advisories, headway changes, elevator outages, and emergency service suspensions.

---

### Our Data Ingestion & Transparency Methodology

1. **Automated Freshness**: Alert feeds are automatically pulled and parsed every 15 minutes by a dedicated background pipeline.
2. **Zero Editorial Alteration**: Official transit warnings and station instructions are presented verbatim without editorial tampering, ensuring commuters receive the exact guidance issued by agency dispatchers.
3. **Open Access**: Smooth Commutes is free, requires no user registration, and operates without tracking scripts, providing fast, low-bandwidth access to essential travel information.
4. **Questions or Discrepancies**: If you observe an alert discrepancy between Smooth Commutes and on-platform announcements, please contact us at [reece@reecewilliams.co](mailto:reece@reecewilliams.co).