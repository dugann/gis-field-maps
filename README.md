# Reference Implementation: Offline Map Rotator

**Status:** De-identified Reference Artifact
**Stack:** AngularJS 1.x, Leaflet.js
**Architecture:** Single-File Monolith (Data Injection Pattern)

## Project Overview

This repository demonstrates a solution for deploying a temporal GIS visualization tool to field devices in a zero-connectivity environment.

The project originated from a requirement to distribute 48-hour activity forecast models to field operations staff. The source material consisted of a compressed archive containing 48 discrete HTML artifacts, each representing a single hour of geospatial probability data.

## Problem Statement

The initial distribution strategy relied on physical printouts of these artifacts. This approach presented significant operational risks:

* **Data Latency:** Static media prevents real-time correlation between the current time and the active forecast model.
* **Usability:** Physical maps lack zoom capabilities required for street-level navigation.
* **Visual Noise:** The raw data contained low-probability vectors (inactivity indicators) that cluttered the display, obscuring actionable high-probability zones.

## System Constraints

The technical environment imposed strict non-functional requirements:

| Constraint | Implication |
| :--- | :--- |
| **Air-Gapped Operation** | Devices operate without reliable cellular/Wi-Fi. No external APIs or tile servers allowed. |
| **Zero Infrastructure** | No internal web hosting available. Application must execute from the local file system. |
| **Client-Side Security** | Browsers enforce strict CORS policies on `file://` protocol, blocking external data fetches. |

## Solution Architecture

The solution is a **Self-Contained Single-Page Application (SPA)** that utilizes a **Data Injection Pattern** to resolve the conflict between the offline requirement and browser security models.

### 1. Data Injection Strategy
To bypass Cross-Origin Resource Sharing (CORS) restrictions on local file access, the build pipeline parses the 48 source artifacts and injects the vectorized geospatial data directly into the application bundle as a JavaScript constant. This ensures immediate data availability without network requests.

### 2. Temporal Synchronization
The application implements an automated rotation engine that:
* Derives the current operational time (target timezone: Indianapolis).
* Performs O(1) lookups against the injected dataset to retrieve the active layer.
* Automatically updates the viewport when the forecast hour changes.

### 3. Client-Side Filtering
To improve data legibility, the rendering engine applies a configurable probability threshold. Vectors falling below this threshold are programmatically excluded from the DOM, reducing visual noise and improving rendering performance on mobile hardware.

## Execution

1.  Download `index.html`.
2.  Open in any modern web browser.
3.  The application initializes immediately using the embedded reference data.

*License: Proprietary / Reference Use Only*
