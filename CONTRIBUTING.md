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
- **Bump the version** in `.claude-plugin/plugin.json` when the skill's behaviour changes.
- **Keep the hyperframes CLI pinned.** Every command in the skill says `npx hyperframes@<version>`, and CI fails on a bare `npx hyperframes`. Raising the pin is its own pull request: change every occurrence, re-run the example end to end, and say in the description what the new version changed.

## Reporting a bad video

Open an issue with the brief you used, your agent and model, and the `plan.md` from the output folder. A screenshot of the frame that went wrong helps most.
