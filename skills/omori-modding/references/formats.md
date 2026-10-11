# OMORI formats: files, dialogue, text codes, events, sprites

Each claim is tagged with how it was checked:
- **[dump]**: checked against the decrypted game in `dump/` (this install).
- **[wiki]**: from the community wiki (https://omori-modding.gitbook.io/wiki), not re-checked.
- **[MV]**: standard RPG Maker MV behaviour, from the MV docs or the MV source.
- **[OpenOmori]**: from the OpenOmori source or docs in `browser/`.

## 1. Where the game's files are

The game is RPG Maker MV (Steam build, NW.js). Data and dialogue are encrypted under `www/`. [wiki] [OpenOmori]

| Plain file in a mod | Encrypted game file it replaces | Notes |
|---|---|---|
| `data/Items.json` (any `data/*.json`) | `data/Items.KEL` | Map data is `data/Map<ID>.json` (3 digits, e.g. `Map001`). [OpenOmori] [dump] |
| `data/*.yml` | `data/*.PLUTO` | Quests, notes, some others. [OpenOmori] |
| `languages/en/<name>.yml` | `languages/en/<name>.HERO` | All dialogue. [OpenOmori] [dump] |
| `maps/map<ID>.json` | `maps/map<ID>.AUBREY` | Tiled-format tile layers. [OpenOmori] |
| `js/plugins/<name>.js` | `js/plugins/<name>.OMORI` | The game's own plugins. [OpenOmori] |
| `img/...`, `audio/...`, `movies/...`, `fonts/...` | same path | Served as-is. [OpenOmori] |

A mod never edits your install. It is layered on at load time (OpenOmori) or applied by OneLoader. [wiki] [OpenOmori]

### What a dump contains [dump]
After `mod dump` (OpenOmori) you get, under `dump/`: `data/` (Map*.json, CommonEvents.json, Troops.json, Items.json,
Skills.json, States.json, Enemies.json, Actors.json, Quests.yml, Notes.yml and more), `languages/en/` (208 dialogue
files; `00_template.yml` is the blank template), `js/plugins/` (every plugin's source), `js/plugins.js` (the
plugin parameters, including the message macros), `maps/` (tilesets and terrain). Images are not dumped.

The wiki's own OneLoader route produces a playtest folder instead: decrypt from the OneLoader **Mods** tab
(Decryption Mode "Everything", generate as RPG Maker project "Yes"); the output is `www_playtest_<id>` in the
game folder, opened in RPG Maker MV. [wiki]

## 2. Dialogue (YAML)

File: `languages/en/<name>.yml`. Keys are message names; the usual form is `message_N`. [wiki] [dump]

```yaml
message_0:
    text: "A white door casts a faint shadow.\\!<br>What would you like to do?"
message_2:
    faceset: MainCharacter_Basil     # image name without extension, case-sensitive
    faceindex: 0                     # position in the 106x106 grid, counted from 0 at the top left
    text: Dialogue goes here.
```

- **Keys** are text names used by `ShowMessage file.key` (`ShowMessage 01_map_whitespace.message_4`). Use
  letters, digits and underscores only. [dump] A key with a space cannot be referenced.
- **Fields** seen in the game's own files [dump]: `text`, `faceset`, `faceindex`, `background` (0 window, 1 dim,
  2 transparent), `position` and `positionType` (0 top, 1 middle, 2 bottom). The positions are confirmed in
  `YEP_MessageCore.js` (`positionType() === 0` / `2`). Background values follow the standard MV message window.
- **Speaker** is written in the text as `\n<NAME>` (name box) or with a macro (`\kel` expands to the name box for
  actor 3, see section 3). [dump]
- **Battle dialogue** uses a YAML block scalar with `\>NAME: \<` at the start of the line. [wiki]
- **Choices** are not stored in the YAML. They are events: `AddChoice file.key Label` then `ShowChoices N`
  (section 5). [wiki] [dump]
- **Quests** are in `data/Quests.yml` (the wiki says `.yaml`; the real file is `.yml`). Structure [dump]:
  ```yaml
  quests:
    Aubergine:
      world: 1
      name: { en: STUFFED TOY, jp: STUFFED TOY }
      text:
        0: ['XX_QUEST.message_26']   # quest state 0 -> dialogue reference
  ```

