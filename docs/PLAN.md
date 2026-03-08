# 🐱 The Zoomies — Implementation Plan

A cozy, charming endless runner starring Hank (a big friendly orange chonky boy) and Bailey
(a skittish, weird black cat who does the sideways Halloween crab walk and stares at walls for
no reason). Built as a single `index.html` for GitHub Pages — no build step, no dependencies.

---

## Repository Structure

```
/
├── index.html        ← The entire game (HTML + CSS + JS in one file)
└── README.md         ← Short description + screenshot for GitHub
```

GitHub Pages: enable from **Settings → Pages → Deploy from branch → main / (root)**.

---

## Tech Stack

| Concern       | Approach                                              |
|---------------|-------------------------------------------------------|
| Rendering     | HTML5 Canvas (2D context)                             |
| Audio         | Web Audio API — procedural chiptune, zero file assets |
| Fonts         | Google Fonts CDN (`Nunito` + `Patrick Hand`)          |
| Frameworks    | None — vanilla JS, no build step                      |
| Deployment    | GitHub Pages static hosting                           |

All art is drawn programmatically on canvas (painterly style via layered fills, soft shadows,
rounded shapes). No external image assets required.

---

## Game Design

### Concept
3am. The cats have the zoomies. The living room is a warzone.

The player controls **one cat at a time** and swaps between Hank and Bailey on the fly.
Each cat has a distinct movement personality. Obstacles scroll in from the right. The goal
is to survive as long as possible and rack up a high score.

### Core Loop
1. Game starts with a cozy title screen (both cats doing their thing)
2. Press **Space** or **tap** to begin
3. Cats run right-to-left scrolling background (living room → hallway → kitchen loop)
4. Obstacles scroll in; player jumps and swaps cats to survive
5. On collision → short stun animation → lose a life (3 lives total)
6. Game over screen shows final score + a cat reaction image drawn on canvas
7. Press Space / tap to restart

### Controls
| Input              | Action                    |
|--------------------|---------------------------|
| Space / Up Arrow   | Jump (active cat)         |
| S / Down Arrow     | Duck / flatten (active cat) |
| Tab / X            | Swap active cat           |
| Touch tap          | Jump                      |
| Touch swipe down   | Duck                      |
| Touch swipe left   | Swap cats                 |

---

## Cat Mechanics

### Hank 🟠
- **Personality**: Friendly, confident, always hungry
- **Size**: Noticeably chonkier than Bailey — wider hitbox but also more ground clearance on jump
- **Jump**: Single jump, floaty and generous (he is a big boy, the air holds him)
- **Duck**: Belly-flop flat to ground, very wide and low
- **Special**: Passing over a treat automatically collects it (Hank notices all food)
- **Idle animation**: Slow trot with slight body wobble (chonk physics)
- **Hurt animation**: Surprised face, floof puff

### Bailey 🖤
- **Personality**: Skittish, weird, chaotic neutral
- **Size**: Lean and slightly smaller
- **Jump**: Double jump available (erratic — second jump goes slightly sideways)
- **Duck**: Sideways Halloween crab walk stance (turns sideways, arches back, scuttles)
- **Special**: Randomly freezes for ~0.8s to **stare at a wall** (a blank section of background).
  This is telegraphed with a "..." speech bubble 0.5s before it happens so the player can swap.
- **Idle animation**: Jittery trot; occasionally glances sideways at nothing
- **Hurt animation**: Poof of fur, teleports slightly backward

---

## Obstacles

| Obstacle         | Behavior                                             | Best counter     |
|------------------|------------------------------------------------------|------------------|
| 🤖 Roomba        | Low, fast, hugs the ground                           | Jump over        |
| 💨 Spray Bottle  | Projectile arc from top-right                        | Duck under       |
| 🧳 Vet Carrier   | Tall, slow, ominous — both cats visibly react        | Jump + swap mid-air |
| 🧹 Broom         | Sweeping low then high — two-phase                   | Duck then jump   |
| 🪴 Knocked Plant  | Hank knocked it off earlier; it rolls toward them    | Jump over        |

Obstacles increase in speed and spawn frequency over time (gentle curve, not punishing).

---

## Collectibles

| Item          | Effect                               |
|---------------|--------------------------------------|
| 🐟 Treat      | +10 points; Hank auto-collects       |
| 🧶 Yarn Ball  | +25 points + brief speed boost       |
| ☀️ Sunspot    | Brief invincibility; Hank lies in it |
| 🐾 Paw Print  | Extra life (rare, caps at 3)         |

---

## Backgrounds (parallax scrolling, 3 layers)

**Layer 1 (slowest)** — Soft painted wall with warm wallpaper hints  
**Layer 2 (medium)** — Furniture silhouettes: couch, bookshelf, houseplants  
**Layer 3 (fastest)** — Floor with rug texture; occasional scattered toys  

Three room sections loop seamlessly: **Living Room → Hallway → Kitchen**

---

## Visual Style

