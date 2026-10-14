---
name: ascii-motion
description: Build an animated figure made of code characters (ASCII art) for a web page, the kind that runs, gallops or drives as you scroll and morphs from one thing into another. Use when asked for an ASCII person, animal or car, a "code characters" figure, a scroll-driven character in the corner or along the bottom, or anything like the Pinterest ASCII runner gifs. Also use when an ASCII figure looks blobby, like clay, like the wrong animal, or when its transitions look like a fade.
---

# ASCII motion figures

The working reference is Eli's **Code Runner** artifact
(https://claude.ai/artifact/582hDAKcRSpYCgJ3xSpDf5, built 2026-10-14). A guy sprints
along the bottom of the page, turns into a galloping cheetah at one third,
then into a wedge supercar at two thirds. Read its source before starting
another one. It took three rounds of corrections to get there, and every
rule below is one of them.

## How the engine works

1. Each form is a shape drawn every frame on a tiny hidden canvas sized to
   the character grid (cols x rows), with the transform scaling world units
   to cells. Canvas anti-aliasing gives per-cell coverage for free.
2. Near-side parts are drawn in red, far-side limbs in green, so far limbs
   render lighter.
3. Coverage maps to a tier: heavy characters (`$@B0N8&%`) inside, medium,
   light (`~^;:'.`) on the edges, and a little random edge dust. Characters
   persist per cell and re-roll a few times a second (flicker).
4. Morphs blend two signed distance fields (chamfer distance transform on the
   grid), so one shape flows into the other. Hiding cells at random is a
   dissolve, not a morph.

## Never do these

- **Never build a body from round capsules and circles.** On 2026-10-14 Eli
  said the guy "looks like a clay figure that's moving" and "he has boobs":
  a chest circle pushed forward reads as breasts. Build limbs and torso as
  muscle profiles with separate front and back radii (quad and hamstring,
  shin and calf, flat chest and broad back, glutes). Make the head bigger
  than feels right and the neck thin: at ASCII resolution a realistic head
  reads as too small.
- **Never invent motion from memory when a reference exists.** Eli sent
  "Asafa Powell Slow Motion Side View HD" (YouTube FwGi9S7wkgk; ~9s is the
  clean full-speed side frame). Open the video in the built-in browser,
  seek the `<video>` element with `currentTime`, and screenshot frames. Copy
  what they show: thigh driven up to level with the shin hanging straight
  down, the push-off leg long and straight, the hand to the chin, the back
  elbow driven way past the body and opening up. The torso is nearly upright
  at top speed and leans hard only while accelerating.
- **Never ship an animal that reads as a different animal.** The first
  cheetah "almost looks more like a dog" with "a weird ball on its neck."
  Look at real side photos first (Wikimedia Commons API search with
  `iiurlwidth`). A cheetah has a round skull with a short blunt muzzle, small
  ears set back low, a short thick neck, a deep chest, a belly tucked up hard
  behind it, and a tail that swoops low then flicks up at the tip. Delete
  any stray decoration dot: it shows up as a lump in the characters.
- **Never leave a tail or loose part static.** "Give it some tail movement."
  Use a travelling wave along the segments, bigger toward the tip, tied to
  the gait phase, with a slow idle sway when standing.
- **Never let a morph snap or dissolve.** He asked for slow changes ("make
  the scroll speeds decently slow so the person doesn't change ... too
  quick"), and later for the code to "scatter a little bit ... and then come
  back together." Rate-limit the stage (about 1.8s per change), pose into the
  next shape first (the guy drops to all fours, the cheetah stretches long
  and low), then SDF-blend while every character flies out from the center
  and back, peaking mid-change.
- **Never pick the page split yourself.** He wanted equal thirds, one per
  form, with the change triggered on crossing each line.
- **Never trust a debug view you didn't render big.** Draw the hidden
  shapes at 4x scale in an overlay to judge anatomy, then check the real
  characters at the target size. A shape that looks fine at 4x can still
  collapse into a stick at 46 rows.

## Motion details that worked

- The figure travels across the bottom of the screen with page progress
  (left at the top, right at the bottom). Scrolling up turns it 180 by
  animating the x scale through cos(turn), which reads like a sprite turning.
  Center each form in its box or it jumps sideways when it turns.
- Gait speed comes from smoothed scroll velocity. Walk to run ramps slowly
  (ease about 0.035 per frame). Standing still keeps the flicker and a small
  breathing bob.
- A car needs motion cues more than detail: wheel spokes that turn into a
  blurred disc at speed, dust off the back wheel, streaks behind, a short
  exhaust flame when fast, a dip under acceleration. Eli liked a low wedge
  (long flat hood, steep windshield, side intake, hex arches, lip spoiler).
- Test buttons (slow, fast, stop) must hold the current form. A fake scroll
  that drifts through the stages hides bugs.

## Full-screen scenes and dark pages (eliutley.com gate, 2026-10-14)

- **Never make the effect cartoonish.** On the torch password page, Eli said
  the first version "seems so cartoonish," and later that the rock wall
  "looks like cartoon cobblestone." He wanted "a dark grainy look... like if it
  was dark and the camera was viewing it." What worked: film grain at 24fps,
  the room rendered at half resolution, and a raycast box room with
  inverse-square light pooling on a smooth concrete floor. Bump-mapped rock,
  tile seams and a held torch all read as cartoon. He also asked to "get rid of
  the lines in the floor."
- **Never put his life into the content unless he asks.** The first version
  hid facts about his mission, job and degree in the code on the walls. He
  said: "unless I ask you to do that, don't do that." Use neutral code.
- **Never drop the signature cursor between stages.** After the gate, the
  burning cursor vanished during the intro and turned back into a normal
  arrow on the monitor screen. Eli: "my clicker isnt even on fire anymore lol"
  (2026-10-14). Draw the cursor on its own top canvas so it burns over every
  layer, and let it go out with the black and relight by itself.
- **Never light everything.** He liked that the fire "doesn't light up the
  entire screen" so "you have to actually look for it." Keep any hidden UI
  (like the password prompt) low and off-center, visible only near the light.
- **A top-down figure only reads on a near-black background.** The first
  desk scene filled the desk with mid-gray, and the guy turned into a
  mushroom of characters. Fixes that worked: desk almost black (about 0.04)
  with a cutoff of about 0.07 so it draws nothing; a dark ring around the head
  and dark outlines around the arms so the parts separate; hair darker than
  the shirt and skin brightest; and frame tight (about 230 world units wide)
  so he's big in cells. Check with a `?grid` debug view of the hidden canvas.
- **A globe made of characters:** sample NASA Blue Marble
  (`cdn.jsdelivr.net/npm/three-globe@2.31.0/example/img/earth-blue-marble.jpg`,
  CORS ok) by inverse orthographic projection per cell. Use land contrast of
  `smooth01((lum-0.13)/0.2)`, keep the clouds light so they don't hide the
  continents, and map brightness onto a 70-glyph ramp with ±1 flicker.
- **Changes between forms are particles with identity.** Each character flies
  from where it was to its new cell, matched by angle around each center.
  Give the build a `?at=<seconds>` jump so each stage can be checked without
  replaying the whole sequence.

## Checking it

- Serve the page locally and drive it in the built-in browser. Check each
  form in each state, mid-morph frames, the turnaround, and phone width.
- If the pane is hidden, rAF stops and screenshots go stale. Read state
  through JS (`getBoundingClientRect`, the status text) instead of trusting
  the image.
