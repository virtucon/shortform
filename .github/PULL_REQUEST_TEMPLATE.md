## What this changes

<!-- One or two sentences. What is different after this lands? -->

## Why

<!-- For a creative change: what went wrong in a real video without it? A before and after clip is the best evidence there is. -->

## Checks

```bash
uvx --from "git+https://github.com/agentskills/agentskills@69ef37e9424c0a7ea9dd2293b559e43ec8176379#subdirectory=skills-ref" skills-ref validate ./skills/shortform
npx -y skills@1.7.0 add . --list
```

- [ ] Both commands above pass locally (CI runs them too)
- [ ] I made a video with the change and watched it on a phone, sound off

## If it applies

- [ ] `SKILL.md` is still under 500 lines and still free of harness-specific syntax
- [ ] Any new asset has a row in [`THIRD_PARTY_NOTICES.md`](../THIRD_PARTY_NOTICES.md), and any new track is CC0 with a row in `assets/music/CREDITS.md`
- [ ] Any version pin I moved is moved everywhere it appears, and the example was re-run
- [ ] The README says what the skill now actually does
