# Step 3: Compose

`/shortform` owns the story: objective, hook, storyboard, text, format, music choice. The hyperframes domain skills own the mechanics: composition structure, timeline wiring, animation technique, lint rules. When they disagree on mechanics, follow hyperframes. When they disagree on format or story, follow this skill.

## Set up

```bash
cd <out>
HYPERFRAMES_SKIP_SKILLS=1 npx hyperframes@0.8.46 init composition --non-interactive --resolution portrait
cd composition
```

`--resolution portrait` scaffolds the 1080×1920 canvas. `HYPERFRAMES_SKIP_SKILLS=1` stops `init` from linking skills into agent directories outside the output folder. Delete the `CLAUDE.md` and `AGENTS.md` that `init` writes into the composition; they route to the generic hyperframes workflow, which this skill replaces.

Confirm the scaffold's root element says `data-width="1080" data-height="1920"`, and that any `body` or stage size in its CSS is 1080×1920 too. Keep the scaffold's `data-composition-id` and its matching `window.__timelines` key as they are. Set the root's `data-duration` to the planned length.

The approval gate in Step 2 is the user's sign-off. It replaces any "preview and get approval before render" step in the hyperframes skills.

Add the safe-zone CSS variables from `vertical.md` before writing any scene.

## Fonts

HyperFrames lint wants fonts loaded from local files. If the project's font is a Google Font or ships in the project, put the `woff2` files in `assets/fonts/` with `@font-face` rules (downloading from Google Fonts is fine when the network allows). If you cannot get the file, use a system stack (`system-ui, -apple-system, "Segoe UI", sans-serif`) rather than stalling. Use the heaviest weight the family has when the type scale asks for more than it offers.

## Assets

Copy everything the composition uses into `composition/assets/` and reference it by relative path. Never point at files outside the composition directory; renders must not depend on where this skill is installed.

- Logo, icons and images from the project.
- User media named in the brief.
- The chosen music track, copied from this skill's `assets/music/` (resolve relative to the skill's own directory) or from the user's path.

## Music

```html
<audio id="music" data-timeline-role="music" src="assets/upbeat.mp3"
       data-start="0" data-duration="<video length>"
       data-track-index="10" data-volume="0.4"></audio>
```

- Volume 0.4 without voiceover, 0.13 under voiceover.
- The bundled tracks are 60s. Set `data-duration` to the video length. End the video on a beat so the loop does not cut mid-phrase.
- Beat grid: run `npx hyperframes@0.8.46 beats` in the composition directory. It writes `beats/<audio path>.json` as `{"beats": [{"time", "strength"}, …]}`. The raw grid is far too dense to be useful (several beats a second), so reduce it first: keep the strongest third of the beats by `strength`, then thin to roughly one every 1–2 seconds across the whole video. Snap scene changes to the nearest of those when the shift is under 0.3s; otherwise keep the planned time. If the command finds no beats or fails, carry on with planned times.

## Kinetic captions

The captions are the script from `plan.md`, one card at a time.

- Each card is its own clip with `data-start` and `data-duration` from the storyboard, on a track above the visuals.
- Reveal word by word or as a whole card; either way the full card is settled within 0.4s and then holds for its read time.
- Swap caption cards with a hard cut: the next card starts on the frame the last one ends, so the band is never empty mid-scene. Animate entrances only.
- Size text to fit its peak animated scale. A line that slams in at 1.05× needs to fit 860px at 1.05×.
- Place captions in a fixed band inside the safe rectangle so the eye does not hunt. Good default: vertical centre of the safe rectangle for text-led scenes, the upper third when product UI fills the middle.
- Style: heavy weight, tight leading, one accent colour for the emphasised word, and a pill, stroke or scrim whenever the background is not flat.
- The hook card exists at time 0 already settled or mid-slam. It must be legible on frame 1.

## Scenes

- Rebuild product UI in HTML and CSS from the real code, using the real strings, colours and fonts. Crop to the part that matters and scale up.
- Brief-only videos use typographic scenes, simple diagrams, numbers and shapes. Keep them concrete to the topic.
- Number the steps on screen for `educate` ("1/3").
- The CTA scene holds at least 2.5s and shows the ask and the handle, URL or repo name as text. It has no exit animation: it stays settled and readable through the very last frame, on the hook scene's background, so the loop back to frame 1 reads as one more cut.
- Give every clip an `id`, and spread caption cards over two or three tracks rather than one; this keeps `check` free of editable-id and track-density warnings.
- Full-bleed decoration that runs past the canvas edge (background shapes, glows) gets `data-layout-allow-overflow`, or `check` flags it on every sample.

## Voiceover (only when the brief asks)

Generate narration with `npx hyperframes@0.8.46 tts`. Run `npx hyperframes@0.8.46 tts --help` for its current flags, and use the `media-use` skill for voice choice if it is installed. Write the audio into `assets/`. Time caption cards to the narration, word for word, and drop the music volume to 0.13.

## Check

```bash
npx hyperframes@0.8.46 check
```

Fix every error, including contrast and overflow findings, and re-run until clean. Then walk the checklist at the end of `vertical.md`. `check` does not know about platform safe zones; that part is on you.

For a visual check, `npx hyperframes@0.8.46 snapshot` writes key frames as PNGs. Look at the first frame, one mid scene and the last frame: is the hook readable, is everything inside the safe rectangle, does the last frame match the first?