### The YAML trap (verified) [dump] [OpenOmori]
Inside a double-quoted YAML string a single backslash is an escape. `"\!"` is an error ("unknown escape
sequence"); `"\\!"` gives the text `\!`. Either write `\\` inside double quotes, or use single quotes or
no quotes (`'Hello.\!'`). The dump's own files use `"\\!"`. `mod lint` reports this with the fix. [OpenOmori]

### Shipping a dialogue file
- A **new** file name is a new language file. A file the game already has is replaced if you use the same name,
  so prefer `files/languages/en/<mod>_<topic>.yml` and reference it with `ShowMessage <mod>_<topic>.message_N`.
- A `ShowMessage` pointing at a key that does not exist: the runtime behaviour was not tested here, so
  treat it as a bug. Check references with `mod lint` (OpenOmori) or search the dump with `grep`.

Coverage in the dump [dump]: 13,567 message entries in 208 files. Of 25,573 `ShowMessage` references in the
maps, 25,452 resolve to an existing `file.key`; the 121 that do not resolve are rare leftovers in the game itself.

## 3. Text codes

A text code is a backslash in a message's text. Codes are case-insensitive for the word codes. [wiki] [dump]

**RPG Maker MV codes** [wiki] [MV]: `\V[n]` variable, `\N[n]` actor name, `\P[n]` party member, `\G` currency,
`\C[n]` colour (from the window skin), `\I[n]` icon, `\{` `\}` text size up and down, `\\` a backslash,
`\$` gold window, `\.` quarter-second wait, `\|` one-second wait, `\!` wait for a button press,
`\>` show the rest at once, `\<` cancel instant display, `\^` no wait after the line.

**TDS effects** [wiki] [dump]: `\SINV[n]` vertical wave (0 off, 1 normal, 2 dense), `\SINH[n]` horizontal shake
(0/1), `\QUAKE[n]` wobble (0 off, 1 intense, 2 soft), `\RAINBOW[n]` (0/1). The game uses these heavily
(`\QUAKE[1]` … `\QUAKE[0]`).

**Archeia** [wiki]: `\com[n]` runs common event n. The dump uses `\Com[2]` and `\com[2]` interchangeably.

**Speaker and name codes** [dump]: `\n<NAME>` (name box), `\n[n]`, and `\fn<NAME>` (seen in 17 dialogue files;
the plugin that defines it was not found, so its exact behaviour is unverified).

**Macros** [dump]: short names that expand to a speaker name or a variable. They are defined as plugin
parameters in `js/plugins.js` (`YEP_X_MessageMacros1`–`5`). Examples from this install: `\kel` → `\n<\n[3]>` (name box with actor 3),
`\mar` → `\n<MARI>`, `\bas` → `\n<BASIL>`, `\he` → `\v[11]`. Run `mod macros` for the full table. A macro can run
straight into text (`\kelHey` is `\kel` followed by "Hey"). The wiki documents only `\bas`.

**Plugin-defined codes** [dump]: `\DII[up]` (draw an input icon) is handled in `Omori BASE.js`
(`case 'DII':`). The lint reads these labels so it does not flag them.

**Colours** [wiki] [dump]: `\c[n]` selects a palette square in
`img/system/Windows.png`. The wiki's convention: 1 blue (skills), 3 green (food), 4 yellow (important items),
5 orange (toys), 11 light blue (locations), 13 purple (equipment and character names), 0 white. The dump uses
`\c[0]`, `\c[3]`, `\c[4]`, `\c[5]`, `\c[11]`, `\c[13]` in line with this, so treat it as a strong default.

## 4. Sprites and pictures (file names)

Character sheets are named with markers. [wiki] (the lint in `mod lint` implements the same rules):

| Marker | Where | Meaning |
|---|---|---|
| none | `name.png` | a standard sheet of 8 characters, 3x4 tiles each, 12x8 tiles total; cells usually 32x32 |
| `$` | prefix | one character; the sheet is divided by 3x4 |
| `!` | prefix | no default 6 px upward offset (doors, decorations) |
| `[SF]` | prefix | a single image, not a sheet (chandeliers, large holes) |
| `%(n)` | suffix | `n` frames across (running, fear enemies) |

Markers combine: `$FA_OMORI_RUN%(8).png` is one character running with 8 frames. [wiki] [dump]

Picture sizes [wiki]: game pictures 640x480 (most cutscene frames), battle animation frames 192x192,
portraits (faces) 106x106, tiles 32x32. Draw large, then downscale for pixel art.

## 5. Events (what the game runs)

Events are RPG Maker MV event lists: a list of commands, each with a numeric code. OMORI adds its own commands
through plugins; most dialogue goes through `356` (Plugin Command) and `355` (Script). [MV] [dump]

**The codes that matter** [MV] (the names in `mod commands` use this table):
`101` show text (its lines are `401`), `102` show choices (`402` when, `403` cancel, `404` end), `111` if
(`411` else, `412` end), `112` loop (`413` repeat), `113` break, `115` exit, `117` common event, `118` label,
`119` jump to label, `121` control switches, `122` control variables, `123` control self switch,
`201` transfer player, `205` set movement route (`505` each move), `230` wait, `108` comment (`408` lines),
`355` script (`655` continued lines), `356` plugin command, `250` play SE, `241` play BGM, `231` show picture.

**Counts in this install** [dump] (from `mod commands`): 505 (route moves) ~64k, 0 (list end) ~59k, 655 ~39k,
356 (plugin commands) ~35k, 230 ~21k, 108 ~19k, 205 ~17k, 111 and 412 13,433 each (every `if` has its end).
Plugin commands: `ShowMessage` 26,641; `AddChoice` 2,838; `ShowChoices` 1,316; `CAM` 835;
`exaFC.followerInstruct` 752; `CallEvent` 731; `AllowEventMovement` 238; `exaFC.clearAll` 231;
`StopEventMovement` 204; `Wait` 112; `SmartPath` 103; `exaFC.followerStop` 90; `HorrorEffect` 86; `Gacha` 56.

**ShowMessage** [wiki] [dump]: `ShowMessage file.key` shows one dialogue entry. The file is the `.yml` name without
the extension.

**Choices** [wiki] [dump]: `AddChoice file.key Label` adds a choice whose text is `file.key` and which jumps to
`Label` when picked. `ShowChoices N` shows the choices added so far; `N` starts at 0 (`ShowChoices 1` shows two).
Labels are RPG Maker "Label" (`118`) and "Jump to Label" (`119`) commands. `XX_GENERAL.message_4` is "YES" and
`message_5` is "NO" in the base game. [wiki]

**Camera** [wiki] [dump]: `CAM PLAYER [SPEED]`, `CAM EVENT <id> [SPEED]`, `CAM <x> <y> [SPEED]`, `CAM DISABLE`.
Speed defaults to 800; lower is faster. [wiki]

**Script calls** [dump]: pure JavaScript. Self switches are the common pattern:
`$gameSelfSwitches.setValue([mapId, eventId, 'A'], true)` (the letter in quotes, case-sensitive).
Only map events have self switches (not common events or troop events). [dump] [wiki]

**Shops** [wiki]: a choice with Buying, Selling and Cancel; Buying runs `this.setupShop('name', 0)` then a Shop
Processing command; Selling uses `1`. The dealer's dialogue is in `languages/en/System.yml` [dump] (the wiki says
`System.yaml`, which is wrong). "Purchase Only" does not work in OMORI. [wiki]

