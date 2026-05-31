# AGENTS.md

This file provides guidance to Codex (Codex.ai/code) when working with code in this repository.

## Project Overview

Static educational website for interactive circuit/electronics simulation teaching. All content is written in Chinese. Hosted on GitHub Pages at `https://kclkvl026.github.io/`.

## Running Locally

No build step required. Serve the repo root with any static file server:
```
python -m http.server 8000
npx serve
```
Then open `http://localhost:8000/Circuit_Analysis/index.html` (or any other index.html).

## Architecture

**Single-file architecture**: Each simulation is a self-contained HTML file with inline `<script>` and `<style>` tags. There are no shared JS modules or component abstractions across pages.

**Four top-level directories:**
- `Circuit_Analysis/` — RLC circuits, Fourier series, frequency response, transient analysis, three-phase circuits
- `Analog_Electronics/` — semiconductor devices, amplifiers, op-amps, feedback, signal generation
- `Digital_Electronics/` — logic gates, Boolean algebra, Karnaugh maps, flip-flops, state machines, ADC/DAC
- `Circuit_Analysis_localcss/` — mirrors `Circuit_Analysis/` but loads CSS/JS locally (for offline use)

Each directory has an `index.html` serving as a card-based navigation hub. Subdirectories group related modules.

**Staging**: The `test/` directory is a staging copy of `Circuit_Analysis/` for manual testing — it is not an automated test suite.

## Tech Stack

- **HTML5 / CSS3 / JavaScript (ES6)** — no framework, no build tools
- **Tailwind CSS v3** — responsive UI (CDN in most pages; local copy in `Circuit_Analysis_localcss/css/`)
- **Chart.js v4.4.0** — waveform/chart plotting
- **KaTeX v0.16.9** — LaTeX math formula rendering
- Dependencies loaded from CDN (`cdn.tailwindcss.com`, `cdn.jsdelivr.net`, `cdnjs.cloudflare.com`)

## Conventions

- Each page uses `<input type="range">` sliders for real-time parameter adjustment, with JS event handlers that recalculate and redraw Chart.js plots dynamically.
- When adding new modules, follow the existing single-file pattern: self-contained HTML with inline JS, Tailwind for layout, Chart.js for plots, KaTeX for math.
- The `Circuit_Analysis_localcss/` variant must be kept in sync with `Circuit_Analysis/` — if you modify files in one, mirror changes to the other. The local variant loads from `css/lib/` instead of CDN.
- No linters or formatters are configured.
