# AGENTS.md

## Table of contents

- [Purpose](#purpose)
- [Spelling and writing](#spelling-and-writing)
- [Markdown formatting](#markdown-formatting)
- [Authoring voice](#authoring-voice)
- [Commit messages](#commit-messages)
- [Pull requests](#pull-requests)
- [Git workflow](#git-workflow)
- [Licence headers](#licence-headers)
- [The hero banner](#the-hero-banner)
- [Testing](#testing)
- [Shared infrastructure](#shared-infrastructure)
- [Repository layout](#repository-layout)

## Purpose

This is `liminal-hq-labs/.github`, the organisation repository for **Liminal HQ Labs**, the experimental sibling of [Liminal HQ](https://github.com/liminal-hq). It holds the organisation profile, the Labs charter, the hero banner and the checks that keep them honest. There is no application code here. New projects start from [`labs-template`](https://github.com/liminal-hq-labs/labs-template), which carries these same rules.

## Spelling and writing

**REQUIREMENT:** Use **Canadian English** in comments, docs, commit messages and pull request text: `colour`, `centre`, `behaviour`, `organisation`, `licence` (noun) and `license` (verb), and `-ize` endings such as `customize`. Keep American spellings where a tool, API or CSS property requires them (`color`, `prefers-color-scheme`).

- Use a real em dash (`—`) in prose, never `--`. A command-line flag such as `--static` is not prose.
- Sentence case for headings, matching the existing documents.
- Write tight. No padding, no restating the heading.

## Markdown formatting

**REQUIREMENT:** Do not hard-wrap markdown prose. Write each paragraph or bullet as one line and let the renderer reflow it. Commit message bodies are the exception: hard-wrap them at roughly 72 characters.

## Authoring voice

**REQUIREMENT:** Ship the result, not how it was arrived at. Write comments, docs and pull request text as the author of the artifact, for the person who meets it later.

- No "this PR", reviewer names or commit hashes in comments or docs. State the fact or the reasoning directly.
- When a comment has been edited more than once, rewrite it as one clean explanation.
- Commit messages and a pull request's `## Test plan` are the exceptions: they are the right place to record why a change happened and how it was checked.

## Commit messages

**Format:** Conventional Commits (`feat`, `fix`, `docs`, `test`, `ci`, `build`, `chore`, `refactor`, `style`), with an optional scope.

- The body explains what changed and why. Use markdown with **bold labels**, `code` and flat bullets, and no headings.
- End with a `Co-Authored-By:` trailer for the model that wrote the change.
- Write the message to a file with a single-quoted heredoc and commit with `git commit -F <file>`, so backticks and `$()` are not expanded. Check the result with `git log -1 --pretty=fuller`.

## Pull requests

- **Title:** capitalized, outcome-focused, no Conventional Commit prefix.
- **Body:** `## Summary` with flat bullets that open with a **bold lead-in**, then `## Test plan` with `- [x]` or `- [ ]` items naming the commands run. Say plainly what was not verified.
- **Labels:** one of `enhancement`, `bug`, `documentation`, `testing`, `ci`, `build`, `chore`, plus scope labels where useful.

## Git workflow

- `main` is the default branch. Work on topic branches (`feat/`, `fix/`, `docs/`, `chore/`, `ci/`). Never commit to `main` directly.
- **Merge commits only.** No squash and no rebase merges. Branches are deleted on merge.
- **Never** push, tag or dispatch workflows unless asked.
- **Never** run `git add -A` or `git commit -a`. Stage explicit paths.
- Planning files such as `PLAN.md` stay out of git.

## Licence headers

Every script, YAML, SVG and other source file starts with a one-line summary, a blank comment line and the copyright block:

```sh
# One-line summary of the file.
#
# (c) Copyright 2026 Liminal HQ, Scott Morris
# SPDX-License-Identifier: Apache-2.0 OR MIT
```

Use `//` comments for JavaScript and an XML comment for SVG. Markdown, JSON (including `.markdownlint.jsonc`) and lockfiles don't need one. `scripts/check-headers.sh` enforces it. Everything here is licensed `Apache-2.0 OR MIT` (`LICENSE-APACHE`, `LICENSE-MIT`).

## The hero banner

`assets/labs-hero.svg` is the organisation's face, so it is held to the same standard as the Liminal HQ and Mailslot banners.

- **Palette:** Afterglow — void `#050507`, text `#e0e0e0`, muted `#b4bfce`, orange `#ffaa40`, rose `#f43f5e`, purple `#a78bfa`, cyan `#22d3ee`, blue `#60a5fa` — plus one Labs accent, **lab green** `#a3e635` with the softer glow `#4ade80`. Lab green is for the flask liquid, glows, bubbles and a few highlights, never body text on a light background.
- **Self-contained:** SMIL animation and a small `<style>` block only. No scripts, `foreignObject`, images, web fonts or external references. At most 60 KB.
- **Accessible:** `role="img"`, `<title>` and `<desc>` that describe the scene.
- **Motion:** one loop set by `data-loop` on the root element. Every animation starts at 0 and its length divides the loop. Under `prefers-reduced-motion: reduce` animated parts are hidden and a complete still frame shows instead.
- **Text:** the wordmark is outlined paths between the `hero-text` markers. Other text uses system font stacks that end in `sans-serif` or `monospace`, with room for wider fallback fonts.
- Indent with tabs.

`scripts/check-banner.sh` checks all of this, and its render pass measures text fit through the whole loop in headless Chromium. Use `--shots DIR` to write frames, and look at them before committing a change to the banner.

## Testing

Run these before opening a pull request. CI runs the same checks.

```sh
docker run --rm -v "$PWD:/workdir" davidanson/markdownlint-cli2:v0.17.2 "**/*.md"
bash scripts/check-headers.sh
bash scripts/check-banner.sh
docker run --rm -v "$PWD:/mnt" -w /mnt koalaman/shellcheck:stable scripts/*.sh
docker run --rm -v "$PWD:/repo" -w /repo rhysd/actionlint
```

The banner check needs `xmllint`, `rsvg-convert`, `python3` and Chromium or Chrome. State exactly what ran and what did not.

## Shared infrastructure

Labs reuses Liminal HQ's shared CI images and reusable workflows rather than keeping its own. They live in [`liminal-hq/.github`](https://github.com/liminal-hq/.github): pick the leanest image tier that fits (`ci-rust`, `ci-web`, `tauri-ci-desktop` and others) and pin it by digest as described there. Only Labs-specific material belongs in this repository.

## Repository layout

```text
README.md AGENTS.md CLAUDE.md LICENSE-APACHE LICENSE-MIT
.editorconfig .markdownlint.jsonc .gitignore
profile/README.md     organisation landing page on github.com/liminal-hq-labs
assets/labs-hero.svg  animated hero banner
docs/labs-charter.md  what a Labs project is, graduation, sunsetting, starting a project
scripts/              header check, banner check and its Chromium text-fit harness
.github/workflows/    CI: markdown lint, header check, banner check
```
