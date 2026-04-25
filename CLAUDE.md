# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**OK FERNAND** — a pixel-art top-down western gunslinger game. Single-file HTML5 Canvas, no build tools, no dependencies.

- GitHub: https://github.com/fernandez-studio/RetroGame (user: `fernandez-studio`)
- Open any `.html` file directly in a browser to run it.

## Architecture

Each version is a self-contained HTML file (`gunslinger.html`, `gunslinger_02.html`, etc.). All game code lives in one `<script>` block per file.

**Rendering pipeline:**
- `S` — scale factor (game pixels → screen pixels). v01: S=4 (960×640), v02: S=6 (1440×960). Arena is always 240×160 game pixels.
- `dot(gx, gy, color)` — draw one game pixel
- `grect(gx, gy, gw, gh, color)` — draw a filled rectangle in game coordinates
- `drawSprite(sprite, palette, gx, gy, flipX)` — render a 2D array sprite using a palette object; `null` cells are transparent
- Sprites are 2D arrays of palette keys (strings). Palettes are `{KEY: '#RRGGBB'}` objects. Palette-swapping gives each player a different color scheme from the same sprite data.

**Current state (gunslinger_02.html):**
- Static render only — `draw()` called once, no game loop
- Scene: arena floor + fence boundary, cactus obstacle, 2 cowboys (P1 red left, P2 blue right mirrored), dynamite mines, tommy gun power-ups, wide-shot power-ups, healing potions, teleport circles in corners

**Next planned milestone (gunslinger_03):**
- `requestAnimationFrame` game loop with `update()` / `render()` split
- Player 1 movement via WASD/arrows, clamped to arena bounds
- Shooting mechanic (spacebar)

## Memory

Memory lives in `.claude/memory/` inside this project folder — it is committed to git and backed up with the repo. The index is `.claude/memory/MEMORY.md`. Always read and write memory files there, not in `~/.claude/projects/`.

## Conventions

- New versions get a new numbered file (`gunslinger_03.html`, etc.) — don't overwrite previous versions.
- Commit and push to `main` after each meaningful version.
- Keep everything in one HTML file per version — no external scripts or stylesheets.
