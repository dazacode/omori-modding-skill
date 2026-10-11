# omori-modding (an agent skill)

A skill for making OMORI mods: the file formats, dialogue and text codes, events, sprites and art without an image model,
scene direction, playtesting, and the checks that keep a mod from becoming slop. It works with the traditional RPG Maker MV
route (OneLoader, OneMaker, Tiled) and with [OpenOmori](https://github.com/dazacode/openomori), which is recommended and optional.

## Install

```
npx skills add dazacode/omori-modding-skill
```

Options from the skills CLI, for example `--list` to see the skill first, or `-g` for a global install.

## What it contains

- `skills/omori-modding/SKILL.md`: the entry point and the director's rules. Read first.
- `skills/omori-modding/references/`: one file per topic (formats, workflows, OpenOmori, quality, asset sourcing, art,
  audio, writing, horror, level design, playtesting, post-canon mods, anti-slop) and `sources.md`, which lists every
  source with how it was checked (read, search result, or unconfirmed).

## What it does not contain

- No game files, no decrypted game data, no images, no sounds, no keys. Those come from your own copy of OMORI.
- No mod. The skill describes how to make one.

## Requirements

- A legitimate copy of OMORI for PC (the folder with `OMORI.exe` and `www/`). The tools read it; they never write to it.
- For the OpenOmori route: [Bun](https://bun.sh) and a clone of OpenOmori.

## Notes

- The quality bar and the benchmarks are opinions, sourced in `references/quality.md` and `sources.md`. Check the sources before you rely on them.
- Licence terms for free assets are checked from each asset's own page where it was possible. Read the licence file in each download before you ship.
- OMORI, its art and music belong to OMOCAT. OpenOmori is an unofficial fan project and is not affiliated with OMOCAT.

## Licence

MIT. See `LICENSE`.
