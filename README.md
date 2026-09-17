# /shortform

**One line in your terminal. One vertical video, ready to post.**

You shipped the thing. Nobody saw it. The launch video never happened because editing one costs an afternoon you would rather spend building — so the best thing you made this month is still a link nobody clicks.

Type one sentence into the terminal you already have open. `/shortform` reads your actual code, writes the hook, and renders a 1080×1920 video for TikTok, YouTube Shorts and Reels — kinetic captions, music cut to the beat, cover frame, caption and hashtags in the box. Ninety seconds of your attention, start to upload.

[![validate](https://github.com/Virtucon/shortform/actions/workflows/validate.yml/badge.svg)](https://github.com/Virtucon/shortform/actions/workflows/validate.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/format-Agent%20Skills-8A2BE2)](https://agentskills.io)

<p align="center">
  <img src="examples/paidly/demo.gif" alt="A 25-second vertical video for a fictional invoicing app, made by /shortform" width="300">
</p>
<p align="center"><sub>Made with <code>/shortform attract freelancers as customers, TikTok, 25 seconds</code> · <a href="examples/paidly/shortform-output/shortform.mp4">watch with sound</a> · <a href="examples/paidly/shortform-output/post.md">the post kit it wrote</a></sub></p>

```
/shortform attract B2B customers for my invoicing app
```

That is the whole interface. No timeline. No flags. No stock footage, no watermark, no uncanny "AI avatar" reading your README aloud. `/shortform` is an open-source [agent skill](https://agentskills.io) — plain Markdown your agent reads — and it builds the video out of your real UI, your real colours and your real words.

It interrupts you once: here are three hooks and a storyboard, pick one. (Twice, if your brief never said what the video is for.) Then it renders while you go do something else.

Free. MIT. Runs on your machine. Works with the agent you already pay for.

**Status: early.** Version 0.1.0. It works end to end — the example below is a real run — but it has been exercised on a handful of projects, not hundreds. Expect rough edges, and please report them.

## Install

**Claude Code**

```
/plugin marketplace add Virtucon/shortform
/plugin install shortform@shortform
```

**Codex, Cursor, Gemini CLI, OpenCode, Copilot and other agents**

```bash
npx skills add Virtucon/shortform
```

Add `-g` to install for every project. The [`skills` CLI](https://github.com/vercel-labs/skills) puts it in the right folder for each agent it finds.

**By hand**: copy `skills/shortform/` into your agent's skills folder (`~/.claude/skills/`, `~/.agents/skills/`, or whatever your agent reads).

### You also need

- [Node.js](https://nodejs.org) 22 or newer
- [FFmpeg](https://ffmpeg.org/download.html) (with `ffprobe`) on your `PATH`
- Chrome, which renders the frames: `npx hyperframes@0.8.46 browser ensure`
- The [HyperFrames](https://github.com/heygen-com/hyperframes) skills, which do the rendering: `npx hyperframes@0.8.46 skills update hyperframes-core hyperframes-animation hyperframes-creative hyperframes-keyframes hyperframes-cli`

The skill runs `hyperframes doctor` first and tells you exactly what is missing and the command that fixes it. It never installs anything itself.

**One thing to know about that dependency.** `/shortform` pins the HyperFrames CLI to `0.8.46`, but the HyperFrames *skills* above install from the upstream repository's `main` branch — there is no version to pin, and nothing records which state you got. Upstream changes to those skills can therefore change how your videos come out with no change in this repository. The pin is raised here deliberately, one pull request at a time; the skills underneath move on their own.

## Use it

Type the command, then say what the video is for in plain words. There are no flags to learn.

```
/shortform attract B2B customers for my invoicing app
/shortform 15 second TikTok, get devs to star the repo
/shortform grow followers, playful, part 1 of a series on our design system
/shortform announce the launch, use the screen recording in ./media/demo.mov
/shortform teach people how compound interest works (no project, just this)
```

| Agent | How to invoke |
|---|---|
| Claude Code, Cursor | `/shortform <brief>` |
| Codex | `$shortform <brief>` |
| Gemini CLI, OpenCode, others | "use shortform to …" or "make a TikTok about …" |

Things you can say in the brief:

| You can set | Example | If you don't |
|---|---|---|
| Objective | "attract customers", "get signups", "hire a designer" | it asks one question |
| Platform | "TikTok", "Shorts", "Reels" | one video, safe for all three |
| Length | "15 seconds" | 20–30s (never over 60) |
| Call to action | "CTA: join the waitlist" | chosen to fit the objective |
| Tone | "deadpan", "premium", "playful" | inferred from your project |
| Your media | "use ./media/demo.mov" | UI is rebuilt from your code |
| Music | "chill music", "use ./track.mp3", "no music" | a bundled track matched to the tone |
| Voiceover | "with voiceover" | off; captions carry the message |

Before the slow render it shows you three hook options and the storyboard, and waits for one reply. Say "just do it" in the brief to skip that.

### What you get

```
shortform-output/
  shortform.mp4    1080×1920, 30fps, ready to upload
  cover.jpg        a cover frame, picked on the hook
  post.md          caption, hashtags, Shorts title, per-platform notes, pre-post checklist
  plan.md          the storyboard and every decision made
  composition/     the HyperFrames project, if you want to edit and re-render
```

When it finishes it offers one re-roll — a different hook from the plan, a different length, or a different track. Or edit the composition yourself:

```bash
cd shortform-output/composition
npm run dev                      # live preview in the browser
npx hyperframes@0.8.46 render --quality delivery --fps 30 --output ../shortform.mp4
```

Want to see the whole thing before you install it? [`examples/paidly/`](examples/paidly) is a complete run, checked in: the fictional project that went in, and the [plan](examples/paidly/shortform-output/plan.md), [video](examples/paidly/shortform-output/shortform.mp4) and [post kit](examples/paidly/shortform-output/post.md) that came out.

## What makes it short-form

A landscape demo cropped to 9:16 dies in a feed. The scroll is ruthless and you get one frame to survive it. `/shortform` plans for that from the first pixel:

- **Objective first.** Eight playbooks (attract customers, drive signups, grow followers, educate, announce a launch, social proof, hire, build community), each with its own hook style, scene structure and call to action.
- **The hook is on frame 1.** No logo sting, no fade in. You pick from three hooks written in three different patterns.
- **Sound-off first.** Big kinetic captions carry the whole message. Music adds energy; it is never needed to understand the video.
- **Safe zones.** Every readable element stays clear of the username, caption, buttons and progress bar that TikTok, Shorts and Reels draw over your video.
- **Phone-sized type.** Minimum sizes, word limits per card, and hold times so text can be read.
- **Something changes every 2–3 seconds,** and cuts snap to the beat where the track has one. Read time wins over beat sync when they disagree; `plan.md` records which cuts snapped.
- **One call to action, then a loop.** The last frame cuts cleanly into the first, so rewatches count.
- **Nothing made up.** Every claim and number on screen is traced to your project or your brief in `plan.md`.

## How it works

1. **Preflight**: checks Node, FFmpeg, Chrome and the HyperFrames skills.
2. **Understand**: reads your README, landing page, styles and core flow for real copy, colours and fonts — straight from the files, no browser and no screenshots of your app.
3. **Plan**: picks the playbook, writes three hooks and a storyboard, and asks you once.
4. **Compose**: builds a 1080×1920 [HyperFrames](https://hyperframes.heygen.com/) composition in HTML and CSS, with captions and music, and runs `hyperframes check`.
5. **Deliver**: renders, verifies size and length, picks the cover, and writes the post kit.

The skill is plain Markdown: a short [`SKILL.md`](skills/shortform/SKILL.md) and one [reference file](skills/shortform/references) per step, loaded only when needed. No binary, no service, no account. Read it, change it, fork it — that is the point.

## FAQ

**Does it post for me?** No. It makes the file and the copy. You upload.

**Can I use a trending sound?** Yes. Upload the video, add the sound in the app, and turn the original audio down. Or say "no music" in the brief.

**Does it work without a project?** Yes. Give it a topic and it makes a typographic video from your brief alone.

**Will it overwrite my last video?** No. If `shortform-output/` exists, the next run writes `shortform-output-YYYY-MM-DD-HHmmss/`.

**Which models work?** Any model your agent runs that can follow a multi-step skill and write HTML. Stronger models make better-looking videos.

**Is the music safe to post?** The four bundled tracks are [CC0](skills/shortform/assets/music/CREDITS.md) (public domain).

## Contributing

Issues and pull requests are welcome, most of all "this video came out wrong" reports with the brief and a screenshot. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Credits

- Inspired by [brag](https://github.com/latent-spaces/brag) by Shunit Haviv Hakimi, which showed how good a one-command project video can be.
- Rendering by [HyperFrames](https://github.com/heygen-com/hyperframes).
- The example composition animates with [GSAP](https://gsap.com) (bundled at `examples/paidly/shortform-output/composition/assets/gsap.min.js`, under GreenSock's own licence, not MIT).
- Music by Of Far Different Nature, omfgdude, Emma_MA and congusbongus, via [OpenGameArt](https://opengameart.org), all CC0.

## License

[MIT](LICENSE)
