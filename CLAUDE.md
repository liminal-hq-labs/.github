# CLAUDE.md

A summary of [AGENTS.md](AGENTS.md), which is canonical. Keep the two in sync.

## What this is

`liminal-hq-labs/.github` is the organisation repository for Liminal HQ Labs: the profile README, the Labs charter, the animated hero banner and the checks for them. No application code. Shared CI images and reusable workflows live in [`liminal-hq/.github`](https://github.com/liminal-hq/.github), not here. New projects start from `labs-template`.

## Rules to remember

- **Canadian English.** Real em dashes. Sentence case headings. No hard-wrapped markdown prose. Ship the result, not the journey: no "this PR" or reviewer names in docs or comments.
- **Conventional Commits.** Body in markdown with bold labels and no headings, hard-wrapped, written to a file and committed with `git commit -F`. End with a `Co-Authored-By` trailer.
- **Git.** Topic branches, merge commits only, branches deleted on merge. Never push, tag or dispatch workflows unless asked. Never `git add -A` or `git commit -a`.
- **Formatting.** Tabs everywhere except YAML, which uses spaces.
- **Headers.** SPDX header on every script, YAML, JavaScript and SVG file: summary line, blank, "(c) Copyright 2026 Liminal HQ, Scott Morris", `SPDX-License-Identifier: Apache-2.0 OR MIT`. Not on Markdown or JSON.
- **Banner.** SMIL plus a small `<style>`, no scripts, web fonts or external references, at most 60 KB, `role="img"` with `<title>` and `<desc>`, one loop set by `data-loop`, a full still frame under reduced motion. Lab green (`#a3e635`, glow `#4ade80`) is the only colour Labs adds to Afterglow. Render frames and look at them before committing.

## Commands

```sh
docker run --rm -v "$PWD:/workdir" davidanson/markdownlint-cli2:v0.17.2 "**/*.md"
bash scripts/check-headers.sh
bash scripts/check-banner.sh                 # add --shots /tmp/shots for review frames
docker run --rm -v "$PWD:/mnt" -w /mnt koalaman/shellcheck:stable scripts/*.sh
docker run --rm -v "$PWD:/repo" rhysd/actionlint -chdir /repo
```
