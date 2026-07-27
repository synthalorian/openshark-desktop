# OpenShark Desktop

> 🦈 *The harness, unchained from the terminal.*

Standalone GUI for [OpenShark](../openshark) — Svelte 5 + Tauri v2 + Rust, wrapped in a neon 1984 synthwave shell.

![Retro soul, modern precision](https://img.shields.io/badge/aesthetic-synthwave-neonpink)

## What It Does

Wraps the `openshark` CLI in a native desktop window:

- **Dashboard** — binary status, version, session stats
- **Chat** — streaming conversation with the shark
- **Agent** — autonomous task runner with live output
- **Memory** — search the persistent memory vault (semantic / recent / keyword)
- **Models** — browse configured providers
- **Tools** — the full arsenal
- **Doctor** — diagnostics with one-click fix
- **Config** — edit `config.toml` in place

## Requirements

- `openshark` binary on `PATH` (v1.1.0+)
- Node.js 18+
- Rust 1.91+ with Tauri v2 system deps (`webkit2gtk-4.1` on Linux)

## Themes

Twelve shark-grade themes, switchable from the 🎨 picker in the sidebar (persists across launches):

| Theme | Vibe |
|-------|------|
| 🦈 Neon Frenzy | Default — hot pink on abyssal black |
| 🌆 Synthwave '84 | The classic. Deep purple grid, electric violet sunset |
| 🤍 Great White | Clean light mode, apex predator in daylight |
| 💨 Mako | Chrome blue velocity |
| 🔨 Hammerhead | Industrial amber on gunmetal |
| 🐯 Tiger Shark | Orange stripes on black water |
| 🦷 Megalodon | Teal bioluminescence in crushing dark |
| 🩸 Blood in the Water | Crimson tide |
| 🪸 Reef | Tropical coral shallows |
| 📺 Shark Week | Documentary navy, red title card |
| 🏖️ Nurse Shark | Warm sand, zero urgency |
| 🎬 Amity '75 | JAWS-poster midnight |

## Typography

UI is set in **[3270](https://github.com/rbanffy/3270font)** — the IBM 3270 mainframe terminal face (bundled locally under `public/fonts/`, BSD-3-Clause, see `LICENSE-3270.txt`).

## Run

```bash
npm install
npm run tauri dev
```

## Build

```bash
npm run tauri build
```

---

*Fast. Precise. Hungry.* — This is the wave.
