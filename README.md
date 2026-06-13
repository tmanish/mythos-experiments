# Mythos Experiments

### Three machines that dream — each generated in a single shot with Claude.

Three self-contained pieces that run entirely in your browser. No servers, no APIs, no build step, no dependencies, no libraries — open the page and it works. Each is a single HTML file you can read end to end. They share one idea: that a few hundred lines of math, running on the hardware already in your pocket, can produce something that feels alive.

> **About the build:** each of these three was produced by Claude as a *first-try, working build* — no scaffolding, no framework, no external code. The source is short enough to read in one sitting and does exactly what it claims. If that sounds like a tall claim, the best rebuttal is the code itself: open any file and look.

| | What it is | Engine | Launch |
|---|---|---|---|
| **ONEIROS** | A volumetric dreamscape you fall through | WebGL raymarching | [▶ Experience it](https://tmanish.github.io/mythos-experiments/oneiros.html) |
| **AETHER** | Real fluid you can push with your finger | WebGL Navier-Stokes solver | [▶ Experience it](https://tmanish.github.io/mythos-experiments/aether.html) |
| **EVERSONG** | A song that has never existed, composed live | Web Audio + Canvas | [▶ Experience it](https://tmanish.github.io/mythos-experiments/eversong.html) |

All three run offline once loaded. All three are mobile-first and respect `prefers-reduced-motion`. Open any of them, turn the brightness up, and — for EVERSONG — put headphones on.

---

## ONEIROS — *the machine is dreaming, and you are inside it*

### [▶ Launch ONEIROS](https://tmanish.github.io/mythos-experiments/oneiros.html)

A luminous nebula coils around a singularity while you fall toward it. Move your pointer and the dream drifts with you; tap and a shockwave ripples through the cloud; hold and you descend, the speed climbing as the edges of vision warm into fever.

### Why it's special

There is no 3D model here. Nothing is drawn the way games draw things — no polygons, no meshes, no textures loaded from disk. The entire scene is **raymarched**: for every pixel on your screen, the program shoots a ray into an imaginary volume and marches it forward step by step, asking at each step *how dense is the dream right here?* The answer comes from layered noise functions evaluated on the fly. The color comes from a cosine palette. The swirl comes from rotating space itself more tightly the closer a ray gets to the core.

That means the dream is **infinite and non-repeating** — there is no loop point, no pre-rendered frame, nothing cached. Each frame is computed fresh from the clock and your cursor, sixty times a second, on your GPU. The film grain, the stars occluded by cloud density, the way the whole field breathes — all of it is generated, not stored.

It's the oldest trick in real-time graphics done with care: proof that *space and light can be conjured from a formula.*

---

## AETHER — *real-time fluid, solved on your GPU 60× a second*

### [▶ Launch AETHER](https://tmanish.github.io/mythos-experiments/aether.html) — then drag.

Luminous dye swirls through a dark field, curling and folding the way smoke does. It's already moving when you arrive — and the moment you drag your finger across it, the fluid responds: you inject motion and color, carve channels, fling pigment that diffuses and dissipates exactly as a real fluid would.

### Why it's special

This is not an animation of fluid. It is **fluid dynamics** — the actual Navier-Stokes equations, the same mathematics engineers use to model airflow over a wing, solved live on your phone's graphics chip.

Every frame, the simulation:

1. **Advects** the velocity field through itself (the fluid carries its own motion forward).
2. Runs a **20-iteration pressure solve** to enforce incompressibility — fluid can't be squeezed, so the solver finds the pressure that keeps it consistent.
3. Applies **vorticity confinement** — and this is the detail that makes it beautiful. Numerical simulations naturally smear away the small curls and eddies; this step detects them and re-injects energy to keep them sharp. It's why AETHER swirls like real smoke instead of dissolving into mush.

All of this happens in floating-point textures ping-ponging through GPU shaders, with a tone-mapped bloom on top for the glow. It checks your hardware for floating-point support when it loads, falls back gracefully if needed, and quietly drops its resolution if the frame rate ever dips — so it keeps flowing on a wide range of devices.

Three autonomous emitters keep the field alive when you're not touching it, tracing looping paths and cycling color. Let go, and they surge back in to reclaim the space.

It's a physics engine wearing the disguise of a toy.

---

## EVERSONG — *a song that has never existed, and will never repeat*

### [▶ Launch EVERSONG](https://tmanish.github.io/mythos-experiments/eversong.html) — headphones on, tap BEGIN.

A complete piece of music begins to compose itself in real time — pads, sub bass, drums, and a melodic hook — and then keeps composing, indefinitely, never looping. The visuals are the song's own voiceprint: a core that pumps with the bass, a radial burst that *is* the live frequency spectrum, a ring for every melody note, a shockwave for every kick.

And you conduct it. **Press and drag:** raise your hand and the music lifts — drums layer in, the filter opens, harmonies stack; lower it and everything hushes into reverb. Slide right and the melody brightens an octave. The two meters in the corner (ENERGY and BRIGHT) show your hand's position registering live.

### Why it's special

Most "generative music" toys play back pre-recorded samples or loop a fixed pattern. EVERSONG has **no audio files at all.** Every sound is synthesized from oscillators in the browser, and every note is *chosen* by a small composition engine built on real music theory:

- It picks a **key and tempo at random** each time you open it — so your session is unrepeatable by construction.
- Chords move through a **weighted transition graph** over the scale degrees, the way a songwriter leans toward some changes more than others — so the harmony wanders but never sounds random.
- The melody is built from a **motif** — a short phrase the piece keeps returning to and *mutating* over time, the way real songs develop a hook rather than spraying notes.
- The whole thing follows a **structural arc** — it rises for a stretch, peaks, hushes, then rises again with a fresh motif. Tension and release, on a loop that never actually loops.
- The reverb is a **convolution hall generated from scratch** at startup — the acoustic space itself is computed, not sampled.

The composition logic was stress-tested through **8,000 chord transitions and 5,000 motif mutations** to confirm it never stalls, never leaves the key, and can genuinely play forever.

Your finger isn't a volume knob. It's a conductor's hand over a band that's writing the music as it goes.

---

## Running them locally

Each file is fully standalone. Clone the repo and open any `.html` file directly in a modern browser (Chrome, Safari, or Firefox, with hardware acceleration enabled). There is nothing to install and nothing to configure.

```
git clone https://github.com/tmanish/mythos-experiments.git
cd mythos-experiments
open oneiros.html   # or aether.html, or eversong.html
```

**Requirements:** a browser with WebGL (ONEIROS, AETHER) and Web Audio (EVERSONG). All three degrade gracefully and tell you plainly if your device can't run them.

---

## A note on what these are

These aren't products. They're demonstrations of a single conviction: that the line between *rendering* something and *generating* something is where the interesting work lives. None of these three store the thing they show you. The dream, the fluid, the song — each is computed, live, from a handful of equations and your own input. Close the tab and it's gone. Open it again and you get something new.

That's the point. They're meant to make you ask, for a second, whether what you're looking at is real or whether you're dreaming it — and then to show you it was just math, running on a machine you already own.

*Built to be read, run, and taken apart. Open the files.*
