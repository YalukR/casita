# CASITA

*[Leer esto en español](README.es.md)*

> Name subject to change.

A lightweight, open-source viewer for astronomical FITS files (images and cubes),
built to run well on modest hardware — student laptops, not research workstations.
A light alternative to CASA Viewer / CARTA / DS9, with two front ends sharing a
single core: terminal (TUI) and desktop (GUI).

**Guiding principle:** the user's hardware should never be the reason the program
doesn't work. If a file is too large for available RAM, the program keeps working
(slower, via streaming), instead of crashing or freezing the machine.

## Current status

**Just getting started.** This repository has no code yet. Phase 1 (see Roadmap
below) is under construction: a TUI viewer for 2D FITS files on Linux.

There are no binaries or install instructions yet — those will be added here once a
working MVP exists.

## Who is this for?

University students (not necessarily astronomy majors) who need to open and inspect
FITS files for a homework assignment, lab, or project, on hardware that isn't
research-grade: 4-8 GB of RAM, integrated GPU, an older CPU, sometimes working over
SSH into a university server that's just as constrained, or with an intermittent
connection.

## Roadmap

The full scope was split into phases by weighing the "ideal scope" against the
actual time available. The rule for deciding what belongs in the MVP: *would a
student who just wants to view a 2D image notice this is missing on day one?* If
not, it gets postponed.

### Phase 1 - real MVP (in progress)
- Reading 2D FITS files (no cubes yet) and header parsing
- Intensity normalization: linear and zscale
- 1-2 colormaps (grayscale + viridis)
- Zoom / pan
- TUI with ANSI truecolor rendering, keyboard navigation, works over SSH
- Streaming / memory-mapped reads - never loading the full file into RAM
- Opening a typical file in under 2 seconds
- Linux only

### Phase 2 - first expansion
- GUI with Tauri (mouse, scroll-to-zoom, export to PNG/JPEG)
- 3D cubes: slice navigation + spectrum extraction
- Additional log/sqrt normalization
- Windows and macOS
- Terminal detection and graceful degradation (Kitty/Sixel) in the TUI

### Phase 3 - later improvements (no fixed date)
- More colormaps
- Robust handling of non-standard headers
- More polished packaging (native installers, signing, etc.)

### Out of scope indefinitely
Real celestial coordinates (WCS), CARTA's proprietary HDF5 format, calibration or
data reduction, hand-drawn regions/photometric measurements, real-time
collaboration, integration with remote science archives, support for rotated cubes.

## Architecture (planned)

The core (FITS parser, normalization, colormaps, streaming reader) lives separate
from the rendering layer from day one, so Phase 2 (Tauri GUI) can reuse it without a
rewrite:

```
casita/
├── core/         # FITS parser, normalization, mmap reader - no UI
├── tui/          # Phase 1: terminal interface (ratatui/crossterm)
└── gui-tauri/    # Phase 2: desktop interface (not started yet)
```

## Non-functional requirements

| Category | Target |
|---|---|
| Installer size | GUI <50 MB - TUI (single binary) <20 MB |
| Idle RAM usage | <150 MB with a medium-sized file open |
| External dependencies | None at runtime beyond the system WebView (GUI) |
| Connectivity | 100% offline |
| Privacy | No telemetry, no accounts, no unsolicited network connections |
| License | GNU AGPLv3 |

## Installation

_Coming soon - once a Phase 1 binary exists._

## License

[GNU AGPLv3](https://www.gnu.org/licenses/agpl-3.0.html) - strong copyleft,
including the network-use clause.

## Contributing

This project is under active construction, and the process itself is part of the
goal (a portfolio piece with clean, documented code). Issues and PRs are welcome,
especially around the open questions from the requirements document (real file
sizes students actually use, most common terminals, whether cubes are a frequent
case for non-astronomer users).
