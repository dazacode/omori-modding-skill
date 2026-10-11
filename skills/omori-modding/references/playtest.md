# Playtesting: how a director learns what works

A playtest exists so the director learns what works, not so the tester has fun (Eric Zimmerman's playtesting primer,
search result). The methods below come from design-school lectures and playtesting writing; they are general, not
OMORI-specific.

## Before the session
- **Write down what you want to notice.** Pre-set categories: where the player seemed confused, what made them laugh,
  how many tries before they quit, where they looked away or put the game down (Zimmerman's advice).
- **Pick the test.** A section test (one scene, many runs, to catch broken logic and timing); a full run (the mod from a
  clean save, to catch pacing and the mid-game drag); a first-hour test (the start, to catch the setup stumble).
- **Use a clean save and a fresh install or profile.** Your own saves carry knowledge you do not have.

## During the session
- **Stay hands-off.** Do not help. A player's mistake is the most useful data (Zimmerman). If you must step in, log it.
- **Think aloud, if they will.** Ask the player to say what they are thinking. Note that it can change the play, since
  talking is unnatural for some players (Cornell lecture, search result).
- **Watch the body, not only the words.** Players can say they were fine while leaning in, looking away or checking
  a phone (playtesting guide, search result).
- **Record.** Video and audio catch what you missed. Game-state logs (flags, the map, the time) show what happened.

## After the session: the problem log
For every problem, record:
1. **The problem**, in one sentence (what the player could not do, or did not feel).
2. **The observations** that show it (the time, the line, the clip).
3. **Importance**: does it block, confuse, bore, or only annoy?
4. **Ease of fix**: a line, a sound, a map change, a scene cut.

Fix the blockers first, then the confusion, then the boredom. A problem that is only annoying can wait.

## What to check for each goal
- **Horror scene:** Did the player feel the GOAL? Did the music-off run still work? Did they explain it to themselves?
- **Mid-game:** Where did attention drop? Where did they say "is this it?" Compare to the scene's TIME plan.
- **First hour:** Where did they stall? Did they grasp the loop without help? Where did they first quit?
- **Choices:** Did they notice the choice mattered? Did every branch feel different?
- **Map:** Did they find the way forward? Did they walk past the exit?

## Repeating it
- Play the mod again after each batch of fixes. A fix can break a scene it did not touch.
- Use checkpoints (OpenOmori) to jump to the scene under test, instead of replaying from the start.
- Give a playtester the first hour only after you have fixed the blockers from your own run. Testing a known-broken start
  wastes their time and yours.
- Keep the problem log with the mod. Note which fixes were made and which were not, and why.

## Limits
- Opinions from one or two testers are signals, not proof. Look for the same problem in two runs before you act on it.
- Horror works on the subconscious and is hard to describe from feedback (Frictional's view). Watch behaviour more than
  answers.
