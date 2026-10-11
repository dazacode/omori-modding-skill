# Post-canon and continuation mods

A post-canon mod picks up after the game's ending. Players love it when it is honest about the canon, and critics and
players punish it when it contradicts the canon or resolves what the game leaves open for no reason. This file is what
was verified about the canon endings, the community's major projects, and what those projects teach. It is written for
the OMORI-first use of this skill. The RPG Maker craft is in the other references.

Confidence labels: **[dump]** checked in this install's events and text, **[wiki]** from omori.wiki (not re-checked),
**[read]** a page that was opened and read, **[search]** a search result only, **[unverified]** claimed by someone but not
found.

## 1. The canon endings and what the game shows [dump] [wiki]
The events in the dump carry the endings. The credits are videos (`ysp.VideoPlayer.loadVideo('<name>.webm')`), and the
sounds around them are named in the events.

| Ending | What the game shows (wiki) | What the dump shows |
|---|---|---|
| Good | Sunny plays the Duet with Mari, absorbs Omori, wakes in reality and "seeks his friends and confesses the truth behind Mari's death". The page does not say what the friends do. | The good-end credits video is started from Map 390 event 12 ("END"), after the picture `the_truth_3`, an animation loop and a 373-frame wait. No dialogue sits directly before the credits in that event. |
| Secret Flower | Watering Basil's flowers on Two Days Left adds a cutscene after the good ending; Sunny and Basil smile at each other as their Somethings fade. | Not traced here. |
| Bad | Omori absorbs Sunny and returns to White Space; in reality Sunny jumps from the hospital balcony. | A bad-end credits video is in Map 390. |
| Suicide (True Route) | Sunny abandons Basil, takes a steak knife, and stabs himself in his sleep; a phone rings over the credits. | The neutral-credits video is in CommonEvents and Maps 13 and 14; the `credits_phone_ring` sound is in CommonEvents and Maps 14 and 511. |
| Suicide (Hikikomori) | Sunny stabs himself after Black Space or on Moving Day; the same phone ring over the credits. | Not traced here. |
| Abandon | Sunny leaves in his mother's car; sirens over the credits suggest Basil succeeded. | The `credits_ambulance` sound is in Maps 6, 13 and 80. Which event starts it was not traced. |

**What the game never shows** [wiki] [search]: whether the friends forgive Sunny. A Steam thread (2021, with later replies)
reaches the same answer: the game "never tells you if his friends forgave him", and the question is left to the player.

**The pasted claim that credits roll immediately after "I have to tell you something" is not supported.** The line does
not appear in the dump, and no source found says it. Do not write a mod that depends on it until it is checked in the game.

**What this means for a continuation:** the canon's open question (forgiveness) is the fan project's whole premise. A
continuation that answers it should say so as an interpretation; one that ignores it needs a reason in its blueprint.

## Quality benchmark (decided)
The quality benchmark is **OMORI's own canon**, because it is the work that critics reviewed: OpenCritic 86 from 11 critics
(100% recommending) and Metacritic 88 from 6, all positive [search]. A mod is judged by playing a named canon scene next to
its own scene (`quality.md`, verify step 4). No critically reviewed fan project was found, so no fan work is a quality
benchmark. Reverie is used below only as a reference for production practice and scope.

## 2. OMORI: Reverie (the most developed post-canon project; production reference only) [read] [search]
Treat Reverie as the benchmark for **production practice and scope**. Do not treat it as the benchmark for **critical
quality**: no critic reviews of it were found, and the claim that it is "critically acclaimed" was not verified.

**What it is** [read]: a fan mod by Stahl and a team of about 30 named contributors, set after the good ending. Chapter 4
(mods.one) opens new paths in the Neural Network and the "scrambled" Pyrefly Forest, adds Doughie, Sweetheart and Spaceboy
as Headspace party members, and runs 4 to 10 hours with optional content. Difficulty has three tiers (Easy, Normal ≈ base
game, Hard "harder than base game"). It ships in English, Spanish and Russian, with Chinese and Japanese each needing
extra steps. Its own page lists conflicts: other mods that change the same files, and saves from Chapters 1 and 2 that
cannot be updated.

