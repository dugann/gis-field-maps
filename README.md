# Reference Implementation: Offline Map Rotator

**Role:** Developer
**Stack:** AngularJS (v1.x), Leaflet.js, Single-File Distribution

## The Scenario

I was handed a zip file containing 48 separate HTML files. Each file represented a different hour of the day, displaying statistical **color-coded polyline overlays** on a city map.

The client intended to print these 48 files as physical packets for field staff.

## The Problem

A paper-based workflow fails in the field for three reasons:

1. **Usability:** You can't zoom on paper to see street-level details.

2. **Compliance:** Relying on agents to manually check their watch and flip to the correct page in a 48-page packet guarantees errors. Agents often end up using outdated maps.

3. **Noise:** The source maps were cluttered with low-probability blue lines (inactivity), making it hard to spot the actual high-activity targets.

## The Constraints

We needed a digital tool, but the operating environment effectively ruled out standard web apps.

| **Constraint** | **The Issue** | **The Consequence** |
| :--- | :--- | :--- |
| **Zero Connectivity** | Company-issued phones often have no cellular or Wi-Fi signal. | We cannot use cloud APIs, map tile servers, or authentication services. |
| **No Hosting** | We had no IT infrastructure to host an internal site. | The tool must run directly from the phone's local file system. |
| **Browser Security** | Browsers block local files (`file://`) from fetching data. | We cannot use `fetch()` or `XMLHttpRequest` to load external JSON files without a server. |

## The Solution

I built a **Self-Contained Single-Page Application (SPA)** that bypasses the need for a server by bundling the data directly into the code.

### 1. Data Injection (Solving CORS)

Since I couldn't fetch the 48 data files dynamically without a server, I created a build script that parses the original HTML files and injects their coordinate data directly into a JavaScript variable. This makes the data instantly available in memory.

### 2. Automated Map Rotation

Instead of building a complex dashboard, I built a simple rotator.

* **Timezone Sync:** The app calculates the current time in the target city (Indianapolis).

* **Auto-Rotate:** It automatically loads the correct map layer for the current hour.

* **Countdown:** A visual timer shows agents exactly when the map will rotate next.

### 3. Noise Filtering

Since I controlled the rendering, I added a filter to hide the "low probability" blue lines. This declutters the map so agents only see the high-priority zones.

## How to Run This Demo

This repo contains the engine with sanitized mock data.

1. Download `index.html`.

2. Open it in any browser (Chrome, Edge, Firefox).

3. The app runs immediately without a local server.

> **Note:** This reference implementation links to CDN versions of Angular and Leaflet for demonstration purposes. The production version utilized locally embedded libraries to satisfy the strict offline requirement.
