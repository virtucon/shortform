# Vertical frame rules

Canvas: **1080 × 1920**, 30fps, 9:16. Always. One render serves every platform.

## Safe zone

Platform UI (username, caption, buttons, search bar, progress bar) sits on top of the video. Anything the viewer must read or see goes inside the safe zone. Backgrounds, colour and decorative motion fill the full canvas edge to edge.

Universal safe zone (worst case across TikTok, Shorts and Reels):

```
top     250px   status bar, tabs, search
bottom  480px   username, caption, music ticker, progress bar
left     60px
right   160px   like / comment / share column
```

Safe rectangle: **x 60–920, y 250–1440** (860 × 1190). Its centre is at x 490, y 845 — left of and above the canvas centre. Centre text on the safe rectangle, not the canvas.

These margins are conservative approximations; platform UI changes and varies by device. When the brief names one platform, you may relax to:

| Platform | top | bottom | right |
|---|---|---|---|
| `tiktok` | 200 | 480 | 160 |
| `shorts` | 250 | 420 | 160 |
| `reels` | 250 | 450 | 140 |

Put the safe zone in the composition as CSS variables so every scene uses it:

```css
:root {
  --safe-top: 250px; --safe-bottom: 480px; --safe-left: 60px; --safe-right: 160px;
}
.safe {
  position: absolute;
  top: var(--safe-top); bottom: var(--safe-bottom);
  left: var(--safe-left); right: var(--safe-right);
}
```

## Type scale

Phones are small and viewers are moving. Go bigger than feels right on a desktop preview.

| Role | Size | Weight | Max per screen |
|---|---|---|---|
| Hook / headline | 110–160px | 800–900 | 8 words, 3 lines |
| Kinetic caption | 80–110px | 700–900 | 4 words, 2 lines |
| Supporting label | 48–64px | 600 | 1 line |
| Minimum for anything | 40px | — | — |

- At 110px a line in the 860px safe width holds about 14 characters. If the hook does not fit in 3 lines at 110px, shorten it, move part of the phrase into the scene's visual (an email subject, a label), or take the next-ranked hook. Never shrink the hook below 110px and never let a line break strand one short word or a closing quote.
- Line height 1.0–1.15 for headlines. Tight tracking on large display text.
- Contrast: text meets WCAG AA against whatever is behind it at every frame. Over busy visuals, use a solid pill, a heavy stroke or a scrim behind the text.
- Use the project's fonts (see Fonts in `compose.md`); otherwise one bold grotesque for everything. Weights in the table are targets; use the heaviest the family offers.

## Layout

- Stack vertically. Side-by-side columns do not survive 860px of width.
- Fill the safe rectangle. Each scene's content block is vertically centred in it and spans at least two thirds of its height. Content bunched at the top with an empty lower half is the most common failure; fix it by scaling the visual up, not by adding filler.
- The safe rectangle applies to the rendered bounding box. Rotated cards, negative margins, highlight pills, shadows and scale-up animations all push pixels outward; leave them room.
- One focal point per scene. If two things compete, make two scenes.
- Product UI: rebuild or crop to the part that matters and scale it up until its text is at least 40px. A full desktop screenshot shrunk to fit is unreadable; show a zoomed region, or pan across it.
- Landscape media sits in a rounded card or device frame within the safe rectangle, with a caption above or below it.
- Keep the brand mark small and persistent (a corner of the safe zone) rather than giving it a scene of its own.

## Motion

- Something changes every 2–3 seconds: cut, push, zoom, caption swap.
- Entrances are fast (0.2–0.4s) with a confident ease; then hold. Text never leaves before its read time is up.
- Vertical movement reads as natural on a phone. Prefer slide-up, push-up and scale over horizontal wipes.
- Cuts land on music beats when a beat grid is available. Story and read time win over beat sync when they conflict.

## Loop

The last frame must cut into the first without a jolt:

- The CTA scene uses the same background the hook scene starts on.
- The CTA stays settled and fully readable on the final frame. No exit animation, no fade to black, no end slate, no trailing silence.
- The hook's entrance motion is what makes the restart feel like a cut rather than a stop.

## Checklist (gate for Step 3)

- [ ] Root element is `data-width="1080" data-height="1920"`.
- [ ] Every readable element sits inside the safe rectangle, checked by looking at snapshots (`check` cannot see safe zones).
- [ ] Each scene's content is vertically centred in the safe rectangle and fills at least two thirds of its height.
- [ ] No text under 40px; hook at least 110px.
- [ ] Hook text is visible on frame 1.
- [ ] No gap longer than 3 seconds without a visual change.
- [ ] Every text element meets its read time.
- [ ] The video makes complete sense with the sound off.
- [ ] Final frame shows the settled CTA on the first frame's background; no fade to black.
