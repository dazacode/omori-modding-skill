# Quality: what a good OMORI mod, map or event looks like

Use this before building and again before calling anything done. It is general: it applies to any OMORI mod or
RPG Maker event, not one project. A project can add its own bar on top (for example a benchmark scene it
must match), but the rules here apply everywhere.

## Before you start

Two files come before this one in any job: `director.md` (the scene blueprint: GOAL, BEATS, CHANGES, CUT IF) and
`anti-slop.md` (the gates for disclosure, licences and tells). A scene that fails either is not finished, whatever this
file says about its quality.

## Where these criteria come from

Written from what critics and players say about OMORI and about RPG Maker games, plus the game's own design
(the dumped data). Sources are listed in `sources.md`. Be honest about the limits: there are few critical
reviews of OMORI *mods*, so for mods the criteria are borrowed from OMORI and RPG Maker reception and from
design writing. Treat them as strong defaults, and confirm with playtests.

What critics praise in OMORI and in the best RPG Maker games:
- **A clear identity.** Its tone (cute surface, grim content), its voice, its art direction and music. Players
  remember OMORI for the way it looks and sounds as much as for its plot. Best-of lists praise distinct
  identity over polish in general (DualShockers on Ib, Fear & Hunger, OMORI; GameRant on Grimm's Hollow).
- **Writing that trusts the player.** Withheld explanation, fragments of story across the game, emotional truth
  in small conversations (Noisy Pixel, Gamelance).
- **Atmosphere and horror that accumulates.** Horror built from small wrong details, not jump scares
  (Gamelance: "accumulates" and "doesn't announce itself"; many RPG Maker horror lists).
- **Music and sound used as storytelling.** Quiet on purpose, loud on the release.

What critics criticise, and what this skill should prevent:
- **A mid-game that drags.** Critics name a stretch around two-thirds of the way through (Gamelance) and
  fantasy stretches with no plot information for hours (Shetani's Lair).
- **A setup that stumbles.** A room the player cannot solve nearly made one reviewer quit (Shetani's Lair).
- **Tension with no release, or rewards out of proportion.** "Bait and switch": a long build-up followed by a
  small payoff (Shetani's Lair). Good pacing alternates peaks and valleys.
- **Filler.** Fetch quests that are only fights, NPCs with nothing to say, comic side beats that could be
  cut (Shetani's Lair, Noisy Pixel).
- **Unskippable length.** Long unskippable events and conversations that repeat one beat (Noisy Pixel,
  Shetani's Lair).
- **Repetitive battles and encounters.** The same fight over and over; encounters every few seconds drive
  players away (RPG Maker forum comments; Noisy Pixel on boss repetition).
- **Feature overload and scope.** Starting too big is the most repeated warning in RPG Maker advice
  (forum "Tips on Making a Good RPG", about 6 posters on scope).
- **Sloppy text and invisible walls.** Spelling errors and unreachable or invisible blockers signal unchecked
  bugs (forum thread, Tiamat-86).

## The bar, in eight parts

### 1. Identity: it still feels like OMORI
- Use OMORI's voice: short lines, `......`, pauses (`\!`, `\|`), names in CAPS, plain third-person present
  narration. Use the game's own text codes, sprites and window style.
- Use the base game's art and sound first. Edit it when you need something new. Original art should match
  OMORI's palette and line weight. Do not use AI-generated sprites or art that sits outside the game's look.
- An adaptation or homage adapts freely. Keep the source's heart, cut or merge beats when the scene plays
  better, and say what changed in the credits.

### 2. Pacing: peaks and rests
- Every scene has a shape: setup, build, a peak, a release. Tension without release is stress; release
  without tension is boredom (Shetani's Lair).
- Use silence on purpose. A short pause before a key line lands better than another line of text.
- Keep the mid-game moving. If a stretch has no plot information and no new mechanic for more than about
  ten minutes of play, cut, merge or add a beat. Check a route's timing in a playtest, not on paper.
- Proportion: the reward for a sequence should match the effort and tension leading to it.
- Keep the story's engine running. An early reveal that gives away the stakes of an escapist sequence
  weakens the whole structure (Intermittent Mechanism). Let fantasy and reality share stakes.
- Let players move at their own pace: skippable text where the game allows it, no forced timers on reading.

### 3. Clarity: the player always knows what to do
- Each scene teaches its mechanic before it asks for it. A mechanic that is only explained in text fails
  on a hard setup (Shetani's Lair, Game Accessibility Guidelines: "interactive tutorials").
- No essential information carried by colour, sound or a flash alone. Pair the cue with text or an icon.
- Keep the screen readable: no tint, darkness or overlay that hides the character or the exit.
- Warn the player before a dangerous or hard area (a sign or a line of dialogue).
- Clear exits and signposts. Test the path with a fresh save, not with knowledge of the answer.

### 4. Choice and consequence
- A choice that matters changes something later: a flag, a route, a line that remembers it.
- Use self switches or variables so a scene does not replay after the player has seen it (`$gameSelfSwitches`).
- On failure, rewind gently and say why. Do not punish a player who did nothing wrong.
- Do not offer a choice that changes nothing. If every branch ends in the same place, make the path the
  same and remove the choice, or make the difference visible.

### 5. Staging: scenes are acted, not just typed
- A line of dialogue on a still screen works only when nothing is happening. When a moment matters, something
  moves: a reaction, a step, a camera move, a sound, a picture.
- Physical action uses frames, not the battle system: a held contact frame under a short shake or flash,
  then the character recovers. End every effect you start.
- Undo every effect at the scene's end: zoom, filters, tints, weather, tremble. Leftover state is the most
  common visual bug.
- Restraint: a heavy effect (vignette, flash, glitch) stops meaning anything if it stays on. Use each one at
  a peak and remove it straight after.

### 6. Sound carries emotion
- Every scene sets its music or ambience on purpose. A silent beat is a choice; make it one.
- Change music at emotional turns: silence before a big line, a theme on the release.
- Narration can have a sound under it when it fits: drips, a far door, a clock, wind.
- Look for chances to tell the story through sound (footsteps getting closer, breathing, a handle rattling).
- Every file the build needs must exist. A missing file should fail the build, not play silence.

### 7. Props, space and depth (maps)
- Scale against the characters. A character is about 30 px tall in the base game; a vanilla room door is about
  30 by 34 px; a chair about 26 px. Nothing ordinary is twice a person's height.
- Use a 3/4 view: a front face, a thin top surface, at most a small side panel. Not isometric.
- Anchor objects at the feet. The engine sorts same-priority events by their bottom edge, which decides who
  stands in front of whom. Furniture stands on the floor rows, never on wall tiles.
- Collision is the footprint, not the picture. Block the tiles the object stands on. A counter hides the
  NPC from the waist down by offsetting them down behind it.
- A doorway reaches the floor (jambs, lintel, sill, mat). A window or sign stays flat on the wall.
- Check every new prop on a real render next to a character before calling it done.

### 8. Craft, compatibility and release
- Mod hygiene: patch rather than replace where you can, so a game update fails loudly (`test` ops in JSON
  patches) instead of silently breaking data. Say which files you replace.
- Test on a clean install (or a profile) with the mod alone and then with the usual mods.
- Accessibility basics (Game Accessibility Guidelines): remappable controls, speed and text-pace options,
  readable default font size, subtitles for important speech, sound and visual cues not only in one channel,
  flashing kept low, settings saved. Most of these already exist in OpenOMORI's settings; a mod must not
  remove them.
- Credits and licences: every third-party file in a credits file (author, URL, licence). Fan art and
  unlicensed assets are marked and removed before publishing. OMORI's own community licensing guide uses
  CC BY-NC-ND 4.0 as the default for uploads; say what licence your mod uses.
- Never commit or publish game files (decrypted data, the original art, save files). Ship patches and your own
  assets only.

## Quality checklists

**Every dialogue file**
- Parses (no single backslash inside double quotes; see `formats.md`).
- Every `ShowMessage file.key` reference resolves. `mod lint` checks this.
- Text codes are known; portraits and `faceindex` are in range.
- Spelling and names match the game's canon and capitals.

**Every scene or event**
- Has a peak and a release, and runs in the time you planned (checked in a playtest).
- Teaches any new mechanic before it is needed.
- Undoes every effect it starts (zoom, filters, tint, weather, tremble, pictures, self switches it set).
- Has music or ambience chosen on purpose.
- Is skippable where the game allows, and not forced to repeat.

**Every map**
- Scale and anchoring look right next to a character.
- The exits and the way forward are obvious. A new player finds them without help.
- No soft-locks: a player who cannot leave a room, cannot finish a quest, or cannot reach a required item is a
  blocker.
- Encounter rate and dangerous areas are sensible (warn before the hard ones).

**Every mod (before release)**
- `mod check` (or the equivalent validator) passes; a clean install plays from the start.
- A full playthrough of the mod's route, not just the parts you wrote. Route checks with checkpoints if you have them.
- At least one person who did not write it has played the first hour and said where they got stuck or bored.
- Accessibility basics above are not broken. Credits and licences are in place.

## How to verify quality (not just that it runs)

1. **Playtest in sections and in full.** Section tests catch the broken scene; a full run catches the mid-game
   drag and the pacing problems (forum advice: about half of development should be playtesting).
2. **Time the parts.** Note where a section runs long or short, against the plan.
3. **Watch a new player.** Silence and the same mistake in two runs point to a clarity problem, not a player problem.
4. **Compare against a benchmark: a critically reviewed OMORI scene.** The benchmark is OMORI itself, because it is the
   work that critics reviewed (OpenCritic 86 from 11 critics, 100% recommending; Metacritic 88 from 6, all positive; reviewers
   praise its story and emotional impact and criticise its combat and pacing, per the search results in `sources.md`).
   Name one canon scene in the blueprint's REFERENCE line, play it, then play your scene, and ask whether yours reaches the
   same level on the same feeling. No fan work was found with critic reviews, so a fan project is not a benchmark for
   quality. A project may name a different canon scene in its own bar file.
5. **Look at the render.** Props, staging and effects are checked on a screenshot of the real map, not in the
   script.

## Anti-patterns to catch early
- An effect left on (vignette, tint, weather) after its moment.
- A long conversation with no information in it, or a scene that repeats a beat already seen.
- A scripted fight where the story does not need one; a real battle that repeats the last one.
- A choice whose two branches end the same way.
- A map with a blocker nobody can see, or a door the player cannot reach.
- A patch that silently does nothing because the target changed (use `test` ops).
- Dialogue that breaks the parser, or a `ShowMessage` pointing at a key that does not exist.