**Palette**
```
Background warm:  #F5EDD6  (parchment)
Accent orange:    #E8824A  (Hank)
Accent black:     #2C2C3A  (Bailey)
Floor:            #C4A882  (warm wood)
Accent pop:       #7EC8A4  (mint — treats, UI)
UI text:          #4A3728  (dark brown)
```

**Drawing approach** — all canvas, painterly feel via:
- Layered semi-transparent fills for soft edges
- `shadowBlur` for ambient glow on lit objects
- Rounded rectangle primitives for cat bodies
- Bezier curves for ears, tails, paws
- Slight grain/noise overlay on background layers

**Font**
- UI / Score: `Patrick Hand` (Google Fonts) — handwritten-cozy feel
- Title screen: `Nunito` bold — rounded and friendly

---

## Audio (Web Audio API — no files)

All sound is synthesized procedurally using oscillators and gain envelopes.

| Sound             | Synthesis approach                                      |
|-------------------|---------------------------------------------------------|
| Background music  | 4-bar looping chiptune: square wave melody + triangle bass |
| Jump              | Rising frequency sweep (square wave)                   |
| Duck              | Short descending blip                                   |
| Collect treat     | Happy arpeggio (C-E-G, fast)                            |
| Collect yarn      | Playful trill                                           |
| Hit obstacle      | Low buzzy thud                                          |
| Cat swap          | Soft "bwip" tone                                        |
| Bailey wall-stare | Eerie single held note (slightly detuned)               |
| Game over         | Descending sad trombone arpeggio                        |
| New high score    | Victory fanfare arpeggio                                |

Mute button (🔊/🔇) in top-right corner. Audio starts only after first user interaction
(required by browser autoplay policy).

---

## UI & Screens

### Title Screen
- Animated Hank doing his chonky trot in place
- Animated Bailey doing the sideways crab walk
- Title: **"THE ZOOMIES"** in large `Nunito` bold
- Subtitle: *"3am. No reason. Full speed."*
- `[ Press Space to Start ]` pulsing prompt

### HUD (in-game)
- Top-left: Score (large, `Patrick Hand`)
- Top-right: 🔊 mute button + High Score label
- Bottom-left: Lives (3 paw print icons)
- Active cat indicator: small portrait of current cat with soft glow outline

### Game Over Screen
- Canvas drawing of both cats looking sheepish/tired
- "ZOOMIES OVER" title
- Final score + High score (stored in `localStorage`)
- `[ Again? ]` button

---

## Code Architecture (single `index.html`)

```
<html>
  <head> fonts, CSS </head>
  <body>
    <canvas id="game"></canvas>
    <script>
      // --- Constants & Config
      // --- Asset drawing functions (Hank, Bailey, obstacles, background)
      // --- AudioEngine (Web Audio API, all synth)
      // --- GameState object
      // --- Entity classes: Cat, Obstacle, Collectible, Particle
      // --- InputHandler (keyboard + touch)
      // --- ScrollingBackground
      // --- GameLoop (requestAnimationFrame)
      // --- Screen renderers: TitleScreen, HUD, GameOverScreen
      // --- init()
    </script>
  </body>
</html>
```

No classes that require transpilation — ES2020 vanilla JS (`class`, `const`, arrow functions,
optional chaining). Targets modern browsers only (GitHub Pages audience is fine with this).

---

## Implementation Order for Claude Code

1. **Scaffold** — `index.html` shell, canvas setup, game loop skeleton, input handler
2. **Background** — parallax room scroll (3 layers, 3 rooms looping)
3. **Hank** — draw function, idle animation, jump, duck, chonk physics
4. **Bailey** — draw function, crab walk animation, double jump, wall-stare mechanic
5. **Obstacles** — Roomba, spray bottle, vet carrier (3 is enough to start); spawn + scroll logic
6. **Collision** — AABB hit detection, stun/life-loss handling
7. **Collectibles** — treats, yarn balls; score counter
8. **Audio** — AudioEngine, all synth sounds, background music loop, mute toggle
9. **UI** — Title screen, HUD, Game Over screen, localStorage high score
10. **Polish** — Particle effects, screen shake on hit, Bailey's "..." wall-stare warning, fonts

---

## Out of Scope (keep it simple)

- No mobile app wrapper
- No backend / leaderboard
- No spritesheet loading (all drawn on canvas)
- No level editor
- No save state beyond high score in localStorage

---

## Notes for Claude Code

- Target file: `index.html` only. Everything inline.
- Canvas resolution: `800×300` logical pixels, scaled via CSS to fill container width (max 900px).
  Use `devicePixelRatio` for crisp rendering on retina displays.
- Game runs at **60fps** via `requestAnimationFrame`; all physics use `deltaTime`.
- Bailey's wall-stare should feel funny, not punishing — the warning window is generous.
- Hank should *feel* heavy and warm. Bailey should feel unpredictable and slightly uncanny.
- The whole thing should make someone smile within 10 seconds of loading it.
