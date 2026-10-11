# Writing OMORI dialogue: craft and mechanics

Dialogue is the most common thing a mod adds, and the most common place for slop. This file is the craft rules and
the mechanics that make them work in this engine. Every rule has a source or a test.

## The voice (OMORI's own)
From the quality reference and the game's own dialogue [dump]:
- **Short lines and rhythm.** Lines are brief. Long speeches are broken by `\!` (wait for a press) and `\|` (one-second
  pause). Ellipses `......` do the work of silence.
- **Names in CAPS** in the text (`BASIL`, `MARI`), and the speaker box set by `\n<NAME>` or a macro (`\bas`).
- **Narration in plain third-person present** ("SUNNY throws it away."). Keep to the game's own grammar: fragments
  are allowed, and the voice is quiet, not florid.
- **Emphasis is rare.** `\QUAKE[1]` and `\SINV` change a line's feel; the base game uses them at the peaks. Overuse
  turns emphasis into noise.
- **Names and speakers.** Unknown or unnamed characters are written "???" until introduced; minor characters and
  narration get no name (Game Developer guidance, search result). OMORI's own `\who` macro expands to `\n<???>`.

## Craft rules (from Jon Ingold's AdventureX 2018 talk, as summarised by Radiator Blog)
Each rule is one the director can test on a line.
1. **Dialogue does dramatic work.** Each exchange moves the scene. Test: name what changed after the line.
2. **Write the subtext, not the stated meaning.** Test: what does the line mean beyond its words? If nothing, rewrite.
3. **Economy of words.** Fewer words, more happening. Test: cut ten percent and read it again.
4. **Do not over-explain.** Characters do not explain what everyone in the world already knows. Test: would this
   character say it to this person?
5. **No filler options.** A choice like "What do you think?" that exists only to give the player options is a
   non-choice. Test: does the option have a reason in the moment?
6. **No lines that state the theme.** Test: read the line aloud. Real people don't say it.
7. **Characters respond to what was said.** A character who ignores a question breaks the scene's logic.
8. **Structure choices as Accept / Reject / Deflect.** Three options work better than two (which implies a correct
   answer) or four (which lacks focus).
9. **Loops and trapdoors control pace.** A loop returns the conversation to its start while it circles at one
   intensity; leave the loop when it escalates. A trapdoor lets the player skip the small talk ("I'm here to see
   Tyrell").
10. **Make punishing loops meaningful.** A loop that resets the player after a bad choice should show why.
11. **Branching is characterisation.** Ingold: "Branching is action, action is character, character is drama."
    Loops slow a conversation; trapdoors speed it; both show mood.
12. **Draft, then ask what is left unsaid, then revise.** Ingold calls writing advice "useful bullshit", a heuristic,
    not a rule. The director decides when a line is done.

## Mechanics: what the engine does with your text [dump] [wiki]
- **Message window.** The game's message core defaults to **4 rows**, a **28 px** font, and **word wrap on**
  (plugin parameters in `js/plugins.js`, YEP_MessageCore). OMORI may override this; check in a playtest before you
  write long lines. A line longer than the window is wrapped and paged by the engine.
- **Pauses and pacing.** `\.` is a quarter-second, `\|` one second, `\!` waits for a press, `\^` no wait,
  `\>` shows the rest at once, `\<` cancels instant display. Use pauses to let a line land, not to fill space.
- **Choices.** `AddChoice file.key Label` for each option, then `ShowChoices N-1`. Each option is a dialogue key.
  Test every branch in a playtest.
- **Self switches** keep one-time lines one-time: `$gameSelfSwitches.setValue([map, event, 'A'], true)`.

## Keys, files and references
- Keys: letters, digits, underscores. `message_N` is the usual form. A key with a space cannot be referenced.
- The file is `languages/en/<mod>_<topic>.yml` (OpenOmori: `files/languages/en/`). Reference from events with
  `ShowMessage <mod>_<topic>.message_N`.
- Run `mod lint` (OpenOmori) to check references, text codes, faces, and the style warnings in `anti-slop.md`.
- YAML trap: in a double-quoted string write `\\!`, or use single quotes. (`formats.md` section 2.)

## Writing process (the director's)
1. Blueprint first (`director.md`): GOAL, BEATS, CHANGES, CUT IF.
2. Draft the beats in plain text, not in YAML, so the writing is judged without markup.
3. Read each line aloud. Apply the tests above.
4. Put it in YAML. Run the lint. Play it in the room.
5. Cut the first pass by a tenth. Check that every line still has a job.
