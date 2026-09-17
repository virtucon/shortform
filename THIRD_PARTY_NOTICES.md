# Third-party notices

The skill itself is MIT (see [LICENSE](LICENSE)). The files below are third-party works redistributed in this repository under their own licences, which are not MIT.

## Bundled with the skill

| Files | Work | Licence |
|---|---|---|
| `skills/shortform/assets/music/*.mp3` | Four music tracks by Of Far Different Nature, omfgdude, Emma_MA and congusbongus, via [OpenGameArt](https://opengameart.org) | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) — per-track sources and edits in [`CREDITS.md`](skills/shortform/assets/music/CREDITS.md) |

## Bundled with the example composition

`examples/paidly/shortform-output/composition/` is a rendered output checked in as a worked example. It carries the assets any composition carries.

| Files | Work | Licence |
|---|---|---|
| `assets/gsap.min.js` | [GSAP](https://gsap.com) 3.14.2, © GreenSock | [GreenSock standard licence](https://gsap.com/standard-license) — the licence header is kept in the file |
| `assets/fonts/SpaceGrotesk.woff2` | [Space Grotesk](https://github.com/floriankarsten/space-grotesk), © 2020 The Space Grotesk Project Authors | SIL Open Font License 1.1 — full text alongside the font in [`assets/fonts/OFL.txt`](examples/paidly/shortform-output/composition/assets/fonts/OFL.txt) |
| `assets/upbeat.mp3` | "Funky House" by Of Far Different Nature | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) |

## Not bundled

[HyperFrames](https://github.com/heygen-com/hyperframes) does the rendering and is invoked through `npx`. It is never vendored here; it carries its own licence.

## Adding an asset

Anything you add to `skills/shortform/assets/` or to a checked-in composition gets a row here in the same pull request, with the licence named and its text shipped beside the file when the licence requires it (the SIL OFL does). Music has the stricter rule — CC0 or public domain only, see [CONTRIBUTING.md](CONTRIBUTING.md).
