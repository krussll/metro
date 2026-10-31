---
layout: page
title: Data Sources & Transit Partners
description: Open transit data sources, GTFS-RT APIs, and developer feeds powering real-time subway and metro status on Smooth Commutes.
---

**Smooth Commutes** is committed to complete data provenance and algorithmic transparency. We ingest, process, and display real-time transit alerts, planned weekend service changes, and rail schedules published directly by official municipal transit agencies. 

Our architecture leverages open **General Transit Feed Specification Realtime (GTFS-RT)** feeds, RESTful developer APIs, and municipal open data portals. We do not modify, summarize, or commercially alter the warnings issued by transit dispatchers.

---

### 1. Metropolitan Transportation Authority (MTA) — New York City

The **[Metropolitan Transportation Authority (MTA)](https://new.mta.info)** operates the New York City Subway, Staten Island Railway, Long Island Rail Road, and Metro-North Railroad, serving over 3.5 million daily subway riders across Manhattan, Brooklyn, Queens, the Bronx, and Staten Island.

- **Official Transit Portal**: [new.mta.info](https://new.mta.info)
- **Live Interactive Subway Map**: [map.mta.info](https://map.mta.info)
- **Official Service Status**: [MTA Service Alerts](https://new.mta.info/alerts)
- **Developer Resources & APIs**: [MTA Developer Resources (api.mta.info)](https://api.mta.info)
- **Open Data Catalog**: [New York State Open Data Portal](https://data.ny.gov) (Open Data NY)
- **GTFS-RT Feed Endpoints**: Ingests NYCT Subway service alerts (`GTFS-RT Alerts`) and trip update feeds across numbered lines (1–7), lettered lines (A–Z), and the Staten Island Railway (SIR).
- **Accessibility & Elevator Status**: [MTA Elevator & Escalator Outages](https://new.mta.info/elevator-escalator-status)
- **Fare System**: [OMNY Contactless Fare Guide](https://omny.info)
- **Data Attribution & Terms**: Used in accordance with the [MTA Developer Terms of Use](https://new.mta.info/developers).

---

### 2. Chicago Transit Authority (CTA) — Chicago

The **[Chicago Transit Authority (CTA)](https://www.transitchicago.com)** is the nation's second-largest public transportation provider, operating the iconic Chicago 'L' rapid transit network comprising 8 rail lines and approximately 145 stations.

- **Official Transit Portal**: [transitchicago.com](https://www.transitchicago.com)
- **Live Train Tracking**: [CTA Train Tracker](https://www.transitchicago.com/traintracker/)
- **Customer Alerts & Planned Reroutes**: [CTA System Status & Alerts](https://www.transitchicago.com/alerts/)
- **Developer Center**: [CTA Developer Center](https://www.transitchicago.com/developers/)
- **API Documentation**: [CTA Customer Alerts API](https://www.transitchicago.com/developers/alerts/)
- **Municipal Data Portal**: [City of Chicago Data Portal (data.cityofchicago.org)](https://data.cityofchicago.org)
- **Accessibility Feeds**: [CTA Elevator Alerts & Station Accessibility](https://www.transitchicago.com/elevators/)
- **Fare Technology**: [Ventra Chicago](https://www.ventrachicago.com)
- **Data Attribution**: CTA transit alerts and route schedules are provided courtesy of the Chicago Transit Authority under the [CTA Developer License Agreement](https://www.transitchicago.com/developers/terms/).

---

### 3. Washington Metropolitan Area Transit Authority (WMATA) — Washington D.C.

The **[Washington Metropolitan Area Transit Authority (WMATA)](https://www.wmata.com)** operates the Metrorail system serving the District of Columbia, suburban Maryland, and Northern Virginia, including direct rail service to Ronald Reagan Washington National Airport (DCA) and Washington Dulles International Airport (IAD).

- **Official Transit Portal**: [wmata.com](https://www.wmata.com)
- **Live Status & MetroPulse**: [WMATA Live Service Status](https://www.wmata.com/service/status/)
- **Trip Planning**: [WMATA Trip Planner](https://www.wmata.com/rider-guide/trip-planner/)
- **Developer API Portal**: [WMATA Developer Portal (developer.wmata.com)](https://developer.wmata.com)
- **Alert & Incident Endpoints**: `Rail.svc/Incidents` and GTFS-RT TripUpdate protocols.
- **Accessibility Feeds**: [WMATA Metrorail Elevator & Escalator Status](https://www.wmata.com/service/elevators-escalators/)
- **Fare Technology**: [WMATA SmarTrip Card](https://www.wmata.com/fares/smartrip/)
- **Data Attribution**: Metrorail alerts and schedule feeds are supplied by WMATA under the [WMATA Developer License](https://developer.wmata.com/terms).

---

### 4. Technical Standards & Ingestion Architecture

Smooth Commutes is engineered as a zero-database, high-speed static web application to ensure that commuters underground or on congested cellular connections receive transit updates with sub-second page loads.

```
┌─────────────────────────────────┐
│ Official Transit Agency APIs   │
│ (MTA, CTA, WMATA GTFS-RT Feeds) │
└────────────────┬────────────────┘
                 │
                 ▼ (Polled every 15 minutes)
┌─────────────────────────────────┐
│ Autonomous Ingestion Engine     │
│ • Validates protocol buffers    │
│ • Filters route-specific alerts │
│ • Normalizes Markdown & HTML    │
└────────────────┬────────────────┘
                 │
                 ▼ (Static Build)
┌─────────────────────────────────┐
│ GitHub Pages / Fastly CDN Edge  │
│ Sub-150ms TTFB, zero tracking   │
└─────────────────────────────────┘
```

1. **GTFS-RT Standards Compliance**: Our background ingestion pipeline parses standard protocol buffer messages following specifications maintained by [MobilityData](https://mobilitydata.org) and [GTFS.org](https://gtfs.org), specifically consuming `Alert` entities with cause, effect, and active period timestamps.
2. **Automated 15-Minute Synchronization**: An automated background pipeline queries agency alert endpoints every 15 minutes, re-generating pre-rendered HTML snippets directly into our static repository branch.
3. **Zero Editorial Alteration**: All dispatch advisories, track work notices, and reroute instructions are presented verbatim without subjective alteration or commercial filtering.
4. **Bandwidth Optimization**: Unlike native transit apps that download heavy maps and tracking libraries on every launch, Smooth Commutes delivers pre-compiled HTML and minimal CSS, ensuring instant access even when cellular reception is weak on underground platforms.

---

### 5. Latency Limitations & Safety Disclaimer

> [!IMPORTANT]
> **Real-Time Latency Notice**:
> Smooth Commutes synchronizes with transit agency feeds on a 15-minute polling interval. While this provides rapid notification of general delays, signal malfunctions, and planned weekend track work, it does not substitute for instant on-platform announcements during sudden emergencies.
>
> In the event of active track fires, police activity, or emergency evacuations, **always follow the direct instructions of train conductors, station agents, and transit police**.

---

### 6. Reporting Data Discrepancies & Feedback

We welcome reports from daily riders if you observe an alert mismatch between Smooth Commutes and live station conditions:

- **Corrections Email**: [reece@reecewilliams.co](mailto:reece@reecewilliams.co)
- **Response Turnaround**: Discrepancy reports are investigated within 24 hours.
- **Contact Hub**: For agency emergency numbers and detailed inquiries, visit our [Contact & Commuter Feedback Hub]({{ site.baseurl }}/contact).