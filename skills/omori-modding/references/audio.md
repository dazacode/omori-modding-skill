# Audio direction for OMORI mods

Sound carries much of OMORI's mood, and critics praise its music for knowing when to be loud and when to be quiet
(Noisy Pixel, fetched). A mod's audio should be planned per beat, not added at the end.

Confidence: the silence and horror points come from a GDC 2005 talk by Akira Yamaoka (secondary coverage) and from
general audio guides. The implementation advice comes from general game-audio sources and is less established.

## Plan sound per beat
Every scene's blueprint (`director.md`) has a SOUND line. For each beat, decide:
- **Music.** A track, a change of track, or deliberately none.
- **Ambience.** Room tone or a background bed under quiet dialogue.
- **Silence.** Where silence is the choice (see below).
- **The sound on the peak.** One sound, placed at the peak, chosen for the feeling.

## Silence as a tool
- Yamaoka's GDC 2005 point: Silent Hill avoided music as an emotional cue for unease, and empty or formerly lived-in
  spaces were a reliable horror tool (Game Developer coverage).
- Silence before a scare raises anticipation; silence after intensity can unsettle more than sound (search results).
- **Test.** Play the scene with the music off. If it still works, the music is adding, not covering. If it works better
  silent, cut the music from that beat.

## Changing music
- **Change on emotional turns.** Silence before the big line; a theme on the release (the quality reference section 6).
- **Transitions on bar or beat boundaries.** Changes that land off the beat sound accidental. Use a stinger or a short
  bridge phrase to smooth a seam (sources agree; the specific values are not established).
- **Curved fades, not linear.** One audio guide recommends curved fades; test by ear.
- **Hold times.** Avoid rapid switching; a minimum hold keeps the music from thrashing (general guidance).
- **Do not switch the whole track when a layer would do.** Naive whole-track switching breaks immersion (search result).

## Loudness and mix
- Set each track's loudness consistently; a track that jumps out at a scene change breaks the scene.
- Keep dialogue clearly above the music and ambience, so the text and its sound are heard together.
- Check the mix at the player's settings: OpenOMORI keeps the game's four volumes (music, background sound, fanfares, effects) separate in its Audio tab (F10).

## Sources of sound
- **Base game first.** OMORI's audio in `audio/` is the reference for the game's mood and cadence.
- **Free licences.** CC0 and CC-BY files fill gaps. Every file goes in the credits file (author, URL, licence).
- **AI-generated audio.** Not used by default (`anti-slop.md`). If used, it is disclosed, and the human's edits are stated.
- **Missing files fail the build.** That is intentional: silence by accident is worse than a stopped build.

## Checks
- [ ] Each scene has a SOUND line in its blueprint.
- [ ] Silence is chosen and placed, not left by accident.
- [ ] Music changes land on beats and are not left running into a scene that needs quiet.
- [ ] Loop points have no audible seam.
- [ ] Music off test passed for every horror scene.
