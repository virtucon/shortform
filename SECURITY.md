# Security

## What this project is

`/shortform` is a set of Markdown files that instruct an AI agent. There is no server, no account and no telemetry. Nothing is uploaded anywhere. The skill never installs software itself — it reports what is missing and gives you the command.

That means the security surface is mostly about what the skill tells your agent to run, and what it pulls from the network:

- `npx hyperframes@0.8.46` — the renderer, pinned to an exact version everywhere the skill calls it (CI enforces the pin).
- The HyperFrames domain skills, which install from upstream `main` and carry no version. Their contents can change without a change here. This is documented in the README and is the one dependency this project cannot pin.
- Nothing else. The skill fetches no other code at run time.

Everything the skill writes goes under `shortform-output/` in the directory you run it in.

## Reporting a vulnerability

Report privately through GitHub: **[open a security advisory](https://github.com/virtucon/shortform/security/advisories/new)**. Please do not open a public issue for anything exploitable.

Useful things to include: which file and which instruction, what an attacker controls, and what they get. A brief that makes the skill do something outside `shortform-output/`, or that makes it run an unpinned or attacker-chosen command, is in scope and worth reporting.

Expect a first reply within 7 days. Fixes land on `main` and, when they matter to people who have already installed the skill, in a tagged release with the advisory published alongside it.

## In scope

- Instructions in `skills/shortform/` that could make an agent run untrusted code, write outside the output folder, or exfiltrate repository contents.
- A dropped or weakened version pin, in the skill or in CI.
- Anything in `.github/workflows/` that would let a pull request from a fork gain write access or read a secret.

## Out of scope

- Vulnerabilities in HyperFrames, Chrome, FFmpeg or Node.js themselves — report those upstream.
- The video being bad. That is a [bug report](https://github.com/virtucon/shortform/issues/new?template=bad-video.yml), and it is very welcome, but it is not a security issue.

## Supported versions

This is an early project with a single line of development. Only the latest release on `main` is supported.
