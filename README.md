# Reference Implementation: Offline Map Rotator

**Status:** De-identified reference artifact  
**Stack:** AngularJS 1.x, Leaflet.js  
**Architecture:** Single-file offline web app (embedded data)

## Overview

This repository shows how to deliver a time-based GIS map viewer to field devices when there is no reliable internet access.

The work started with a requirement to distribute 48 hours of forecast maps to field staff. The input was a zip file containing 48 separate HTML files, one per hour, each containing geospatial probability data.

## Problem

The original approach was to print the maps, which created avoidable issues:

- **Stale information:** Paper cannot automatically align the current time to the correct hourly forecast.
- **Limited usability:** Printouts cannot zoom for street-level use.
- **Cluttered visuals:** Low-probability data made the maps noisy and harder to read.

## Constraints

| Constraint | What it means |
| --- | --- |
| **Offline operation** | No dependable cellular or Wi-Fi, and no external map or tile services. |
| **No hosting** | The tool must run directly from the local file system. |
| **Browser security limits** | Browsers restrict local file access, so the app cannot load data from other local files at runtime. |

## Solution

A self-contained, single-page map viewer that embeds all required data and updates automatically based on the current hour.

### Embedded data

A build step extracts the geospatial data from the 48 source files and packages it into the app as a JavaScript constant. This avoids runtime file loading and avoids network requests.

### Automatic hourly rotation

The app uses the Indianapolis time zone to select the active hour’s layer and refreshes the map automatically when the hour changes.

### Readability filtering

A configurable probability threshold removes low-value vectors before rendering. This reduces clutter and improves performance on mobile devices.

## How to run

1. Download `index.html`.
2. Open it in any modern browser.
3. The map loads immediately using the embedded reference data.

*License: Proprietary, reference use only*
