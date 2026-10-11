# Directing an OMORI mod: the director's job

The skill's purpose is to keep the person in the director's chair. Tools and checks exist to serve decisions the
person makes, and to catch the places where a scene has no decision behind it. A mod made without decisions is
the kind people call slop, whatever tool produced it.

## What a director owns
No single source defines "game director" precisely; the Wikipedia article is a disambiguation page. The working
definition below is drawn from how Frictional Games describes its own horror process and from film staging
vocabulary (sources.md). The director owns:

1. **The emotional goal of each scene.** Frictional's question for horror is "Is this game scary?" before
   anything about fun. For any scene, the director says what the player should feel and when.
2. **The shape of each scene.** What happens, in what order, and where the peak and release sit.
3. **What the player sees, hears and can do** in that scene, and why each one is there.
4. **Cutting.** Every element has to serve the goal. Frictional lost whole sections of Amnesia late in development
   and treats cutting as part of the job.
5. **Sign-off.** Nothing ships because it runs. It ships because the director has played it and judged it against
   the goal.

## The scene blueprint (write this before any file)

Fill this in for every scene. If a line can't be filled in, the scene is not decided yet.

```
SCENE: <map id, event name, part>
GOAL:       what the player should feel by the end (one sentence, an emotion, not a plot point)
REFERENCE:  the scene in OMORI or the source it is closest to (name it; the pick is a decision)
OPENS:      what the player is doing when it starts; how it begins (a sound, a line, a movement)
BEATS:      1. setup    2. build    3. peak    4. release    (each: what happens, who acts, what the player does)
CHANGES:    what is different afterwards (flag, route, item, relationship, map state)
STAGING:    camera move, blocking (who stands where, who moves), effects (each with its undo)
SOUND:      music/ambience at each beat; silence where it is the choice; the sound on the peak
TIME:       planned seconds for the scene and for the skip path
UNDO:       every effect started here, and where it is stopped
CUT IF:     the line or beat that could go without losing the goal (name it)
```

Two rules come from the sources. First, if the GOAL is a plot point ("Kel finds the letter") rewrite it as a feeling
("Kel's dread that the letter is worse than it looks"). Second, CUT IF must name something real. A scene with nothing
to cut is usually a scene with nothing decided.

## Staging is the director's language

Film staging gives a vocabulary that transfers to RPG Maker events (Celtx: staging is the map, blocking the route,
camera placement the audience's seat).
- **Shot purpose.** An establishing view orients the player. An over-the-shoulder view builds intimacy in a
  conversation. A close-up is for an emotional beat. A wide shot shows the action. Pick the purpose first, then the
  shot. The camera in this engine is the `CAM` command (`CAM EVENT <id>`, `CAM PLAYER`, `CAM <x> <y>`), and it has a
  fixed speed scale (800 default, lower is faster).
- **The camera reveals the blocking; it does not fight it.** A move exists because someone moved. Show the
  character's action, then the reaction, then what the character sees.
- **Cuts can say more than one long take.** Reverse Shot's critique of God of War's one-take style: the cut that
  would have shown more was never made. In RPG Maker the cut is an instant camera or picture change; use it when a
  shot change is the point.
- **Control is a meaning.** Taking camera or movement control away (`AllowPlayerMovement` / `StopPlayerMovement`)
  says something about the character. Do it on purpose, and give it back with `AllowPlayerMovement`.
- **Every frame is composed.** Check the render. A prop or character in the wrong place is a staging error.

## Acting: what happens on the screen
- A line on a still screen is correct only when nothing should happen. When a moment matters, something moves: a
  reaction, a step, a camera move, a sound, a picture. (The quality reference, section 5, covers the technique.)
- Physical action is timed frames, not the battle system. Plan the hold on the contact frame and the recovery.
- Start an effect, end it. Each effect in the blueprint's UNDO line is a required step.

## Working with the team of one (you and your tools)
You may be the only person on the project. The skill's standing rule: you decide, the tools check. Tools do not
produce the decision. An AI tool can be asked to check or suggest, but a human decision goes in the blueprint,
and a human plays it.

## Review: how a director judges a scene
1. Read the GOAL. Play the scene once with no notes. Write down the first feeling you had.
2. Compare that feeling to the GOAL. If they differ, the scene is wrong, not the player.
3. Play it again with the sound off, then once with the text skipped. Each run tests one channel.
4. Time it against TIME. A scene that runs far over plan drags (see `playtest.md`).
5. Check the UNDO line: after the scene, the map looks as before, except for the intended CHANGES.
6. Decide: keep, fix (name the fix), or cut. Record the decision in the mod's notes.

## Common ways a director goes wrong
- **Plot-first goals.** Scenes that exist to deliver information feel like exposition. Start from the feeling.
- **Effects as decoration.** A vignette, a shake, or a glitch that is not tied to a beat has no meaning.
- **Scenes that restate.** A second scene that gives the same beat again. Check the previous scene's CHANGES.
- **Choices with no difference.** Every branch ends in the same place. Make the path visible or remove the choice.
- **Explaining.** The director lets the player infer. Frictional: explaining everything kills dread on contact.
- **Filler.** Side scenes or NPCs with no CHANGES and no GOAL. Cut them or give them a job.
- **Adding a mechanic to fill time.** New mechanics must serve the scene's GOAL. Otherwise they are padding.

## Working alone (one person, limited time)
The skill assumes a single person with limited time. The agent helps search, check and record assets, and it does not
generate art (`asset-sourcing.md`). The director's scope rules are: count the scenes before starting, cut any scene with
no GOAL, reuse the game's own art before making new art, ship less and polish more, and keep a known-issues list.
