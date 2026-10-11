# Sprites and animation craft for OMORI mods

The file-level rules (sheet sizes, `$`, `!`, `[SF]`, `%(n)` markers, portrait size) are in `formats.md` section 4, and
`mod lint` checks the names and frame divisibility. This file is about making the frames read well.

Confidence: timing advice agrees across several pixel-art guides, but the exact millisecond values disagree from one guide
to the next. Use them as starting points and test them in the game.

## Timing first
- **Timing beats frame count.** Guides agree that uneven, deliberate frame timing improves an animation more than adding
  frames. Even timing is the common beginner mistake.
- **Think in milliseconds per frame, not fps.** Vary the timing per frame. RPG Maker animation timing is set per
  character (`wait` values in the animation or event movement) and per picture; check the engine's real unit in the
  game before you tune numbers.
- **Pixel art usually reads well at about 8–12 frames per second.** Going higher can look floaty (search results). The
  base game's own cadence is the best reference: copy it.

## The three parts of an action
1. **Anticipation.** A small move opposite to the action (a pull back before a strike, a crouch before a jump). It
   telegraphs the action so the player's eye knows what is coming.
2. **Contact.** The frame where the thing happens (the hand reaches, the foot lands, the impact). Guides agree the
   contact frame is held longer than the frames around it. Test the hold: too long looks frozen, too short looks weak.
3. **Recovery.** The settle after the action (ease back to idle over a few frames rather than snapping).

For RPG Maker, the same three parts are done with the frame order and holds in the action's frame list (see
`quality.md` section 5 on the acted-frame technique, and the project's own action tools for specifics).

## Squash, stretch and small motion
- Even at small sizes, one pixel of compression before a jump or a stretch at the peak can make a move feel right. The
  change is so small it is not noticed; removing it is noticed.
- Hit reactions should be short (guides suggest two to three frames; test it).

## Reuse before drawing
- Use the base game's action cells first (they are already timed to the game's rhythm). Edit a missing pose from the
  character's own run or walk cells, in their skin colours and outline.
- A new pose that does not match the outline weight, shading direction or palette of the base sheet looks foreign. Put it
  next to three base sprites at the same zoom before you accept it.

## Emotion and state variants
- The base game's emotion variants change the `Index` of the looping motions (the wiki's table: Neutral 0, Happy 5,
  Sad 3, Angry 4; Damage and Dead stay the same) [wiki]. Reuse the pattern for any new variant rather than drawing an
  unrelated sprite.

## Checks before a sprite is accepted
- [ ] Feet on the floor line, or deliberately lifted (a flying or floating character).
- [ ] Palette and outline weight match the nearby base sprites.
- [ ] Frame count divides the sheet width (`mod lint` checks this for `%(n)` sheets).
- [ ] Timing is not uniform across the cycle.
- [ ] The pose reads at the game's real size, not only zoomed in.
- [ ] Credits entry if any base-game art was edited, or an AI-generated part was used (`anti-slop.md`).