**What it is not** [read] (the team's own devlog, January 2026): "NOT intended to be THE post-good ending mod"; "Reverie is
focused on battles". The original gimmick was "what if headspace bosses are party members", and the story was secondary.
The pasted description (Sunny moving to a new city, a quiet study of guilt) is not what Reverie is. That description
belongs to a different project, AFTER (see section 4).

**Production practices worth copying** [read] (devlogs, 2023–2026):
- **Tasks in man-hours.** Estimates are in hours, with a note that a 10-hour task may take 5 days at 2 hours a day.
- **Structure before work.** Work is set up so team members can start at once (July 2026).
- **Writing needs group sign-off, then a final meeting.** Discussion comes before work, because otherwise the work may go
  unused. Writing was formally approved in March 2026 with "there should be no more rewrites".
- **Playtesting after content, before balance.** Gameplay testing started in January 2025 ("simply playing through what we
  have so far"), and balancing came later.
- **Restructure with a log.** Every enemy now uses one data slot, not four; AI is set by emotion (Angry favours offence,
  Sad defence, Happy specials). Plans were cut and reduced, and each cut is dated in the devlogs.
- **Cut for pacing, not for ease.** Content was made optional or reduced when it dragged; Chapter 5 went from 8 long
  cutscenes to 5.
- **Public release record.** Versioned releases with dates and a changelog, a GitHub releases repository, an in-game guide
  book, and an asset licence (CC BY-NC-SA) that the devlogs say is more lenient than before (the change date is not stated).
- **Know the bottleneck.** The team named cutscene implementation as the main bottleneck, and pixel art as a bottleneck
  for Chapter 3.

**What the team itself criticised** [read] (its own words, so they are reliable as warnings):
- Focusing on side characters "has hurt its popularity"; major characters are absent in the early chapters.
- Some story elements "feel forced": the Dreamworld continuation and the Unbread Twins' place in the real-world section.
- Overt Shin Megami Tensei and Touhou references in Chapter 4 are "a valid concern", especially where they conflict with the
  theme.
- Slow development is "a valid criticism". Writing became "more like a puzzle to solve".
- Earlier chapters were balanced under an older philosophy, so bosses were changed indirectly.

**The lessons for a skill user** (a checklist, not a verdict on Reverie's quality):
1. Keep the mod's main character and their arc on screen early; side-character focus is a known cost.
2. Test every reference to another franchise against the theme before it ships.
3. Write each scene's purpose before writing its lines; Reverie's own puzzle-like writing shows what happens without one.
4. Plan the cutscene pipeline early; it was the bottleneck.
5. Publish known issues and the save-compatibility rules on the upload page.

## 3. OMORI: The Dreamer (a cancelled or paused project) [read] [search]
The Dreamer was an episodic, book-by-book project (five books planned, with books 4 and 5 as the endgame). The developer's
updates say it is no longer going ahead in that form: the lead's health declined, the team was small and unpaid, and the
plan became either a shorter, more surreal rewrite that needs outside help, or a polished PRELUDE with a changed ending.
The developer said "It's my mod, and so it ends with me" and will not hand off the project. Summary of the developer's
updates (dates inferred; years are not always shown): the team "is not in development hell"; scope "will be adjusted
according to what is possible"; the team "just isn't big enough, especially for tilesets and sprites".

The pasted claims about a PRELUDE playthrough (Hero leaving the room first, Aubrey conflicted) were not found anywhere.
The PRELUDE's final part exists as a video title ("Omori The Dreamer Prelude: Part 5 (Finale)"), but its content was not
checked.

**Lesson:** a continuation with a single developer and a multi-book plan is a schedule risk. Plan a release you can finish,
and say so on the page.

## 4. OMORI: Afterwards and AFTER (two different projects) [read] [search]
- **OMORI: Afterwards** (mods.one, by ITAdude): a dialogue-heavy adaptation of a fanfic by AestheticallyBlue. The author
  describes it as "mostly dialogues about the characters' feelings than gameplay." It starts when Sunny wakes in the hospital
  and ends as he leaves for the city in his mother's car. It took about two weeks to make, is credited as the author's
  first mod, and has versions 0.1 and 1.1 (June 2024). The writer is credited for "literally writing the entire story".
  Note: the mods.one page calls the source a fanfic with a similar name, so the name overlaps the name of another project (see section 5).
- **AFTER** (itch.io, by digichiaki): an unofficial AU. Sunny moves to a town called "Close To City". It is not canon and does
  not pick up from the hospital.

## 5. What a similar name means for your mod [read]
- A fanfic adaptation shares its name with other projects in the same space. Two projects with similar names risk confusing
  players; the title and the credits should state which fanfic each one follows.
- Afterwards is the closest direct comparison: the same canon gap, a dialogue-first approach, and a short build time. Its
  reception was not found.
- Check any fanfic adaptation's licence and permission with its author before publishing, and credit the author in the
  credits file (`anti-slop.md` gate on credits).

## 6. Rules for a continuation mod (from the above)
1. Start from the canon's last state, and quote the canon's wording only where it is short and exact.
2. Say which open question you are answering (forgiveness, what the friends think, what happens to Sunny after) and that
   the answer is your interpretation. Canon left the question open for a reason.
3. Keep each character's grief and anger distinct; name what each one wants from Sunny in the blueprint.
4. Do not reveal what Omori is, or who Basil is, unless the game already shows it.
5. Publish the scope and the release plan honestly. Do not promise a multi-book project you cannot finish; Reverie's team
   said "scope will be adjusted according to what is possible" and shipped in chapters, which is the honest model.
