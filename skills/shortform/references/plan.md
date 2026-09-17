# Step 2: Plan

Write `<out>/plan.md`. It is the contract for the composition; someone should be able to build the video from it without seeing the source.

## Decide

1. **Archetype** from `archetypes.md`, with one line on why.
2. **Length.** Default 20–30s. Use the brief's number if given, capped at 60s. If the material is thin, go shorter. Platform nudges when named: `tiktok` and `reels` 15–30s, `shorts` up to 60s if the content earns it.
3. **Three hooks**, ranked, from three different patterns.
4. **The one real thing** the video shows (screen, flow, number, image).
5. **CTA** wording and the on-screen handle, URL or repo name.
6. **Tone** in three words, plus the palette and fonts taken from the source.
7. **Music.** Pick by tone from `assets/music/`:

   | Track | Fits |
   |---|---|
   | `upbeat.mp3` | launches, customer and signup videos, default |
   | `chill.mp3` | educate, calm or premium products |
   | `cinematic.mp3` | big reveals, social proof, hiring |
   | `playful.mp3` | funny, casual, consumer, community |

   If the brief gives a file path, use that file. If the brief says music off, use none. Exact beat times are detected in Step 3; in the plan, mark which scene changes should land on a beat.

## The script is the captions

There is no voiceover by default, so the script is the on-screen text. Write it as the viewer will read it:

- Short lines, 2–4 words per caption card. Break on natural phrases.
- Plain spoken words. Second person. Present tense.
- One emphasised word per card at most (colour or scale), and it should be the word that carries meaning.
- Total word count: about 2.5 words per second of video is the ceiling. A 25s video tops out near 60 words. Fewer is better.

If the brief asks for voiceover, write narration first and derive captions from it, word for word.

## Storyboard

One row per scene. Durations must sum to the target length.

```markdown
| # | Time | Dur | Beat | On-screen text | Visual | Motion / transition | Cut on |
|---|------|-----|------|----------------|--------|---------------------|--------|
| 1 | 0.0  | 2.5 | Hook | Still chasing / invoices by email? | Inbox pile-up, brand red bg | Text slams in on frame 1, inbox items stack | beat 1 |
| 2 | 2.5  | 3.5 | Agitate | 30 days late. / Every time. | Calendar pages flipping | Push up | beat 5 |
```

Every row needs real text and a concrete visual. "Feature highlight" is not a visual.

## plan.md layout

```markdown
# Shortform plan: <subject>

- Objective: <user's words> → archetype `<name>`
- Audience:
- Platform: universal | tiktok | shorts | reels
- Length: <n>s
- Tone:
- Palette / fonts:
- Music: <track or off>
- CTA:
- Assumptions: <anything defaulted because the brief did not say>

## Hooks
1. "<hook>" — <pattern> — <why it ranks first>
2. …
3. …

## Storyboard
<table>

## Claims check
| On screen (exact string) | Source |
|---|---|
| "6 hours a week" | README.md:12 |
| "€9/mo" | pricing.tsx:40 |
| "invoices paid 30 days late" | the brief |
```

Every number, price, date, name, quote and comparative that will appear on screen gets a row, quoted exactly as it will be rendered, with the file and line or the brief it came from. Step 4 gates on this table: anything on screen that is not in it gets cut. If a claim would make the video better and no source supports it, it does not go in — not softened, not hedged, cut.

## Approval message

When the gate applies, show only what the user needs to decide:

```
Objective: <one line>
Length: <n>s · Music: <track> · CTA: "<cta>"

Hooks — pick one:
1. …
2. …
3. …

Storyboard:
<table with #, Time, On-screen text, Visual>

Reply with a hook number, or tell me what to change.
```

One round. Apply the changes they ask for, update `plan.md`, and move on without asking again.
