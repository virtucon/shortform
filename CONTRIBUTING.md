# Contributing

Thanks for helping. The skill is plain Markdown, so most contributions are edits to the files in `skills/shortform/`.

## Where things live

```
skills/shortform/SKILL.md        the router: input, steps, gates, laws
skills/shortform/references/     one file per step, loaded on demand
skills/shortform/assets/music/   bundled CC0 tracks and CREDITS.md
.claude-plugin/                  Claude Code plugin and marketplace manifests
.claude/skills, .agents/skills   symlinks to skills/shortform (do not copy files here)
examples/                        sample products and the videos made from them
```

## Try your change

1. Link your checkout so your agent loads it:
   ```bash
   ln -s "$(pwd)/skills/shortform" ~/.claude/skills/shortform    # Claude Code
   ln -s "$(pwd)/skills/shortform" ~/.agents/skills/shortform    # most other agents
   ```
2. Open `examples/paidly` in your agent and run `/shortform attract freelancers as customers`.
3. Watch the result on a phone, sound off. Is the hook readable at once? Is any text under the platform UI?

## Before you open a pull request

```bash
uvx --from "git+https://github.com/agentskills/agentskills@69ef37e9424c0a7ea9dd2293b559e43ec8176379#subdirectory=skills-ref" skills-ref validate ./skills/shortform
npx -y skills@1.7.0 add . --list
```

CI runs both of these, on the same pinned versions, plus the manifest, symlink, relative-link and CLI-pin checks in [`.github/workflows/validate.yml`](.github/workflows/validate.yml). `claude plugin validate .` is worth running too if you have Claude Code; CI cannot.

## Ground rules

- **Keep it portable.** No harness-specific syntax in the skill body (`$ARGUMENTS`, tool names, home-directory paths). It must read the same in Claude Code, Codex, Cursor, Gemini CLI and OpenCode.
- **Keep it short.** `SKILL.md` stays under 500 lines; detail goes in `references/`. If you add a rule, try to remove one.
- **Say why.** A pull request that changes creative guidance should say what went wrong in a real video without it. A before and after clip is the best evidence.
- **Music must be CC0 or public domain**, with the licence stated on the source page. Add it to `assets/music/CREDITS.md` in the same pull request. Keep each track near 1MB.
- **Any other bundled asset gets a row in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)**, and ships its licence text beside the file when the licence requires that — fonts under the SIL OFL do.
- **Bump the version** in `.claude-plugin/plugin.json` when the skill's behaviour changes.
- **Keep the hyperframes CLI pinned.** Every command in the skill says `npx hyperframes@<version>`, and CI fails on a bare `npx hyperframes`. Raising the pin is its own pull request: change every occurrence, re-run the example end to end, and say in the description what the new version changed. Two things to re-check at every bump:
  - **`HYPERFRAMES_SKIP_SKILLS=1` still stops `init` writing into agent directories.** Upstream calls it a temporary escape hatch (`--skip-skills` is `[temporarily ignored]`), so it can go away under us, and then a `/shortform` run installs skills into the user's `~/.agents/skills/` and `~/.claude/skills/`. Test it in a throwaway HOME, and run both halves — the run *without* the variable is the control that proves the test can still fail:

    ```bash
    tmp=$(mktemp -d)
    real_home=$HOME                          # reuse the npm cache; nothing is written to it
    run() {  # $1 = subdirectory; the rest is the environment under test
      (cd "$tmp" && env -u CLAUDE_CONFIG_DIR -u XDG_CONFIG_HOME -u CODEX_HOME \
         -u VIBE_HOME -u HERMES_HOME -u AUTOHAND_HOME \
         HOME="$tmp" npm_config_cache="$real_home/.npm" "${@:2}" \
         npx hyperframes@<version> init "$1" --non-interactive --resolution portrait) \
      || { echo "init failed — this run proves nothing"; return 1; }
    }
    run control                              # no guard
    find "$tmp" -maxdepth 4 -name hyperframes-core   # must PRINT something
    rm -rf "$tmp/.agents" "$tmp/.claude"
    run guarded HYPERFRAMES_SKIP_SKILLS=1    # guard on
    find "$tmp" -maxdepth 4 -name hyperframes-core   # must print NOTHING
    ```

    `env -u` matters: the CLI resolves agent directories from `CLAUDE_CONFIG_DIR`, `XDG_CONFIG_HOME`, `CODEX_HOME`, `VIBE_HOME`, `HERMES_HOME` and `AUTOHAND_HOME` *before* falling back to `$HOME`, so with any of them set — Claude Code often sets the first — the install lands in your real config directory while the throwaway HOME looks clean. If the control prints nothing, or either `run` exits non-zero, the test is broken, not the guard — an `init` that never ran installs nothing and looks exactly like a working guard. If the guarded run prints anything, the guard is gone: say so in the pull request and fix `references/compose.md` before raising the pin.
  - **The doctor check names have not changed.** `SKILL.md` Step 0 names blocking checks (`Node.js`, `FFmpeg`, `FFprobe`, `Chrome`, `Disk`, `Frames cache`, `Archive extractor`, and `/dev/shm` on Linux) by their exact `name` string. Diff `doctor --json` against the old version; a renamed or added check silently stops blocking. Diff on Linux as well as macOS — the `/dev/shm` check only appears on Linux.
- **Know what the pin does not cover.** The hyperframes *domain* skills (`hyperframes-core` and friends) install from upstream `main` with no ref and no version marker, so their guidance can change without any change here. If a video regresses and this repo did not move, suspect them first — `~/.agents/.skill-lock.json` records a content hash per skill, which at least tells you whether the bytes changed.
- **Compositions ship their assets.** No CDN references, no absolute paths outside the composition directory. A render must work with the network down; a blocked CDN does not fail the render, it silently drops the animation.

## Reporting a bad video

Open an issue with the brief you used, your agent and model, and the `plan.md` from the output folder. A screenshot of the frame that went wrong helps most.
