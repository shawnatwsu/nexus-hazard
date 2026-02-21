# 🌍 NEXUS — Global Hazard Monitor

> **A free, real-time 3D Earth visualization tracking natural disasters as they happen — built for education, transparency, and public awareness.**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-00e5ff?style=for-the-badge&logo=github)](https://YOUR-USERNAME.github.io/nexus-hazard/)
[![Data: USGS](https://img.shields.io/badge/Data-USGS%20Earthquakes-green?style=for-the-badge)](https://earthquake.usgs.gov)
[![Data: NASA EONET](https://img.shields.io/badge/Data-NASA%20EONET-blue?style=for-the-badge)](https://eonet.gsfc.nasa.gov)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

---

![NEXUS Globe Screenshot](screenshot.png)

---

## 📖 What Is NEXUS?

NEXUS is a single-file web application that renders a real-time interactive 3D globe displaying active natural hazard events worldwide. It pulls live data from NASA and the USGS every 5 minutes and plots earthquakes, wildfires, volcanoes, severe storms, floods, landslides, dust events, and sea ice anomalies directly onto a rotating Earth.

It was built as a **free public educational tool** — no login, no paywall, no tracking. Anyone with a browser can open it and immediately understand what is happening on our planet right now.

**Why does this exist?** Most real-time disaster data is locked inside hard-to-read spreadsheets, government portals, or expensive subscription platforms. NEXUS makes that same data immediately accessible and understandable to a 10-year-old and a geology PhD alike.

---

## ✨ Features

### 🌐 Real-Time Globe
- Interactive 3D orthographic globe rendered with **D3.js** and the Canvas API
- Smooth drag rotation, mouse wheel + pinch-to-zoom
- Auto-orbit mode with momentum physics
- Star field background, atmospheric glow, ocean gradient
- Country and continent labels that appear at the right zoom level

### 🚨 Live Hazard Data
| Hazard Type | Source | Refresh |
|---|---|---|
| Earthquakes (M2.5+) | USGS Earthquake Hazards Program | Every 5 min |
| Wildfires | NASA EONET (≤21 days old) | Every 5 min |
| Volcanoes | NASA EONET | Every 5 min |
| Severe Storms & Cyclones | NASA EONET | Every 5 min |
| Floods | NASA EONET | Every 5 min |
| Landslides | NASA EONET | Every 5 min |
| Dust & Haze Events | NASA EONET | Every 5 min |
| Sea & Lake Ice Anomalies | NASA EONET | Every 5 min |

- Events are **deduplicated** to avoid double-plotting the same event from multiple sources
- Wildfire events are **age-filtered** — events older than 21 days are dropped since NASA EONET can be slow to close resolved fire records
- Earthquake dot **size and colour scale with magnitude** (M2.5 = tiny green dot → M8.0+ = large red dot)
- Earthquake dot **depth** is shown as a line from surface to hypocenter

### 🚨 Shake Alert — Breaking News
- Automatically detects M6.5+ earthquakes in fresh data
- Animated red banner slides in from the top with event name, magnitude, depth, and age
- Globe rim flashes red and flies to the event location
- Three-tone audio alert plays (Web Audio API — no files required)
- Auto-dismisses after 18 seconds

### 🎓 Hazard Education Center
Eight fully written educational modules — one for every hazard type — covering:
- What the hazard is and how it forms
- Why it happens (geology, meteorology, climate)
- Measurement scales (Richter, VEI, Saffir-Simpson, AQI, etc.)
- Safety and preparedness advice
- Global impact statistics

**Age-level reading toggle:** Cycle between KIDS / TEEN / ADULT mode — the same content is rewritten in age-appropriate language for each level. Great for classroom use.

### 📋 Daily Briefing
One-click summary of the current global situation — headline event, active counts by type, and clickable cards that fly the globe to each event.

### 📤 Share a Disaster
Click any event → hit SHARE → generates a branded card with event stats and a random earth science fact. Share via:
- Copy text to clipboard (Twitter/SMS ready)
- Post directly to X (Twitter) with pre-filled text
- Save as PNG image

### 🏔 Tectonic Plate Overlay
Toggle real plate boundary lines drawn over the globe (Peter Bird's PB2002 dataset). Instantly shows the visual relationship between plate boundaries and earthquake/volcano clusters — one of the most powerful teaching moments in earth science.

### ◉ Heatmap Mode
Toggle from individual event dots to a density heat cloud. Each hazard type bleeds its signature color across the globe — shows the **geography of risk** at a glance rather than individual events.

### 📜 Historical Events Timeline
A slider spanning 1906–2024 with 26 major disasters. Click any card to fly the globe to the location. Each event includes death toll, description, and a Wikipedia link. Events include:
- 1906 San Francisco Earthquake (M7.9)
- 1960 Valdivia Earthquake — largest ever recorded (M9.5)
- 2004 Indian Ocean Tsunami (M9.1, 228,000 deaths)
- 2011 Tōhoku Earthquake & Tsunami (M9.0)
- 2023 Turkey-Syria Earthquake sequence

### 🎵 Procedural Ambient Music
Atmospheric soundtrack generated entirely via the Web Audio API — no audio files required. Bass drones, chord pads, shimmer harmonics, reverb, and random chimes. Toggle with the ♪ MUSIC button.

### 💡 Did You Know Ticker
18 rotating earth science facts scroll across the bottom alert bar — auto-rotates every 18 seconds. Designed to teach passively while the globe spins.

---

## 🛠 Technology Stack

NEXUS is intentionally **zero-dependency** beyond two small CDN libraries. Everything else is vanilla JavaScript.

| Technology | Purpose |
|---|---|
| [D3.js v7](https://d3js.org) | Orthographic globe projection, GeoJSON rendering, graticule lines |
| [TopoJSON v3](https://github.com/topojson/topojson) | Compressed world map geometry |
| [Natural Earth / world-atlas](https://github.com/topojson/world-atlas) | Country and land boundary data |
| [Peter Bird PB2002](https://github.com/fraxen/tectonicplates) | Tectonic plate boundary GeoJSON |
| Canvas 2D API | All globe rendering (no WebGL required) |
| Web Audio API | Procedural ambient music and alert beeps |
| Vanilla JS / HTML5 | Everything else — no frameworks, no bundlers |

**Total file size: ~1 HTML file.** No build step. No Node.js. No npm. Open the file and it works.

---

## 🌐 Data Sources & Transparency

NEXUS is committed to full data transparency. Every event dot on the globe traces back to a public government or space agency data source.

### USGS Earthquake Hazards Program
- **Endpoint:** `https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_month.geojson`
- **Coverage:** All earthquakes M2.5 and above globally, past 30 days
- **Latency:** Updated every 5 minutes
- **License:** Public domain (U.S. government data)
- **Limitation:** Does not cover events below M2.5

### NASA EONET (Earth Observatory Natural Event Tracker)
- **Endpoint:** `https://eonet.gsfc.nasa.gov/api/v3/events?status=open&limit=500`
- **Coverage:** Wildfires, volcanoes, severe storms, floods, landslides, dust/haze, sea ice
- **Latency:** Updated irregularly by NASA analysts — not truly real-time for all types
- **License:** Public domain (NASA open data)
- **Known limitation:** Wildfire events may remain "open" in the system for weeks after the fire is contained. NEXUS applies a 21-day age filter to mitigate this.

### What NEXUS Does Not Have Access To
- Fatality or injury counts in real time (these come from official government reports, not sensor data)
- Events below M2.5 for earthquakes
- Flash floods or tornadoes with less than a few hours of warning
- Private commercial satellite data

When the app is running in a sandboxed environment (like an embedded iframe) or network requests are blocked, NEXUS falls back to a representative dataset of ~130 historical events spanning all hazard types, clearly labelled as demo data.

---

## 🚀 Getting Started

### Option 1 — Just Open It
Download `index.html` and open it in any modern browser. That's it. No installation, no server required.

```bash
# If you have Python installed you can also serve it locally:
python3 -m http.server 8000
# Then open http://localhost:8000
```

### Option 2 — GitHub Pages (Recommended for sharing)
See the full step-by-step deployment guide below.

### Browser Compatibility
| Browser | Support |
|---|---|
| Chrome / Edge | ✅ Full support |
| Firefox | ✅ Full support |
| Safari | ✅ Full support |
| Mobile Chrome / Safari | ✅ Touch + pinch zoom supported |

**Requires:** A browser released after 2018. No plugins or extensions needed.

---

## 📦 Deployment — GitHub Pages (Free)

1. **Create a GitHub account** at [github.com](https://github.com) if you don't have one
2. **Create a new public repository** — name it `nexus-hazard` (or anything you like)
3. **Upload `index.html`** via the "Add file → Upload files" button
4. Go to **Settings → Pages**
5. Under "Branch" select **main** and folder **/ (root)** → click Save
6. Your site will be live at `https://YOUR-USERNAME.github.io/nexus-hazard/` within ~60 seconds

To update the site, simply upload a new `index.html` to replace the existing one.

### Custom Domain (Optional — ~$12/year)
If you want a clean URL like `nexus-hazard.earth`:
1. Buy a domain from [Porkbun](https://porkbun.com) or [Namecheap](https://namecheap.com)
2. Add it in **Settings → Pages → Custom domain**
3. Point your domain's DNS to GitHub's servers (GitHub provides the exact records to add)

---

## 🗺 Roadmap

Features planned or under consideration:

- [ ] **Quiz Mode** — 5-question daily earth science quiz tied to current events on the globe
- [ ] **"Could It Happen Here?"** — click any country to see its top 3 hazard risks and historical worst event
- [ ] **NASA FIRMS Integration** — truly live fire hotspot data (requires a free API key from [firms.modaps.eosdis.nasa.gov](https://firms.modaps.eosdis.nasa.gov/api/map_key/))
- [ ] **Classroom Mode** — simplified UI for use in school presentations, teacher-controlled hazard filter
- [ ] **Event comparison tool** — side-by-side two historical disasters with statistics
- [ ] **Offline mode** — cached data for use without internet
- [ ] **Accessibility improvements** — screen reader support, keyboard navigation
- [ ] **Multiple language support** — Spanish, French, Arabic, Mandarin

---

## 🤝 Contributing

Contributions, bug reports, and feature suggestions are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/quiz-mode`
3. Make your changes to `index.html`
4. Open a pull request with a clear description of what changed and why

Since this is a single-file application, all changes happen in `index.html`. Keep it that way — the zero-dependency, single-file design is intentional and important for accessibility (teachers can download and use it offline, students in low-bandwidth areas can share it via USB).

### Reporting Data Issues
If you notice an event that seems wrong, stale, or mislocated — that's almost certainly an upstream data issue with USGS or NASA EONET. Please [open an issue](../../issues) and include the event name, type, and what you think is wrong. We can investigate whether a filter or correction is appropriate.

---

## 📚 Educational Use

NEXUS is explicitly designed for classroom and self-directed learning. Teachers and educators are encouraged to:

- Use it freely in presentations (fullscreen works great on a projector)
- Share the URL with students as a homework resource
- Use the HISTORY slider for lessons on famous disasters
- Use the LEARN panel as a structured introduction to each hazard type
- Use the AGE MODE toggle to match reading level to your class

If you use NEXUS in a classroom and have feedback on how to make it more useful for teaching, please open an issue or reach out — improving the educational experience is the top priority.

---

## 📰 Press & Attribution

If you write about or reference NEXUS, please credit:

> **NEXUS Global Hazard Monitor** — Data sourced from the U.S. Geological Survey (USGS) and NASA Earth Observatory Natural Event Tracker (EONET). Tectonic plate data from Peter Bird (2003), *An updated digital model of plate boundaries*, Geochemistry Geophysics Geosystems.

---

## ⚖️ License

MIT License — free to use, modify, and distribute for any purpose including commercial use. See [LICENSE](LICENSE) for full text.

Data from USGS and NASA is in the public domain (U.S. government works). Tectonic plate boundary data (PB2002) is used under academic open-data terms.

---

## 🙏 Acknowledgements

- **[USGS Earthquake Hazards Program](https://earthquake.usgs.gov)** — for the most comprehensive public earthquake feed on Earth
- **[NASA EONET](https://eonet.gsfc.nasa.gov)** — for aggregating natural event data from dozens of satellite and ground sources
- **[Mike Bostock & D3.js](https://d3js.org)** — for the geographic projection library that makes the globe possible
- **[Peter Bird](http://peterbird.name/publications/2003_PB2002/2003_PB2002.htm)** — for the PB2002 tectonic plate boundary dataset used in science education worldwide
- **[Natural Earth](https://www.naturalearthdata.com)** — for the public domain world map data
- Every scientist, engineer, and analyst at USGS and NASA whose work feeds into these public data streams

---

*Built with 🌊 for the people who want to understand the planet they live on.*
