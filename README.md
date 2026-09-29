# Cove Beach

An interactive tropical cove rendered in real time in the browser: a crescent of white sand between limestone cliffs, a sea arch, a pier and boardwalk, palms, reef and sea life, and a little hillside village behind the beach. Everything (terrain, water, textures, models, sound) is generated in code; the whole thing is one file, `cove-beach.html`.

**Live demo: <https://aeronh1.github.io/Cove-Beach-Showcase/>**

## Running it

The live demo above runs it straight from GitHub Pages. To run it locally, open `cove-beach.html` in a desktop browser (Chrome, Edge or Firefox; WebGL 2 required). It needs an internet connection the first time to load two libraries from cdnjs:

- [three.js](https://threejs.org/) r160
- [dat.gui](https://github.com/dataarts/dat.gui) 0.7.9

Nothing else is downloaded. Loading takes a few seconds while the scene is built.

If your browser blocks local files, serve the folder instead:

```bash
python -m http.server 8765
```

then visit <http://localhost:8765/cove-beach.html>.

A laptop GPU should hold around 60 fps at High quality; the page lowers resolution and then quality on its own if it can't keep up (see *Auto-drop* under Rendering).

## What's in the scene

- **The cove**: a sandy beach with wet sand, swash and foam where waves run up; limestone cliffs with ledges, plants and a sea cave; a sea arch and sea stacks at the mouth; a rock shelf with tide pools that fill and drain with the tide.
- **The water**: layered ocean waves that grow, steepen and break toward the shore, reflections and refraction, caustics on the sea floor, underwater fog and light shafts, foam trails, and bioluminescence at night.
- **The beach**: striped umbrellas, loungers, towels, a sandcastle, a lifeguard tower, a beach bar (cabana), surfboards and kayaks, a boardwalk on the left hillside, and a pier with stairs, lanterns and a moored rowboat.
- **The village**: about 25 pastel houses stepping up the hill, a chapel with a bell tower facing a cobbled plaza with a fountain, a main street and a lane down to the beach, and street lamps that come on at dusk.
- **Life**: a fish school and reef fish, a sea turtle, crabs, gulls and sandpipers.
- **Weather**: Sunny, Golden hour, Night and Storm, cross-fading between each other. The tide rises and falls over a few minutes.
- **Sound**: waves, wind, gulls, rain and the boat creaking, all synthesised live (starts on your first click or key press).

## Controls

The film tour plays on its own. Drag or press a movement key to take over the camera.

| Key | Action |
| --- | --- |
| `W` `A` `S` `D` | Move (switches to manual camera) |
| `Q` / `Space` | Move down / up |
| `Shift` | Move faster |
| Drag | Look around |
| Mouse wheel | Change movement speed |
| `C` | Cycle camera mode: film tour, manual, follow |
| `K` | Follow the turtle, the fish school or a gull (press again for the next) |
| `P` | Hold the current tour shot |
| `O` | Freeze time |
| `V` | Clean view (hide all UI); `Esc` to exit |
| `T` | Next weather |
| `B` | Build a tower on the sandcastle |
| `M` | Mute / unmute |
| `H` | Show / hide the controls panel |
| `I` | Show / hide the guide |

**Clicking** in the scene: click the water to splash, the beach ball to kick it, a crab to startle it, or a gull to scare it off.

The **toolbar** at the bottom has buttons for camera mode, hold, freeze, clean view, weather, sound and *Build a tower*.

## Controls panel

Press `H` (or *Open Controls*, top right) for the full panel:

- **Environment**: weather preset, time of day, sun direction, wind, tide period and range.
- **Water**: clarity, swell height and direction, foam, caustics, bioluminescence.
- **Life**: number of fish, gulls and crabs; turtle on or off.
- **Rendering**: quality (High / Low), auto-drop, exposure, bloom, ambient occlusion, god rays, grain, vignette, sharpening.
- **Lens**: depth of field, f-stop, maximum blur size.
- **Camera**: film tour, letterbox bars, change weather each loop, camera mode, follow target, path speed, stay above water, hold, freeze, clean view, volume, sound.
- **Close-ups**: jump straight to a tour shot (the cove from above, breaking waves, the sea arch, tide pools, palms and hammock, the village on the hill, the sandcastle, beneath the waves), line the fish species up for a look, or follow the turtle.

## The film tour

The tour loops through: the cove from above, the beach, breaking waves, the sea arch, tide pools, palms and hammock, the village on the hill, the sandcastle, seagulls, beneath the waves, and back to the beach. Turn on *Change weather each loop* to see a different preset every time around.

## How it's built

Everything lives in the single `<script>` in `cove-beach.html`, in numbered sections: parameters, noise and texture generation, geometry helpers, renderer / sky / sun, shared material shader patches, terrain, cliffs and rocks, structures (boardwalk, pier, boat, cabana, lifeguard tower, village), vegetation, decorations, sea life, water, post-processing, camera / interaction / audio, weather, and UI plus the main loop.

A few notes for anyone changing it:

- **Materials** are standard three.js materials extended through one `patchMaterial()` hook that adds swaying, underwater tinting, caustics, fog and night lighting from the pier lanterns and village street lamps.
- **Layers** separate what each render pass sees (above water, underwater, reflection, refraction, sky, water surface).
- **Placement**: props, vegetation and houses avoid each other through a shared list of exclusion circles (`EXCL`), and anything that stands on the ground samples the terrain with `groundAt(x, z)`.
- **Debugging**: `window.COVE` exposes handles for the console, e.g. `COVE.setWeather('Night')`, `COVE.setMode('Manual')`, `COVE.setCam([x, y, z], [tx, ty, tz])`, `COVE.advance(seconds)` to step the world, and `COVE.profile()` for timings.

The git history records each round of work, one commit per change with the reasoning in the message body.
