# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Quick Start

**Running the game:**
- Open `index.html` directly in a browser (double-click), or
- Run a local server: `npx serve .` then visit `http://localhost:3000`

There is no build, lint, or test setup. Verify changes by running the game in a browser.

## Codebase Architecture

This is a single-file arcade game (423 lines). Key structure:

**Sections of `game.js`:**
1. **Input** — `keys` (held state) and `justPressed` (consumed edge-triggered); `pressed(code)` reads the latter
2. **Utils** — `wrap()` for toroidal world, `dist()`, `rand()`, `randInt()`
3. **Entity classes** — `Bullet`, `Asteroid`, `Ship`, `Particle`; all have `update(dt)`, `draw()`, and a `dead` flag
4. **Game state** — `ship`, `bullets`, `asteroids`, `particles` arrays; `score`, `lives`, `level`, `state` globals
5. **Update/draw functions** — collision, state transitions, rendering
6. **Main loop** — `requestAnimationFrame` with dt clamped to 0.05s

**Key game mechanics:**

- **Toroidal world**: Ship, bullets, and asteroids wrap at edges via `wrap()`; particles do not
- **Asteroid sizes**: 1–3, indexed in parallel arrays `RADII`, `SPEEDS`, `POINTS` (index 0 unused); size 3→2→1 when hit
- **State machine**: `'playing'` → `'dead'` (2s freeze, then `ship.reset()`) → `'gameover'` (Space restarts)
- **Input handling**: `pressed('Space')` for shooting and restart; arrow keys for rotation/thrust
- **Physics**: All motion is frame-independent (dt-based), except ship drag applies per frame (`DRAG = 0.987`)
- **Collision**: Bullet vs asteroid by distance; ship vs asteroid uses `a.radius * 0.82` (fudge for visual accuracy)
- **Level**: Clears when no asteroids remain; next level spawns `3 + level` asteroids

**Canvas**: Fixed 800×600 pixels (`W`, `H` constants).

## Language & Conventions

- Code comments and UI text are in **Spanish**
- Naming is straightforward: class names are PascalCase, functions are camelCase, game constants are UPPER_CASE
- Particles are temporary (no wrap) and cleaned up when `ttl <= 0`

## Notes

- Power-ups and the "shooting star" feature were removed (but README still mentions them—consider updating if relevant)
