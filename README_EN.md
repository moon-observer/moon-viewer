# Moon Observer | Real Lunar Phases

<p align="right">
  <a href="./README.md">中文</a> · <strong>English</strong>
</p>

> Observe the real lunar phase, orientation, and related celestial events in your browser—from this moment and this place.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-24292f?logo=github)](https://moon-observer.github.io/moon-viewer/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Astronomy Engine](https://img.shields.io/badge/Astronomy%20Engine-2.1.19-596b7a)](https://github.com/cosinekitty/astronomy)

**Live site:** [https://moon-observer.github.io/moon-viewer/](https://moon-observer.github.io/moon-viewer/)

## Overview

Moon Observer is an interactive lunar visualization centered on the observer's location and time. It does not play a sequence of pre-rendered phase images. Instead, it calculates the geometry among the Sun, Moon, Earth, and ground-based observer in the browser, then continuously renders the corresponding lunar phase and orientation with WebGL.

The current V4 release includes:

- The real topocentric lunar phase, altitude, azimuth, distance, and apparent angular diameter;
- Lunar libration, position angle, earthshine, terminator, coordinate grid, and surface labels;
- A stable 4K base texture with 8K / 16K surface and elevation tiles loaded on demand;
- The next moonrise, next moonset, and the Moon's current direction;
- Locally visible lunar eclipses, lunar occultations of bright stars, and planetary occultations;
- Simple and Professional modes, with Chinese and English interfaces;
- Desktop and mobile browser interaction;
- Full-Moon and current-viewport PNG exports generated locally in the browser.

The project is intended for public science communication, lunar exploration, photography composition reference, and general observing plans.

## Features

### Real-Time Lunar Rendering

- Renders a spherical Moon in real time with WebGL instead of relying on a fixed sequence of phase images;
- Derives the illuminated side, dark side, and terminator from the current positions of the Sun, Moon, and observer;
- Presents the topocentric phase and surface parallax for the selected observing location;
- Displays geocentric optical libration, diurnal-parallax orientation changes, and lunar position angle;
- Uses lunar elevation data for surface normals, grazing-light shadows, and subtle limb relief;
- Supports earthshine intensity, surface-feature contrast, labels, coordinate grids, and terminator display;
- Keeps a 4K texture as a stable base layer, then loads 8K / 16K color and elevation tiles for the current view when more detail is needed;
- Loads high-resolution tiles into GPU texture atlases on demand, avoiding the cost of decoding a complete 16K image at once.

### Location and Time

- Gets the current coordinates through browser geolocation;
- Accepts WGS-84 latitude and longitude directly;
- Provides common cities for quick location changes;
- Supports point selection on AMap; locations in mainland China are converted from GCJ-02 to WGS-84 before astronomical calculations;
- Resolves an IANA time zone from coordinates and applies daylight-saving rules for the selected date;
- Allows automatic time-zone handling to be disabled in favor of a fixed UTC offset;
- Supports moving backward or forward by 5 minutes, 1 hour, or 1 day;
- Provides multiple forward simulation speeds and reverse playback.

### Observation Data

Professional mode provides the following real-time values:

- Illuminated fraction and phase name;
- Phase angle;
- Lunar altitude and azimuth;
- Earth–Moon distance;
- Longitudinal and latitudinal libration;
- Lunar position angle.

The map shows the Moon's current direction from the observer, together with the directions of the next moonrise and moonset. Azimuth starts at true north (`0°`) and increases clockwise.

### Lunar Eclipse Predictions

- Searches for lunar eclipses visible from the selected location;
- Distinguishes penumbral, partial, and total lunar eclipses;
- Displays P1, U1, U2, MAX, U3, U4, and P4 contact stages;
- Marks the intervals during which the Moon is above the local horizon;
- Supports jumping to the locally optimal moment, scrubbing the timeline, and playing the locally visible sequence;
- Includes the Danjon scale and optional HDR exposure and turquoise-band approximations.

Eclipse coloration is an adjustable visual approximation constrained by public scientific references. It is not an exact atmospheric color forecast for a particular eclipse.

### Lunar Occultations of Stars and Planets

- Searches a limited built-in target catalog for upcoming occultations of bright stars;
- Searches for future local lunar occultations of planets;
- Displays ingress, closest approach, and egress states on a timeline;
- Indicates whether the Moon is above the local horizon during the event;
- Supports jumping to an event and playing the surrounding sequence;
- Preserves the currently applicable occulted star or planet in exported images.

Occultation searches use lightweight in-browser calculations and a finite target catalog. They are suitable for interactive demonstrations and preliminary planning, but do not replace professional ephemerides, IOTA predictions, or dedicated occultation software.

### Image Export

- Simple mode provides two side-by-side actions: **Export full Moon** and **Export current view**;
- **Export full Moon:** always creates a fixed `4800 × 4800` PNG containing the complete lunar disc, regardless of the current zoom level;
- **Export current view:** preserves the current browser composition, including zoom, pan, and aspect ratio. The output keeps the visible viewport ratio with a `4800 px` long edge, even at `1×`;
- When the current view includes dark sky, the export retains a glow and star field consistent with the full-Moon export style;
- A centered footer labels the city, local time, UTC offset, and coordinates. Coordinates are rounded to two decimal places to reduce exposure of precise location data;
- Professional mode exports the current astronomical state and preserves applicable lunar-eclipse or occultation targets;
- If the Moon is below the local horizon, the page displays a temporary notice and does not generate a simulated human-eye observation;
- Image generation takes place entirely in the browser; exported images are never uploaded to a project server.

## Usage

### Simple Mode

1. Open the page to view the real Moon for the default location at the current time.
2. Select a city in the bottom dock to switch quickly among other locations where the Moon is currently visible.
3. Select **More** to use your current location or choose a point on the map.
4. On desktop, use the mouse wheel to zoom; double-click to switch between the default view and `2×`.
5. On mobile, pinch over the Moon to zoom and drag with one finger after zooming in.
6. Select **Export full Moon** for a fixed `4800 × 4800` complete-disc image, or **Export current view** to preserve the on-screen zoom, pan, and aspect ratio.

Simple mode follows real time and hides more advanced astronomical controls, making it suitable for everyday viewing and quick sharing.

### Professional Mode

1. Select **Professional** in the upper-right corner.
2. Under **Operating mode**, choose:
   - **Real astronomy mode:** the scene is driven entirely by location and time;
   - **Free demonstration mode:** inspect libration, rotation, terminator, and other variables independently.
3. Under **Observing location**, enter coordinates, use the current position, select a common city, or choose a point on the map.
4. Under **Observation time**, choose local civil time. With automatic time zones enabled, the app converts it to UTC according to the location's IANA time zone and selected date.
5. Use the time controls or **Advance time** to observe continuous changes in the lunar surface.
6. Review **Observation data**, or select an event under **Lunar eclipse predictions** or **Lunar occultation forecast**.
7. Adjust earthshine, contrast, and overlays under **Display and textures**.
8. Select **Export** to generate an image of the current astronomical state.

## Multi-Resolution Lunar Assets

The page uses two complementary sets of multi-resolution resources:

| Resource | Resolution | Tiles | Format | Purpose |
| --- | ---: | ---: | --- | --- |
| Base lunar texture | 4K | Single image | WebP | Stable first render and fallback while high-resolution tiles are unavailable |
| Lunar color texture | `8192 × 4096` | `256 px`, with edge extension | WebP | Surface texture at medium zoom |
| Lunar color texture | `16384 × 8192` | `256 px`, with edge extension | WebP | Surface texture at high zoom |
| Lunar elevation map | `8192 × 4096` | `512 px`, with edge extension | 16-bit data packed as PNG | Terrain lighting at medium zoom |
| Lunar elevation map | `16384 × 8192` | `512 px`, with edge extension | 16-bit data packed as PNG | More detailed terrain lighting at high zoom |

The application requests only the tiles needed for the current view. The base surface and elevation layers remain available during loading so an incomplete high-resolution layer does not leave visible holes.

## Data and Technology Sources

| Source | Use in This Project | License or Notes |
| --- | --- | --- |
| [Astronomy Engine 2.1.19](https://github.com/cosinekitty/astronomy) | Positions of the Sun, Moon, and planets; horizontal coordinates; phases, distances, libration, eclipses, and related astronomical calculations | MIT License |
| [NASA Scientific Visualization Studio — CGI Moon Kit](https://svs.gsfc.nasa.gov/4720) | Global LRO/LROC WAC lunar color texture and LRO/LOLA elevation data | Retain attribution to NASA SVS, LRO/LROC, and LOLA; scientific uses should refer to the original data products |
| [USGS / IAU Gazetteer of Planetary Nomenclature](https://planetarynames.wr.usgs.gov/) | Lunar feature names and coordinate references | Maintained by the USGS Astrogeology Science Center; names are approved by the IAU |
| [@photostructure/tz-lookup 11.5.0](https://github.com/photostructure/tz-lookup) | Approximate IANA time-zone lookup from coordinates | CC0-1.0; results may be approximate near time-zone boundaries |
| [AMap JavaScript API 2.0](https://lbs.amap.com/api/javascript-api-v2) | Map display, point selection, and location interaction | Users must comply with the AMap Open Platform terms and key-management requirements |
| [NASA Lunar Eclipses](https://science.nasa.gov/moon/eclipses/) / [Five Millennium Catalog of Lunar Eclipses](https://eclipse.gsfc.nasa.gov/LEcat5/appearance.html) | Reference for eclipse mechanics, contact stages, and visual design | Used as scientific and visual-modeling references |

## Accuracy and Intended Use

The upstream Astronomy Engine project describes its core astronomical position calculations as targeting approximately `±1 arcminute` agreement with authoritative models. This does not mean every output of this application has the same error range. Final results are also affected by:

- Accuracy of the input location, elevation, and device time;
- Atmospheric-refraction approximations, especially near the horizon;
- Simplified IANA time-zone boundaries and historical rules;
- Occultation target coordinates, catalog size, and in-browser search step size;
- Lunar textures, elevation sampling, shading, and visual enhancement;
- Browser, GPU, display scale, and available graphics memory.

This project is not intended for spacecraft navigation, measurement calibration, safety-critical decisions, or work requiring professional ephemeris accuracy. Verify authoritative ephemerides and original scientific data for formal observation, research, or engineering use.

## Privacy and Network Requests

The current version has no user accounts, project backend database, or self-hosted analytics service. Astronomical calculations, lunar rendering, and image export are performed primarily in the browser.

- The page, Astronomy Engine, time-zone library, base lunar texture, and tiles are served statically through this project's GitHub Pages site;
- The app connects to AMap when the map is used;
- **My location** calls the browser Geolocation API only after user authorization. The process remains subject to the privacy policies of the browser, operating system, and device location services;
- Exported images remain on the user's device and are never uploaded to the project server.

## Local Development

This is a static website and requires no build step. Because of high-resolution tiles, browser security policies, and location capabilities, preview it through a local HTTP server instead of opening `index.html` directly.

```bash
git clone https://github.com/moon-observer/moon-viewer.git
cd moon-viewer
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

A recent Safari, Chrome, Edge, or Firefox is recommended. The browser should support WebGL, Canvas, ES2020, and WebP. Geolocation usually requires HTTPS or the `localhost` secure context.

## Key Files

```text
moon-viewer/
├── index.html                    # Page, styles, interactions, and primary rendering logic
├── astronomy.browser.min.js      # Local browser build of Astronomy Engine
├── tz.js                         # Local IANA time-zone lookup data and logic
├── moon_base_4k.webp             # 4K base lunar texture
├── nasa_lola_height_4k.js        # 4K base LOLA elevation data
├── moon_tiles_v3_256/
│   ├── manifest.js               # Browser-side lunar tile manifest
│   ├── manifest.json             # Lunar tile metadata
│   ├── 8192/                     # 8K WebP lunar tiles
│   └── 16384/                    # 16K WebP lunar tiles
├── moon_height_tiles_v3/
│   ├── manifest.js               # Browser-side elevation tile manifest
│   ├── manifest.json             # Elevation tile metadata and encoding
│   ├── 8192/                     # 8K elevation PNG tiles
│   └── 16384/                    # 16K elevation PNG tiles
├── xhs_qr.jpeg                   # Xiaohongshu profile QR code
├── README.md                     # Chinese documentation (default)
├── README_EN.md                  # English documentation
└── LICENSE
```

## Technical Notes

- The project uses a static single-page structure, with the main interface and rendering logic in `index.html`;
- WebGL renders the lunar surface, illumination, earthshine, terrain, and eclipse shadow cone, while Canvas handles supporting labels;
- High-resolution color and elevation resources use equirectangular tiles selected through manifests;
- Texture atlases retain only tiles needed for the current view to control network, decoding, and GPU-memory costs;
- Astronomical calculations, event searches, and image composition all run in the browser;
- The repository can be hosted directly by any static file server.

## Project and Community

- [GitHub](https://github.com/moon-observer/moon-viewer)
- [Xiaohongshu](https://xhslink.cn/m/31HMrIiXd2u)

<p align="center">
  <a href="https://xhslink.cn/m/31HMrIiXd2u">
    <img src="./xhs_qr.jpeg" width="180" alt="Xiaohongshu profile QR code">
  </a>
</p>

## License

Original source code and documentation in this project are released under the [MIT License](LICENSE). You may use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, provided that the copyright and permission notices are retained in all copies or substantial portions of the software.

**The MIT License does not alter or override the licenses, terms, trademarks, or attribution requirements of third-party components, scientific data, map services, and media.** Follow the corresponding terms and source notices above when using, redistributing, or commercially publishing such content.

Copyright © 2026 moon-observer
