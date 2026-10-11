# OpenOmori: the recommended toolchain

OpenOmori runs the player's own OMORI copy in a browser and adds a mod loader, a map and event compiler, checkpoints
and route tools. Source: https://github.com/dazacode/openomori (MIT). Its docs are in `docs/`
(MODDING.md, STORY-MODEL.md, ARCHITECTURE.md, TROUBLESHOOTING.md) and its types in `browser/types/omori-mod.d.ts`.
Read those for anything not here; this file is the map. [OpenOmori]

## Install and run
- `bun install`, then `bun run start`. It asks for the OMORI folder once (or set `OMORI_DIR`, or name the folder
  `OMORI…` next to the project). Open http://127.0.0.1:8080/.
- Keys in the game: F10 settings, F9 debug panel (teleport, switches, variables, checkpoints), F8 mods and profiles,
  F7 screenshot.
- Saves go to the browser's storage, never to the game folder. Profiles keep saves separate per mod set.
- Tests: `bun run test` (needs Chrome and the install); unit tests `bun test src`; types `bun run typecheck`.

## The CLI (`bun run mod <command>`)

| Command | What it does |
|---|---|
| `dump [dir]` | decrypt the game to readable files (default `./dump`, git-ignored) |
| `find <text>` | search the dump; prints file:line |
| `say <file.key>` | print one dialogue entry from the dump, with its face |
| `say <text>` | search the dialogue text (first 25 hits) |
| `macros` | list the message macros (`\kel` …) from the game's plugin parameters |
| `commands [n]` | plugin commands, script-call heads and event codes the game uses, with counts |
| `new <id>` | scaffold `mods/<id>` (mod.json, scripts/main.js, README) |
| `check [path...]` | validate mods (default `./mods` and `./examples`) and dry-run every edit |
| `lint [path or name...]` | check mod dialogue, text codes, faces, sprite names, ShowMessage references |
| `pack <dir> [out]` | zip a mod folder |
| `map show|new|build|...` | the map and event compiler (see below) |
| `routes [out.md\|--json]` | report the story's flags, phases, endings and route counters |

`lint` also reports style warnings (`style:`): words and phrases that machine-written text over-uses (delve, tapestry,
"it is important to note"...). They are a prompt to read the line again, not proof; see `anti-slop.md`.
`lint` results: `✗` error (fails the command), `!` warning, `·` note. Warnings are usually worth reading: an
unknown text code, a missing portrait, or a `ShowMessage` that matches no dialogue key. `lint` needs the dump for
the game's own dialogue and macros; without it the game-side checks are skipped and it says so.

## Mod layout

```
my-mod/
  mod.json          id (a-z 0-9 . _ -), name, version, optional author/description/dependencies/loadAfter/scripts
  files/            full replacements, mirroring the game's www/ (data/, languages/en/, maps/, img/, audio/)
  patches/          edits: <path>.patch.json (JSON Patch RFC 6902) or .merge.json (RFC 7386); JS targets use find/replace
  scripts/          browser JavaScript; run at a boot phase (early, pre-plugins, post-plugins, ready)
```

- Write plain files; the loader decrypts, edits and re-encrypts. The mapping table is in `formats.md` section 1.
- Start a JSON patch with a `test` op on the value you change. A game update then fails loudly, not silently.
- Unknown keys in `mod.json` produce a warning, so typos are caught.
- Two full replacements of one file conflict (the later wins, a warning is logged). Prefer patches.
- **MapInfos above 999 (fixed).** A map registers in `data/MapInfos.json` with the `set` op, which assigns the slot for its id
  and pads with null if needed. It never inserts, so mods can register maps in any order without shifting each other.
  The earlier `add` form did shift, and two mods using it corrupted the registry; that was the bug this fixed. `mod lint`
  reports the same map id in two mods (error), and flags remaining `add` registrations (error for two mods, warning for one).
- A failed patch is skipped and logged; the rest of the mod still loads.

## Scripts and the `OmoriMod` API (summary)

All mods' scripts share one global scope. Wrap yours in an IIFE.

```js
(() => {
const mod = OmoriMod.scope();                              // logs and errors are tagged with your mod
mod.on('map:setup', mapId => { /* ... */ });               // also scene:goto, game:new, game:load, game:save
mod.hook(Game_Party.prototype, 'gainGold', function (orig, amount) { return orig(amount * 2); });
mod.command('mymod.heal', (args, interp) => { /* a plugin command you define */ });
const power = mod.settings.add({ id: 'power', type: 'slider', label: 'Power', min: 0, max: 10, default: 5 });
power.get();                                               // types: slider, toggle, choice, action
})();
```

Errors inside `mod.*` calls are reported against the mod, and the game carries on. Hooks that throw run the
original and switch off after 10 errors. Debug with `$gameVariables`, `$gameSwitches`, `$dataMap` in DevTools.
Full types: `browser/types/omori-mod.d.ts`.

## Maps and events without RPG Maker

Spec file: `mods/<mod>/maps/<id>.spec.json`. Build: `bun run mod map build <mod>`. Show: `bun run mod map show
<mod> <id>` (ASCII) or `mod map show --game <id>` (a real map, with a coordinate ruler).

```json
{
  "id": 1001, "name": "MY STREET", "size": [34, 12], "tilesFrom": 13,
  "stamps": [{ "from": 13, "rect": [3, 9, 34, 10], "at": [0, 1] }],
  "blocked": ["#.#"],
  "events": [
    { "name": "Guide", "x": 12, "y": 6, "sprite": "FA_KEL", "pages": [
      { "do": [ { "say": ["Hello!", "Talk to me again."] }, { "self": "A" } ] },
      { "if": { "self": "A" }, "do": [ { "say": "Welcome back." } ] } ] },
    { "name": "Exit", "x": 32, "y": 6, "trigger": "touch", "do": [ { "transfer": { "map": 13, "x": 11, "y": 10 } } ] }
  ]
}
```

