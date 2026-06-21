# Solstice Village

A 3D exploration game built for the June Solstice. You are Sol, the courier. The longest night has fallen and every window in the village has gone dark. Gather light at the central brazier and carry it home to home until the sky turns gold.

**[Play it here →](https://aryakoste.github.io/solsticeGame/)**

---

## Gameplay

- Talk to each of the 8 keepers to learn their story
- Gather light at the plaza brazier — your lantern holds ~85 seconds of flame
- Carry the light to dark homes and deliver it
- Watch `dawnAmount` rise from 0 to 1 as the village lights up — every system in the game (sky colour, sun height, fog, moon fade, brazier size, NPC behaviour) is driven by this single float

There is a second ending. Collect all six spirit wisps scattered around the village without lighting any homes, and the game takes a different path.

---

## Controls

| Input | Action |
|---|---|
| `W A S D` | Move |
| `Shift` | Run |
| Click + drag | Rotate camera |
| `E` | Interact (talk, gather light, deliver, collect wisps) |
| `Esc` | Release mouse lock |
| **Mobile** | Left joystick to move, drag right side to rotate camera, Talk button to interact |

---

## Features

**World**
- Circular village layout — 8 homes at radius 17, lamp posts at radius 11.5, 32 swaying trees beyond
- Central brazier that grows larger and brighter as dawn increases
- Secret grove in the northeast with glowing mushrooms, a bench, and an ancient standing stone (its inscription changes based on your progress)
- Juniper's garden with flowers and a raised bed border

**Characters**
- 8 unique NPCs with different skin tones, hair styles, and two-line dialogue arcs
- Each keeper mentions another by name — the village feels like people who know each other
- NPCs slowly walk toward the plaza and gather around the brazier as dawn rises past 50%
- Roaming cat (follows you after you pet it), deer (startles and flees when you get too close), owl on a lamp post

**Atmosphere**
- Chimney smoke rises from every lit home — growing grey spheres that expand and fade
- Deep bell tone (110 → 880 Hz harmonic series, 3-second decay) rings when each home is lit
- Footstep audio — alternating low tones timed to your walking pace
- 28 fireflies on parametric curves, occasional shooting stars, aurora band of 12 semi-transparent planes
- 1,200 star particles in a single `BufferGeometry` draw call
- Full ambient drone (5-harmonic sine oscillator series at 55/82/110/164/220 Hz) — no audio files

**Mechanics**
- Lantern fuel drains over time — the HUD shifts from gold to orange to red as light fades
- Each spirit wisp shifts the lantern to its own colour (teal, blue, green)
- Enter any lit home to see its interior: hearth, ceiling beams, bookshelf, rug, and a note left by the keeper on the table
- Minimap (Canvas 2D): houses go from grey to glowing gold as you light them

**Alan Turing nod**
- A binary strip at the top of the screen displays each letter of "SOLSTICE" in 8-bit ASCII as homes are lit — one letter per house
- Mira the clockmaker: *"Every lit window is a 1, every dark one a 0. Light mine and we add a bit to the dawn."*
- The standing stone in the secret grove has a name beneath the runes: TURING

---

## Technical

Built with [Three.js r128](https://threejs.org/). No build step, no bundler, no dependencies beyond the CDN script.

**Notable implementation details:**

- `Object3D.lookAt()` in Three.js r128 uses camera convention (local −Z faces target). NPCs are oriented with `Math.atan2(-pos.x, -pos.z)` instead, so local +Z (the face) points toward the plaza
- Animation timing: `performance.now()` returns milliseconds. Missing a `× 0.001` conversion makes `Math.sin(now × 4)` cycle 637 times per second — the source of the "vibrating cat" bug
- Third-person camera: spherical coordinates (yaw, pitch, distance) to Cartesian with `cx = px - sin(yaw) × cos(pitch) × dist`
- House collision: player position is rotated into each house's local space, clamped to the box, then the push vector is rotated back to world space
- All visual state — sky, sun position, moon opacity, fog, lights, brazier, NPC behaviour — driven by a single `dawnAmount` float (0 → 1)

---

## Running Locally

```bash
git clone https://github.com/Aryakoste/solstice-village
cd solstice-village
# open index.html in any modern browser
# no server required
```

---

