# Level design for OMORI maps

Maps are where a player spends time, so a bad map costs more than a bad line. The OMORI-specific layout and prop rules
are in `quality.md` section 7. This file covers how a map guides a player and paces them.

Confidence: the principles below agree across several level-design guides (search results, mostly informal). The
numbers are not established; treat them as starting points and test them in a playtest.

## Orientation: landmarks and sightlines
- **Every area has a signature.** A distinct visual anchor (a tower, a tree, a coloured door) lets a player say where
  they came from. Featureless corridors confuse players quickly (sources agree).
- **Keep sightlines open to key destinations.** A player who can see the goal finds it. Block the view only on purpose.
- **Guide with light, colour and framing before any marker.** Warm, bright areas draw the eye; cool or desaturated
  areas read as quiet or dangerous. An arrow on the floor means the geometry failed (one guide's phrasing, search result).
- **OMORI test.** Put a new player in the room with no hint. Where do they look first? Did they move toward the exit?

## Pacing: intensity and rest
- **Alternate intensity and rest.** A level with no quiet part exhausts; a level with no tension bores. Sources
  suggest at least one high and one low moment per short section (figures unverified).
- **Mirror the scene's beats.** A map with a peak scene should have a quiet approach to it and a quiet exit from it
  (the scene blueprint's BEATS in `director.md`).
- **Rest is not dead air.** Quiet rooms still need something to notice (a sound, a small object, a line). An empty
  room for minutes reads as a stopped game (one designer's phrasing).
- **Loops and choice.** Multiple paths back to a hub feel more rewarding than a single line; a linear path gives full
  control over pacing but can feel on rails. Choose on purpose per map.

## Teaching
- **Introduce a mechanic in a low-risk space** before it is needed in a dangerous one.
- **Warn before a dangerous area** (a sign, a line of dialogue, a sound). Players accept a hard area when it is announced.
- **Encounter rate.** RPG Maker forum advice says frequent random encounters drive players away, and that repeating
  the same fight with no variation hurts more than the rate. Vary enemy groups; keep a safe route where possible
  (opinion from forum posts; not measured).

## Blockout and eye level
- **Build a rough blockout first and test it at player height.** A layout that looks logical from above can feel wrong
  at eye level (level design guides agree; search results).
- **Check every exit.** No soft-locks: a player who cannot leave a room, reach a required item, or finish a quest is a
  blocker. Test with a fresh save.
- **Collision matches what is drawn.** A visible gap that blocks the player, or a visible wall that does not, breaks
  trust. (`quality.md` section 7 covers footprints.)

## Checklist for a map
- [ ] Each area has a signature the player can name.
- [ ] A new player finds the way forward without help (test with someone who has not seen the map).
- [ ] The peak and the rest are placed on purpose (blueprint BEATS).
- [ ] Dangerous areas are announced; the first encounter is survivable.
- [ ] No blockers without a way round; no dead ends that look like exits.
- [ ] Render checked at the real player's zoom and position.