- **Art**: `stamps` copy a rectangle (all layers and collision) from an existing map; `paint` places your own
  tiles from a PNG of 32x32 tiles at `files/img/tilesets/`. Layer names are the game's (`GROUND - I` …
  `ABOVE ALL - IV`).
- **Collision**: `blocked` rows (`#` blocked, `.` open) on top of the stamped tiles.
- **Editing a game map**: `"base": <id>` with the same `id` changes it in place; a new `id` clones it.
  Replacing a game map conflicts with another mod that edits the same map (the build warns).
- **Triggers**: `action` (default), `touch`, `event-touch`, `auto`, `parallel`. Priorities `below`, `same`, `above`.
- **Page conditions**: `switch`, `self`, `item`, `variable: [id, ">=", n]`. Anything else: an `if`/`then` command.
- **Commands** (`do`): `say`, `switch`, `variable` (set/add/sub), `self`, `gold`, `item`, `transfer`, `wait`, `se`,
  `bgm`, `common`, `plugin` (any plugin command, e.g. `"CallEvent 2, Page 2"`), `script`, `comment`, `fade`,
  `erase`, `if`/`then`/`else`. `say` text is written to `languages/en/<mod>_map<ID>.yml`.
- **Sprites**: a game sprite name (`FA_KEL`) or your own PNG in `files/img/characters/NAME.png`.
- The build warns about touch events on blocked tiles, unknown sprites, transfers to missing maps, unknown
  common events; it fails for bad ids, sizes, stamp rectangles, events outside the map.

## Checkpoints and routes

- Checkpoint = a snapshot of the running game (about 30 KB gzipped). Capture, list, restore (an automatic undo is
  taken first), diff, export or import, from the F9 panel or with `OmoriMod.checkpoints.capture/list/restore/
  diff/remove`.
- *Story beats* (Settings > Gameplay, off by default) record a checkpoint the first time each story switch is set.
- Ship checkpoints in a mod with the panel's *script* button: `OmoriMod.checkpoints.register(name, data)`.
- `compare(a, b)` gives the flag differences between two checkpoints. `forge(base, patch, name)` builds a checkpoint
  from a flag patch, so a route can be recreated without replaying it.
- Story model: `docs/STORY-MODEL.md` (the flags are named by the developers; the route tooling uses them).

## Compatibility
OneLoader and GOMORI v1 mods install through the same panel. Partly supported: the legacy delta formats and
`exec` run-at modes are skipped with a warning. Not yet tested against mods from the wild. [OpenOmori]

## Notes for agents (from OpenOmori's own guide)
- Do not read the game folder directly: `mod dump` once, then read `./dump`.
- Prefer `patches/` with a leading `test` op over `files/` replacements.
- Loop: edit, `mod check <mod>` must exit 0, open `?hot=1`, read `latest.log`.
- For new maps and dialogue, write a spec and build; use `mod map show` to read a map as text.
- Never edit the install (`OMORI.Build.*`); it is read-only by design.

## Art commands (added in the art round)
- `bun run mod art sizes <folder>`: real sizes of the game's images in `img/<folder>` (characters, faces, pictures,
  tilesets, animations ...), from the player's own install. Measured values are in `art-assets.md`.
- `bun run mod art key`: prints the image key read from your own install, for the project's art scripts. Nothing is stored.
- `bun run mod art decode <folder/name> [outDir]`: writes one game image as a PNG to `./art-out` (git-ignored). It is game
  art: open it to read or to trace, and never commit it.

## Modding risk, and what OpenOmori does about it
OMORI's data is structured (JSON and YAML under `data/`, `languages/`, `maps/`), and the whole game can be modded. The
risk is in how far a change reaches, not in whether it is possible. OpenOmori's guards, by tier:

| Tier | Examples | Guards |
|---|---|---|
| Low | dialogue entries, new sprites, new maps with an id no other mod uses (1000 and up are shared, see workflows.md) | the map compiler refuses bad ids, sizes and stamp rectangles; `mod lint` checks dialogue and sprite names; a broken mod does not stop the game |
| Medium | JSON patches to existing data, a changed event page, a plugin command | every patch is dry-run against the real game by `mod check`; a `test` op makes a changed game file fail loudly instead of silently; a failed patch is skipped and logged |
| High | replacing a whole game file, editing a game map in place (`"base"`), hooking core game classes, changing flags the story depends on | two full replacements of one file conflict and the build warns; a map edited by two mods conflicts; hooks that throw run the original and switch themselves off after 10 errors; `mod routes` shows the story flags a change touches |

Every tier is protected by the same habits, which the agent should follow without being asked:
1. **Work on a copy, never the install.** The server serves the install read-only; `OMORI.Build.*` is never edited.
2. **Use a profile.** F8 profiles keep saves apart, and switching makes an automatic backup first. Test risky changes in a
   new profile, not the original game.
3. **Checkpoint before a risky change, restore to undo it.** F9 checkpoints (or `OmoriMod.checkpoints.capture`) take a
   snapshot of the whole game; a restore first makes its own undo checkpoint.
4. **`mod check` must exit 0** before a playtest. Warnings are read, not ignored.
5. **Watch `latest.log`.** Every mod and game error is written there with the mod's name.
6. **Start the game with `?mods=0`** to rule out a mod when something breaks, and `?hot=1` while developing.
7. **Never ship save files, game files or decoded art.** `dump/`, `art-out/` and `.omori-dir` are git-ignored for this reason.

When a change is in the high tier, the blueprint in `director.md` should name the flags, maps and files it touches, and
`mod routes` or a checkpoint diff should confirm nothing else moved.
