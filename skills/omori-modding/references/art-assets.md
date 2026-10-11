# Art assets for OMORI mods, without an image model

The wiki's art section is nearly empty: its Intro page is "TBA", and the only worked example is a Headspace portrait (see
section 6). This file fills the gap with what can be done by hand, by recolouring the game's own art, and by drawing
in code. None of these methods needs a generative image model, and the skill's default is not to use one (see
`anti-slop.md`). Where a free library is used instead, the order and the licence checks are in
`asset-sourcing.md`. Sizes are **measured** from this install [dump-tool] unless marked otherwise.

Confidence labels: **[measured]** read from the game's own images with `mod art sizes`; **[wiki]** the community wiki,
not re-checked; **[read]** a page that was opened; **[search]** a search result only; **[code]** a method a short script can do, written from the method rather than from any project's files.

## 1. Measured sizes of the game's images [measured]

| Folder | What it holds | Measured size and cell size |
|---|---|---|
| `img/characters` | field sprites | standard 8-character sheet **384×256**, 12×8 cells of **32×32** (249 of 780 sheets). A single-character sheet with `$` is 96×128 (3×4 cells). A running sheet `$…RUN%(8)` is 256×128: 8 frames across, 4 rows. |
| `img/faces` | portraits | sheets of **106×106** cells, 3 columns (e.g. 318×1166 = 3 × 11 cells). |
| `img/pictures` | scenes and cutscene frames | **640×480** is the commonest size (52 of 200 sampled), and 1920×1440 and 1920×480 also appear. |
| `img/animations` | battle animation frames | **192×192** cells (960×384 = 5 × 2 cells). |
| `img/tilesets` | map tiles | **32×32** tiles (a 1024×1024 sheet is 32 × 32 tiles). |

Two things the wiki's table says are confirmed here: portraits are 106 px cells, and tiles are 32 px. The standard
character sheet is 384×256, not the 576×560 or 48 px cells that a forum answer gives for RPG Maker in general [search];
OMORI does not use those numbers.

**Use `mod art sizes <folder>`** to check any folder, and `mod art decode <folder/name>` to write one image as a PNG to
`./art-out` (git-ignored) so it can be opened in an editor. The decoded image is game art: do not commit it.

## 2. Making a sprite sheet by hand [wiki] [read] [search]
- **Character sheet** [search, consistent with the measurement]: 3 columns × 4 rows per character (frames across, then
  the four directions down/left/right/up). Eight characters make up the sheet: two rows of four blocks. The decoded
  sheet in section 1 shows exactly this.
- **Frame timing** matters more than frame count (see `sprites-and-animation.md`).
- **Name the file with the markers** (`formats.md` section 4): `$` for one character, `!` for no offset, `[SF]` for a
  single image, `%(n)` for n frames across. `mod lint` checks the names and that the width divides by the frame count.

## 3. The human workflow the wiki describes [wiki] [read]
OMOCAT draws most sprites in pencil, scans the line art, and colours it digitally; the wiki calls this "time consuming
and roundabout" (line art page, [read]). The alternatives the wiki lists:
1. **Draw large, then downscale.** The Headspace walkthrough draws at **512×512**, then downscales to **106×106** with the
   "average colors" interpolation, then sharpens to restore the line [read].
2. **Match the line width** by resizing a reference to 512 and comparing it with the brush [read].
3. **Pencil brush around 6 px, texture at 8% opacity** for the line work [read].
4. **Colour with tone curves** (RGB, Red, Blue, Green curves) to pull the shadows and highlights toward OMORI's cooler,
   contrasting look [read]. The wiki's list of colour tools is: colour balance (three tonal values), tone curve, and gradient
   map (monochrome images) [read].
5. **Split the head.** A faceless head template with face variants on top saves time on many expressions. Basil's
   console battle portraits use this [read].
6. **Mask the result** to the character's outline, so the transparent area stays transparent.

Software: the wiki recommends **Medibang** (free, with paid extras; simple, few filters) and lists Krita, Clip Studio
Paint, Photoshop and Aseprite without details (its "TBA" page) [read]. Search results for 2026 compare LibreSprite
(free fork of Aseprite, slow maintenance, no tilemaps), Piskel (free, browser, basic animation), Pixelorama (free, MIT, on
Godot, actively developed) and Krita (free painting app with an animation timeline) [search]. Vendor pages rank these; the
choice is the director's.

