# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

"The Zoomies" — a cozy endless runner game starring two cats (Hank and Bailey). The entire game lives in a single `index.html` file deployed via GitHub Pages. No build step, no dependencies, no external assets beyond Google Fonts.

## Tech Stack

- **Rendering**: HTML5 Canvas 2D API (800×300 logical pixels, retina-aware via `devicePixelRatio`)
- **Audio**: Web Audio API — all sounds procedurally synthesized, zero audio files
- **Language**: Vanilla ES2020 JavaScript (classes, const, arrow functions, optional chaining)
- **Fonts**: Google Fonts CDN (Nunito for titles, Patrick Hand for UI/score)
- **Persistence**: localStorage for high scores only
- **Deployment**: GitHub Pages from main branch root

## Architecture

Everything is inline in `index.html`. The script section is organized into these logical sections:

1. **Constants & Config** — color palette, physics values, canvas dimensions
2. **Asset drawing functions** — procedural canvas art for cats, obstacles, backgrounds (painterly style via layered semi-transparent fills, `shadowBlur`, bezier curves)
3. **AudioEngine** — Web Audio API oscillator/gain-based synthesis for all sounds
4. **GameState** — central state object
5. **Entity classes** — Cat, Obstacle, Collectible, Particle
6. **InputHandler** — keyboard + touch (Space/Up=jump, S/Down=duck, Tab/X=swap cats, touch gestures)
7. **ScrollingBackground** — 3-layer parallax, 3 rooms looping (living room → hallway → kitchen)
8. **GameLoop** — `requestAnimationFrame` at 60fps, deltaTime-based physics
9. **Screen renderers** — TitleScreen, HUD, GameOverScreen
10. **init()** — entry point

## Key Design Decisions

- **Two cats with distinct mechanics**: Hank (orange, chonky, single jump, auto-collects treats) and Bailey (black, lean, double jump, random wall-stare freeze mechanic)
- **AABB collision detection** for obstacles and collectibles
- **3 lives system** with stun animation on hit
- **Difficulty curve**: obstacle speed and spawn frequency increase over time
- **All art drawn programmatically** — no image/sprite assets. Warm palette: parchment (#F5EDD6), orange (#E8824A), dark (#2C2C3A), wood (#C4A882), mint (#7EC8A4)

## Development

No build commands. Open `index.html` in a browser to test. Use any local HTTP server for development:

```
npx serve .
# or
python -m http.server
```

## Implementation Reference

See `docs/PLAN.md` for the full game design specification including obstacle types, collectible effects, audio synthesis details, and implementation order.
