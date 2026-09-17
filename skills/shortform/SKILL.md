---
name: shortform
description: Turn the current project, or any brief, into a vertical 9:16 short-form video built for TikTok, YouTube Shorts and Instagram Reels, using HyperFrames. Takes a free-text objective such as "attract B2B customers" or "grow followers" and shapes the hook, structure and call to action around it. Use when someone says "/shortform", "shortform", "make a TikTok", "make a Short", "make a Reel", "short-form video", or wants a vertical social video about what they built.
license: MIT
compatibility: Requires Node.js 22+, FFmpeg on PATH, the hyperframes CLI (npx hyperframes@0.8.46), the hyperframes domain skills, and a model that can view images (the safe-zone gate reads snapshot frames).
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
| Length in seconds | 20–30 |
| Call to action wording | from the archetype |
| Tone | inferred |
| Media paths (images, screen recordings) | none |
| Music (`off`, a mood, or a file path) | on, mood matched to tone |
| Voiceover | off |

**Hard cap 30s.** Whatever the brief asks for, record `min(requested, 30)` as the length and keep the original number to report. Every later step works from the recorded number.

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

**Every step needs the registry.** Each command fetches the pinned CLI through `npx`. If any call fails to reach it — a non-zero exit with an npm error such as `ENOTFOUND`, `ECONNREFUSED`, `ETIMEDOUT`, `E404`, `EAI_AGAIN` or `429`, rather than a normal CLI error — stop at that step and say the hyperframes CLI could not be fetched, quoting the error. Do not drop or change the pin, do not substitute a different version, and do not improvise around the missing command. Leave `<out>/` as it is: whatever is already on disk stays valid and the run can resume from that step once the registry is reachable.

## Step 0: Preflight

Before any other work, run from the project directory:

```bash
npx hyperframes@0.8.46 doctor --json
```

**First, did the command run at all?** `doctor` itself always exits 0 and always prints JSON, so anything else means the command never got to run. Stop before Step 1 either way — every later step shells out to the same CLI, and the expensive failure is the one that lands after the plan is approved — but say which failure it is, because the fixes differ:

- **No JSON and one of the npm error codes above** — the registry could not be reached. Quote the error and offer: check the network or proxy, retry, or pre-install the pinned version (`npm install -g hyperframes@0.8.46`). An `E404` is different: the pin does not exist on the registry, so retrying will not help and this repo has to change it.
- **No JSON and no npm error code** — the CLI was fetched and failed to run (an unsupported Node, a crash in a check). Quote the raw output as it stands and do not send the user looking at their network.

Once the JSON is in hand, ignore the top-level `ok` — it is false whenever any optional tool is absent — and read the `checks` array in three groups.

**Blocking.** Stop the run if `ok` is false on any of: `Node.js`, `FFmpeg`, `FFprobe`, `Chrome`, `Disk`, `Frames cache`, `Archive extractor`, and `/dev/shm` when it is present. `Disk` and `Frames cache` are what makes a render finish rather than start: frames are extracted into the cache directory and the MP4 is written to disk, so a full disk that passes preflight fails in Step 4, minutes of work later. `Archive extractor` unpacks the managed Chrome download. `/dev/shm` appears on Linux only — including Docker, where the default 64MB is below the 256MB Chrome needs and the render dies part-way through.

Two of these do not fail the way you would expect, so read their `detail` as well as their `ok`:

- **`Node.js` never reports `ok: false`** — it only echoes the running version. Read the version out of `detail` yourself and stop if it is below v22.
- **`Disk` and `Frames cache` pass when they cannot measure** — `detail` reads `Unable to check` or `free space unknown`. Treat that as unproven rather than fine: say so, and say the render needs a few GB free. They fail only below 1GB and 2GB respectively, which is already tight.

**Informational.** `Version`, `CPU`, `Memory`, `Environment` never block. Record `Memory` and `CPU` for Step 2, which decides length.

**Optional.** `whisper-cpp`, `TTS (Kokoro)`, `BGM (MusicGen)`, `Docker`, `Docker running`. The only one that ever matters is `TTS (Kokoro)`, and only when the brief asks for voiceover — then it blocks too. A hosted voice key is not a substitute: `tts` synthesises through Kokoro and fails without it, whatever else is configured.

Also confirm the hyperframes domain skills are available to you: `hyperframes-core`, `hyperframes-animation`, `hyperframes-creative`, `hyperframes-keyframes`, `hyperframes-cli`.

If a blocking check fails, stop and tell the user what is missing and how to fix it. Do not install anything yourself.

| Missing | Tell the user |
|---|---|
| the CLI itself (no JSON, npm error code) | the npm registry could not be reached; quote the npm error, then: check the network, retry, or `npm install -g hyperframes@0.8.46`. On `E404` say instead that the pinned version is not on the registry and this repo has to move the pin |
| the CLI itself (no JSON, no npm error) | the CLI was fetched but would not run; quote the output as it stands |
| Node.js below v22 (read from `detail`) | install from https://nodejs.org |
| FFmpeg / FFprobe | `brew install ffmpeg`, `apt install ffmpeg`, or https://ffmpeg.org/download.html |
| Chrome | `npx hyperframes@0.8.46 browser ensure` |
| Disk / Frames cache | relay the `hint` field, and say what the check reported free and that the render extracts frames to that path — a few GB is the working minimum |
| Archive extractor | relay the `hint` field; without it the managed Chrome download cannot be unpacked |
| `/dev/shm` (Linux) | relay the `hint` field; in Docker that means restarting the container with `--shm-size=512m`, which this skill cannot do from inside it |
| TTS (Kokoro), voiceover asked for | relay the `hint` field, or offer to build the video without voiceover |
| hyperframes skills | `npx hyperframes@0.8.46 skills update <name> …`, naming each missing skill. Bare `skills update` refreshes what is already installed and does not expand a partial install, so a skill that was never there stays missing. |
| anything else | relay the `hint` field from that doctor check |

**Gate:** `doctor` returned JSON, every blocking check passes (including Node v22+ read from `detail`, plus `TTS (Kokoro)` when the brief asks for voiceover), and the five skills are readable.

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