**Plot armor** [wiki]: a character who would die does not, once, when the game marks them as having plot
armour (the base game does this for OMORI). Per-character setup is in the troop page 7 and the plugin
`NEB_PlotArmorFix.js`. [wiki]

**Balloons** and bubbles: the wiki page is marked WIP; quest bubbles are shown with common events 47–50
(show new quest, ongoing, finished, erase), variable 16 holds the event id the bubble sits over. [wiki]

## 6. Battle data (notetags) [wiki]

Enemy, skill and state behaviour is set by notetags in the Notes fields and in the States tab. Most enemy
notetag explanations on the wiki are marked TBA; the ones documented are: `<Sideview Battler: filename>`,
`<Sideview Battler Frames: n>`, `<Sideview Battler Speed: n>`, `<Sideview Battler Size: w, h>`,
`<StatusYOffset: n>`, `<DamageOffsetY: n>`, `<FallOnDeath>`, and a `<AI Priority>` block of
`Condition: SKILL id, target` lines (checked in order; the wiki does not confirm the order).
Skill notetags (`<AntiFail>`, `<HideInMenu>`, `<ChainSkill: n, direction>`, `<EnergyCost: n>`, the
`<Setup/Whole/Target/Follow/Finish Action>` blocks) are documented on the wiki. The states page explains
Element Rate, State Resist and the Apply/Remove/React notetag groups. Treat all of these as wiki-level
knowledge; test in the game.

Map and event notetags come from Yanfly's plugins (hitbox resize, sprite offset, copy event, activation
radius). They are documented on the wiki and used in the base game (`<Sprite Offset: +0, +14>` is the
counter-hides-the-waist trick from the quality reference).

## 7. Maps

- A map is three things: tile layers (`maps/map<ID>.json`, Tiled format), RPG Maker map data with events
  (`data/Map<ID>.json`), and an entry in `data/MapInfos.json`. [OpenOmori] [wiki]
- Layer names matter for draw order: `GROUND`, `SAME AS CHARACTER`, `ABOVE ALL` (the wiki's names). The game's
  own names are `GROUND - I` … `ABOVE ALL - IV` [OpenOmori] [dump].
- Collision: a `COLLISION` layer (invisible in game) in Tiled, or `blocked` rows in OpenOmori's spec.
- Regions (wiki): `regionId` 10 slides, 20 blocks only enemies, 28 footprints, 90 toggles walk/climb,
  92 red flash and 10 damage to the party. [wiki]
- Tiled 1.0.3 is the version the wiki recommends (newer versions can break the terrain file). [wiki]
- OpenOmori compiles maps from a short spec and does not need Tiled. [OpenOmori]

## 8. Endings and credits [dump]

- Credits are videos, played with the game's video plugin: `ysp.VideoPlayer.loadVideo('<Name> End Credits Draft N.webm')`
  as a script command. The good-end credits start from Map 390 event 12 after the picture `the_truth_3`.
- The sounds over the endings are `credits_phone_ring` (a background track, command 241 Play BGM, file in `audio/bgm/`)
  and `credits_ambulance` (a background sound, command 245 Play BGS, file in `audio/bgs/`). Not sound effects.
- Replacing an ending means replacing or repointing these events; check every event that calls a credits video.
- What the endings show, and the open question of forgiveness, are in `post-canon.md`.

## 9. Save and route data [OpenOmori]

- Story progress is tracked by named switches, not a chapter variable. `mod routes` reports the spine, the
  phases and the endings from this install's events. Checkpoints (save snapshots) can be captured, diffed,
  restored and forged. See `openomori.md`.