## 4. Recolouring and reusing the game's art [code] [search]
This is the strongest non-generative route, because the new art already fits the game's palette.
- **Cut and recolour** the base sheets: take an existing townswoman's sprite, recolour the outfit (for example to scrubs),
  and reuse an existing sprite where one fits rather than drawing a new one.
- **Cut and recompose.** Cut frames from the game's own images and draw missing poses in OMORI's palette, pixel by pixel in code.
- **Palette swaps.** Store the pixels as indices and swap the palette, rather than keeping one copy per colour scheme.
  This is the standard for sprite variants [search]. For a full-colour PNG, the same effect needs a lookup at render time
  or a copy per variant (also [search]). Use a copy per variant only for a small project.
- **Licence (decided: basic mod standard).** The base game's art belongs to OMOCAT. Edited base art may ship in a mod under
  the basic mod standard: credit the source image and its author (OMOCAT) in `CREDITS.md`; ship only the edited derivative,
  never copies of the raw game files; state what changed; and follow the upload default the wiki names (CC BY-NC-ND 4.0).
  No source found says what OMOCAT allows beyond that, so a mod that sells, or redistributes game art unchanged, is outside
  this standard [read] [unverified].

## 5. Drawing in code (procedural, rules-based) [code] [search]
Code is a legitimate way to make art when the rules are the point: props, architecture, tiles, repeating shapes.
- **Shared rules first.** A shared rules module sets the palette, the 3/4 view (front face dominant, a 3–6 px top,
  2–3 px side) and the feet-on-the-floor anchoring. Every prop follows them.
- **Hand-authored parts, combined by code.** A TT-Forums thread on procedural vehicles describes stitching predefined
  pixel blocks and notes that each part needs shading for every facing [search]. The same holds for OMORI's 3/4 view.
- **Pillow (PIL) is a good tool** for this. Run scripts with `python -I` inside a virtual environment.
- **Generated patterns are a signal, not a shortcut.** Random mirrored sprites [search] look like slop when they are
  shipped without a human pass. Use code for placement and repetition, and a human for the shapes.

## 6. Dithering and shading [search]
- **Dither to stretch a small palette.** Two colours placed in a pattern read as a third (search: Mega Cat Studios, Divoom).
  Denser dots read darker or lighter with the hue.
- **Ordered (Bayer) dithering** uses a matrix (2×2, 4×4, 8×8) and gives clean multi-step gradients; it can show a
  crosshatch at very small sizes. **Random dithering** looks organic but can read as noise. Pick one per surface and keep it.
- **OMORI uses dither** in battle backgrounds, and the wiki's resources page links a dither tool (ditherit.com) [wiki].
- Hue-shifted shading (cooler shadows, warmer lights) was searched for and not found in the results. Test it by eye.

## 7. Animated pictures and frame grids [wiki] [dump]
The engine plays a picture as a frame grid. The script calls (verified in the dump: `setupPictureCustomFrames` is used
about 250 times in the maps):
- `this.setupPictureCustomFrames(id, width, height, hframes, vframes)`: width and height are the **whole image**.
- `this.setPictureFrameIndex(id, frame)`: a single frame, counted from 0 at the top left.
- `this.setPictureAnimation(id, frames, delay, loops, wait)`: `delay` is the frames between changes; `loops` 0 is infinite.
- `this.removePictureCustomFrames()` when finished. Higher picture ids draw above lower ones [wiki].

## 8. Maps and tiles [code] [wiki]
- **Tiles** are 32 px; the map compiler (`mod map`) can paint your own 32 px tiles from `files/img/tilesets/` (OpenOmori).
- **Stamp** existing map areas rather than painting from nothing (`mod map` stamps; `formats.md` section 7).
- **Collision** is a footprint, not the picture (`quality.md` section 7).

## 9. A checklist for any new art
- [ ] Right size for its folder (section 1), checked with `mod lint` for sheets.
- [ ] Right frame layout: 3 frames × 4 directions per character; `%(n)` matches the width.
- [ ] Palette and outline weight checked next to three base-game images at the same zoom.
- [ ] Made by hand, or edited from base art with the edit recorded; or an AI part disclosed (`anti-slop.md`).
- [ ] Credited in `CREDITS.md`: author, source image, licence, and what changed.
- [ ] Looks right in the game at its real size, not only zoomed in.

## Gaps this file could not fill
- The wiki gives no palette values beyond the colour-index convention in `formats.md`; read `img/system/Windows.png` in the
  game to get them.
- No source gives the frame timing that OMORI uses; measure it from the animation events in the dump.
- No source gives OMOCAT's rules for derivative art; ask the rights holder or keep edits to what the licence allows.
