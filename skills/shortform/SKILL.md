---
name: shortform
description: Turn the current project, or any brief, into a vertical 9:16 short-form video built for TikTok, YouTube Shorts and Instagram Reels, using HyperFrames. Takes a free-text objective such as "attract B2B customers" or "grow followers" and shapes the hook, structure and call to action around it. Use when someone says "/shortform", "shortform", "make a TikTok", "make a Short", "make a Reel", "short-form video", or wants a vertical social video about what they built.
license: MIT
compatibility: Requires Node.js 22+, FFmpeg on PATH, the hyperframes CLI (npx hyperframes@0.8.46) and the hyperframes domain skills.
---

# /shortform

Make one vertical video that a stranger scrolling a feed will stop for, watch to the end, and act on.

## Input

Treat any text the user supplied with the invocation as the **brief**. It is prose, not flags:

```
/shortform attract B2B customers for my invoicing app
/shortform 15 second TikTok, get devs to star the repo
/shortform teach people how compound interest works, no project, playful
```

Extract from the brief, in the user's words where possible:

| Field | Default when absent |
|---|---|
| Objective (what the viewer should do or feel) | ask — see below |
| Audience | inferred from project or brief |
| Platform (`tiktok`, `shorts`, `reels`) | universal |
| Length in seconds | 20–30 (hard cap 60) |
| Call to action wording | from the archetype |
| Tone | inferred |
| Media paths (images, screen recordings) | none |
| Music (`off`, a mood, or a file path) | on, mood matched to tone |
| Voiceover | off |

If the brief has no objective, ask exactly one question: "What should this video achieve, and for whom?" If you cannot ask, assume `announce-launch` for a project and `educate` for a topic, and say so in the plan's Assumptions line.

**"Cannot ask" means:** the brief says to go straight through ("just do it", "no questions", "don't ask"), or you are running somewhere no one will answer — a scheduled or CI run, a hook, a subagent with no channel back to the user. Anywhere a person is reading your output as it appears, you can ask. When unsure, ask: one question costs a few seconds, a video built for the wrong objective costs the whole run.

**Subject.** If the working directory holds a project, the video is about that project. If it holds no project, or the brief says to ignore it, run brief-only: the brief is the whole source.

## Output

Everything goes in `shortform-output/`. If that directory exists, use `shortform-output-YYYY-MM-DD-HHmmss/` instead, and use the same directory for the whole run.

```
<out>/plan.md            storyboard and decisions
<out>/composition/       HyperFrames project
<out>/shortform.mp4      1080x1920, 30fps
<out>/cover.jpg          cover frame
<out>/post.md            caption, hashtags, title, posting notes
```

Paths such as `assets/music/` and `references/` in this skill are relative to the directory containing this SKILL.md. Resolve them from there, never from a hardcoded home-directory path.

**Pinned CLI.** Every command in this skill runs `npx hyperframes@0.8.46`. Use that exact version; do not drop the pin for `npx hyperframes`, whose newest release can change flags and output shape under you. The pin is raised deliberately, in one pull request that re-runs the example.

The pin covers the CLI only. The hyperframes domain skills read in Step 3 install from upstream `main` and carry no version, so their guidance can change under a fixed CLI pin. When their mechanics and this skill's format or story rules disagree, `compose.md` says which wins.

## Step 0: Preflight

Before any other work, run from the project directory:

```bash
npx hyperframes@0.8.46 doctor --json
```

The command always exits 0, and its top-level `ok` is false whenever any optional tool is absent, so ignore both. Read the `checks` array and require `ok: true` on exactly these four: `Node.js`, `FFmpeg`, `FFprobe`, `Chrome`. Everything else (`whisper-cpp`, `TTS (Kokoro)`, `BGM (MusicGen)`, `Docker`, `Docker running`) is optional; the only one that ever matters is `TTS (Kokoro)`, and only when the brief asks for voiceover.

Also confirm the hyperframes domain skills are available to you: `hyperframes-core`, `hyperframes-animation`, `hyperframes-creative`, `hyperframes-keyframes`, `hyperframes-cli`.

If a required check fails, stop and tell the user what is missing and how to fix it. Do not install anything yourself.

| Missing | Tell the user |
|---|---|
| Node.js (needs 22+) | install from https://nodejs.org |
| FFmpeg / FFprobe | `brew install ffmpeg`, `apt install ffmpeg`, or https://ffmpeg.org/download.html |
| Chrome | `npx hyperframes@0.8.46 browser ensure` |
| hyperframes skills | `npx hyperframes@0.8.46 skills update <name> …`, naming each missing skill. Bare `skills update` refreshes what is already installed and does not expand a partial install, so a skill that was never there stays missing. |
| anything else | relay the `hint` field from that doctor check |

**Gate:** the four required checks pass and the five skills are readable.

## Step 1: Understand the source

**Read:** [references/inspect.md](references/inspect.md)

**Gate:** you can answer every question in its rubric.

## Step 2: Plan

**Read:** [references/archetypes.md](references/archetypes.md), then [references/plan.md](references/plan.md)

Pick the nearest objective archetype, write three hook options, and write `<out>/plan.md`.

**Approval gate.** Show the user the three hooks and the storyboard table and wait for one reply. Skip this gate when the brief says to go straight through ("just do it", "no questions") or when you cannot ask; then take the hook you ranked first.

**Gate:** `<out>/plan.md` exists, scene durations sum to the target length, and the hook is chosen.

## Step 3: Compose

**Read:** the five hyperframes domain skills named in Step 0. Do not enter the `hyperframes` entry-point interview; this skill has already made those decisions.
**Read:** [references/vertical.md](references/vertical.md), then [references/compose.md](references/compose.md)

Build the HyperFrames project in `<out>/composition/` with a 1080x1920 root.

**Gate:** `npx hyperframes@0.8.46 check` passes with zero errors inside `<out>/composition/`, and every rule in the vertical checklist holds.

## Step 4: Render and deliver

**Read:** [references/deliver.md](references/deliver.md)

**Gate:** `shortform.mp4` is 1080x1920 and within the target length, `cover.jpg` exists, and `post.md` is written.

## Laws

These hold for every video, whatever the objective.

1. **The first 2 seconds decide everything.** Frame 1 already shows the hook text and motion. No logo sting, no fade from black, no "hey guys".
2. **Sound-off first.** The on-screen text alone carries the whole message. Music adds energy, never meaning.
3. **One idea.** One objective, one audience, one call to action. Cut everything else.
4. **Stay in the safe zone.** Platform UI covers the top, bottom and right edge. See `references/vertical.md`.
5. **Readable.** A short label holds at least 0.8s after it settles; a sentence holds about 0.3s per word. Fast in, then hold.
6. **Something changes every 2–3 seconds.** A cut, a zoom, a new caption, a new scene. Stillness is where viewers leave.
7. **Show the real thing.** At least one scene shows the actual product, UI, data or subject. No abstract filler, no stock phrases such as "streamline your workflow".
8. **End on the ask, then loop.** The last scene is the call to action, still fully readable on the final frame, on the same background the hook opens on.
9. **Say nothing untrue.** Use only claims, numbers and quotes found in the source or the brief. Never invent customers, metrics or testimonials.
