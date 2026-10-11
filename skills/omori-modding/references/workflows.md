# Workflows: two ways to make an OMORI mod

OpenOmori is **recommended**, not required. A mod can be made with the traditional tools described in the community
wiki alone. Pick the path the person is using; do not force OpenOmori on someone who wants RPG Maker.

| | Traditional (wiki) | OpenOmori (recommended) |
|---|---|---|
| Runs the game | Steam game + OneLoader (loader) | `bun run start` in the browser |
| Edits | RPG Maker MV (paid) + OneMaker MV, Tiled 1.0.3, a text editor for YAML | Plain JSON/YAML/JS files and a short map spec |
| Dialogue | Hand-written YAML in `languages/en/` | Same YAML (the same files) |
| Maps and events | Drawn in RPG Maker MV (events) and Tiled (tiles, collision) | `maps/<id>.spec.json`, compiled by `mod map build` |
| Checks | Playtest in RPG Maker MV; read the OneLoader log | `mod check`, `mod lint`, `mod find`, `mod say` |
| Packaging | Bundletool builds a zip from a playtest folder | `mod pack` makes a zip; drag onto the game |
| Cost | RPG Maker MV licence (Steam) | Free; needs Bun |

Both paths produce the same kind of mod: a folder or zip that patches or replaces the game's files.

## Traditional path (wiki)

Source pages are listed in `sources.md`. Steps as the wiki gives them:

1. **Install OneLoader** on your copy of the game (https://github.com/rphsoftware/OneLoader/releases). It is the
   mod loader; "required to create and install mods". Its install page is on mods.one. [wiki]
2. **Decrypt to a playtest project.** In the game's options, open the **Mods** tab, under *Decrypt* set
   Decryption Mode to "Everything" (or "Base Game" for vanilla only), set "Generate as Rpgmaker project?" to
   "Yes", press *Start decryption*. Output: `www_playtest_<id>` in the game folder. [wiki]
3. **Install RPG Maker MV and OneMaker MV.** OneMaker MV 1.2.2 (the wiki's version) is the modified editor for
   OMORI modding. Windows: run the installer's `.bat` (you may need to allow it past antivirus). Linux or manual:
   copy the contents into the RPG Maker MV folder. The compatibility plugin (`Plugins.7z`) goes in
   `js/plugins/` and is added in the Plugin Manager. [wiki]
4. **Open the playtest project** in RPG Maker MV (File > Open) and playtest with the green play button. [wiki]
5. **Edit.** Map events and the database in RPG Maker MV. Maps' tiles and collision in Tiled 1.0.3 (newer
   Tiled can make maps the game does not load). Dialogue in VS Code (or any editor) as YAML in
   `languages/en/`. Quest and dealer text in `data/Quests.yml` and `languages/en/System.yml`. [wiki] [dump]
6. **Package.** Bundletool (mods.one/mod/bt) builds a mod zip from the playtest folder's changed files. [wiki]
7. **Install.** Drop the zip onto OneLoader's mods folder without extracting it. [wiki] (A Steam discussion says so.)
8. **Mod metadata for OneLoader.** A `mod.json` with `manifestVersion: 1`, `id` (unique on mods.one), `name`,
   `description`, `version` (e.g. `"v1.0"`), and a `files` object listing paths. The full schema is at
   https://rph.space/oneloader.manifestv1.schema.json (not fetched here: check it before shipping). [wiki]

Things the wiki says about the traditional route that matter in practice:
- Event documentation on the wiki is incomplete; it recommends YouTube tutorials for the event system. [wiki]
- The Event Plugin Commands page covers only `ShowMessage`, `AddChoice`/`ShowChoices` and `CAM`. Most
  plugin commands in the game are not documented there (see `formats.md` section 5 for counts). [wiki] [dump]
- Use the community resources (listed in `sources.md`) for plugin commands the game uses.

## OpenOmori path (recommended)

Needs Bun (https://bun.sh) and Chrome, and the player's own copy of the game (the folder with `OMORI.exe` and
`www/`). The repo contains no game files. [OpenOmori]

```
bun install
bun run start                       # asks for the OMORI folder once; open http://127.0.0.1:8080/
bun run mod dump                    # decrypt your install into ./dump (git-ignored): read it, grep it
bun run mod find "COLD STEAK"       # search the dump: file:line
bun run mod say 01_map_whitespace.message_4   # read one dialogue line (or: mod say <text> to search)
bun run mod macros                  # the speaker and variable shortcuts (\kel, \bas ...)
bun run mod commands 25             # what the game uses: plugin commands, script calls, event codes
bun run mod new my-mod              # scaffold mods/my-mod
bun run mod check my-mod            # validate and dry-run every edit against the game files
bun run mod lint my-mod             # dialogue, text codes, faces, sprite names, ShowMessage references
bun run mod pack mods/my-mod        # my-mod.zip, ready to share
```

Mod folder: `mod.json`, `files/` (replacements, mirroring `www/`), `patches/` (edits), `scripts/` (browser
JavaScript). Patch with `test` ops first so a game update fails loudly. Full reference: `openomori.md`.

Dev loop [OpenOmori]: put the mod in `./mods`, open the game with `?hot=1`, edit a file, reload; read
`latest.log` for mod and game errors (each line names the mod).

### Making a map or event (OpenOmori)
1. `bun run mod map show --game 13` to see a real map as text (`#` blocked, `.` open, letters are events).
2. `bun run mod map new my-story 1001 "My Street" 34x12` writes a starter `maps/1001.spec.json`.
3. Stamp art from an existing map (`stamps`), mark collision (`blocked`), add events (`say`, `switch`, `transfer`,
   `common`, `plugin`, ...) and pages with conditions.
4. `bun run mod map build my-story`, then `bun run mod map show my-story 1001` to see the result.
5. `bun run mod check my-story` must pass. Playtest the room at `?hot=1`.

Map ids: the game uses 0–999. Ids from 1000 up are **shared by every mod in the same dev folder**, and another mod may already use some. Pick an id no other mod uses, and run `mod lint <mod> <other mod>`, which reports a duplicate id (error). Registration
uses the `set` op, so mods do not shift each other's entries (see openomori.md). A worked example, tested in game with a second mod in the same folder, uses 1300.

### Writing dialogue (either path)
1. Find the existing style first: `mod say` or `grep` the dump for a similar scene.
2. Write `languages/en/<mod>_<topic>.yml` (OpenOmori: `files/languages/en/`) with `message_N` keys, using
   **single quotes or `\\` in double quotes** for text codes (see `formats.md` section 2 on the YAML trap).
3. Reference lines from events with `ShowMessage <mod>_<topic>.message_N`.
4. Run `mod lint` (OpenOmori) and look at the warnings: unknown codes, missing faces, unresolved references.

### Writing choices (either path)
`AddChoice <file>.<key> <Label>` for each option, then `ShowChoices <N-1>` (N options; the count starts at 0).
Each option has a label, and a `Label` command jumps to it. Test every branch.

## Common to both paths

- **Patch, do not replace**, where you can. A replaced file conflicts with any other mod that replaces it, and
  does not survive a game update. [OpenOmori] [wiki]
- **Keep your mod's assets and the game's assets separate**: credit, and do not ship the game's files.
- **Playtest every route**, from a clean save, at 1x speed for timing. Use checkpoints (OpenOmori) to jump
  to a state and test a single scene.
- **Use the quality reference** (`quality.md`) before you call something finished.
