# Step 4: Render and deliver

## Render

From `<out>/composition/`:

```bash
npx hyperframes render --quality delivery --fps 30 --output ../shortform.mp4
```

Use `--quality draft` while iterating. The final file is always `--quality delivery` (older CLI versions call it `high`).

## Verify

```bash
ffprobe -v error -select_streams v:0 \
  -show_entries stream=width,height,r_frame_rate:format=duration \
  -of default=noprint_wrappers=1 ../shortform.mp4
```

Must report `width=1080`, `height=1920`, and a duration within 1s of the plan and at most 60s. If not, fix the composition and render again. All three platforms accept H.264 MP4 at this size as is; do not re-encode.

## Cover frame

Platforms let the user choose a cover on upload. Give them a good one: the hook card, fully settled. Take the time from the storyboard.

```bash
ffmpeg -y -ss <hook settled time, e.g. 1.2> -i ../shortform.mp4 -frames:v 1 -q:v 2 ../cover.jpg
```

Look at it. If it caught a transition, nudge the time by a few tenths and re-extract. Do not bake the cover into the video; frame 1 is already the hook, and an extra frame would break the loop.

## post.md

Write `<out>/post.md`. The user should be able to copy from it straight into each app.

```markdown
# Post kit: <subject>

## Caption
<1–2 sentences. First 6 words restate the hook, because captions truncate. End with the CTA.>

## Hashtags
<3–5. Mix: 1–2 niche, 1–2 audience, 1 broad. No banned-looking walls of tags.>

## YouTube Shorts title
<under 60 characters, front-load the keyword, may include #Shorts>

## Cover
cover.jpg — <the text visible on it>

## Platform notes
- TikTok: <caption variant if needed; suggest adding a trending sound at low volume over the original audio>
- YouTube Shorts: <title above; description line; related-video link if they have one>
- Instagram Reels: <caption variant; note that the cover should be checked against the 4:5 profile-grid crop>

## CTA check
The video says: "<cta>". Before posting, make sure <the link is in your bio | the waitlist page is live | the repo is public>.

## Music credit
<track name, artist, licence — from assets/music/CREDITS.md; or "user supplied">
```

Rules for the copy:

- Same truth rule as the video: no claims the source or brief does not support.
- Write like a person. No "Excited to share", no emoji walls.
- Hashtags are relevant and specific; never pad with generic viral tags.

## Tell the user

Keep it short:

1. Where the video is, its length, and the objective it was built for.
2. That `post.md` has the caption, hashtags and title, and `cover.jpg` is the cover.
3. The one thing to do before posting (from the CTA check).
4. Offer one re-roll: a different hook from the plan, a different length, or a different track.
