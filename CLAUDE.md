# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a collection of self-contained browser games, each implemented as a single HTML file with no build tools, no dependencies, and no external assets. Everything — HTML structure, CSS, and JavaScript — lives in one file per game.

## Running Games

Open any `.html` file directly in a browser:
```
open "shooter.html"
open "tictactoe.html"
```

No server, no build step, no package manager.

## Architecture Conventions

### Single-file structure
Each game file is organized into clearly separated comment-block sections in this order:
1. `<style>` — dark theme CSS (body background `#0d0d0d` or `#1a1a2e`, canvas/grid centered via flexbox)
2. Constants & Config
3. Game State Variables
4. Input Handling
5. Entity Classes
6. Game Logic (collision, spawning, etc.)
7. Render Functions
8. Game Loop / Init

### Visual style
- Dark backgrounds: `#0d0d0d` (body) / `#111` (canvas fill)
- Accent colors: `#00e5ff` (cyan), `#e94560` (red/danger), `#ffd700` (gold)
- All visuals drawn with Canvas 2D API using shapes only — no image assets

### Game loop pattern (`shooter.html`)
Fixed-timestep accumulator at 60fps using `requestAnimationFrame`. `update(dt)` receives `dt` in seconds; `render()` is called once per frame. Clamp delta to 100ms to handle tab-switch spikes.

### Input pattern
A plain `keys` object tracks live key state via `keydown`/`keyup` listeners. Read `keys['ArrowLeft']` etc. inside `update()` rather than triggering actions directly in the event handler. Canvas `mousedown` with `getBoundingClientRect()` offset handles click regions.

### State machine
Games use a string `gameState` variable (`'START'`, `'PLAYING'`, `'GAMEOVER'`). All rendering and update logic branches on this value; transitions happen in event handlers and inside `update()`.