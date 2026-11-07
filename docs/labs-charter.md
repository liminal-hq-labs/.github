# Labs charter

Liminal HQ Labs (`liminal-hq-labs`) is the experimental sibling of [Liminal HQ](https://github.com/liminal-hq). This charter says what a Labs project is, how one graduates to Liminal HQ or is retired, and how to start a new one.

## What a Labs project is

- **An experiment.** It tests whether a tool is worth building and whether it can be calm to use. Its README carries `> **Status:** experimental` and an `experimental` badge until it graduates.
- **Public from day one.** Code, issues and decisions are open from the first commit.
- **Dual-licensed** under `Apache-2.0 OR MIT`, with `LICENSE-APACHE` and `LICENSE-MIT` at the root.
- **Held to the same house rules** as Liminal HQ: Canadian English, Conventional Commits, merge commits only, SPDX headers, tests and honest test plans. `AGENTS.md` in each repository spells them out.
- **Built on shared infrastructure.** CI images and reusable workflows come from [`liminal-hq/.github`](https://github.com/liminal-hq/.github).

## What a Labs project isn't

- **Not a promise.** A project may be paused or abandoned. If it is, it is archived with a note, not deleted.
- **Not stable.** APIs, command-line flags, file formats, settings and UI may break between any two commits. There is no deprecation period before graduation.
- **Not supported.** Issues and pull requests are welcome, but there is no response-time commitment.
- **Not a place for secrets or private work.** Anything that can't be public from the first commit doesn't belong here.

## Graduation

A project graduates to Liminal HQ when it has earned a permanent place.

### Criteria

- It solves a real problem for people other than its author, and its scope is settled.
- Its core behaviour is written down (`SPEC.md` or equivalent) and decisions are recorded in `docs/decisions.md`.
- CI is green and covers formatting, linting, tests and the header check.
- It installs or runs from a documented path without building from a branch.
- The maintainer is willing to keep it working and to deprecate before breaking.

### Steps

1. **Transfer the repository** from `liminal-hq-labs` to `liminal-hq` (Settings → Transfer ownership). A transfer keeps the history, issues, pull requests and stars, and GitHub redirects the old URLs and git remotes.
2. **Update links** in the README, `SPEC.md`, `docs/`, issue templates (the security advisory link in `.github/ISSUE_TEMPLATE/config.yml`) and package metadata (`Cargo.toml` `repository` and `homepage`, AppStream metainfo, desktop files and similar).
3. **Update badges.** Replace the `experimental` status badge with the project's real status and point CI badges at the new owner.
4. **Rename CI images and registries**, for example `ghcr.io/liminal-hq-labs/<name>-ci` to `ghcr.io/liminal-hq/<name>-ci`, and update every workflow that pulls them.
5. **Change the footer** from `Part of Liminal HQ Labs` to `Part of Liminal HQ`, and drop the `experimental` topic.
6. **Announce it.** Add the project to the [Liminal HQ profile README](https://github.com/liminal-hq/.github/blob/main/profile/README.md) and to [liminalhq.ca](https://liminalhq.ca/), and move it from "What we're trying" in the [Labs profile](../profile/README.md) to a short "Graduated" note.
7. **Recheck repository settings** after the transfer: labels, topics, merge settings, workflow permissions and secret scanning carry over, but confirm them.

## Sunsetting

When an experiment stops, it is archived rather than deleted, so links keep working and the history stays useful.

1. Add a note at the top of the README: `> **Archived:** <one line on why and what to use instead>.`
2. Close or answer open issues and pull requests with a short pointer to that note.
3. Archive the repository (Settings → Archive this repository).
4. Move the project in the [Labs profile](../profile/README.md) from "What we're trying" to a short "Retired" note.

## Starting a project

1. **Create the repository from the template.**

	```sh
	gh repo create liminal-hq-labs/<name> --public --template liminal-hq-labs/labs-template --description "<one line>"
	```

2. **Fill in the placeholders.** Clone it and run `scripts/init-project.sh <name> "<one-line description>"`, then follow `docs/new-project.md` in the new repository.
3. **Apply the labels.** Copy the house set from Mailslot, then delete the ones that are Mailslot-specific (`cli`, `ipc`, `protocol`, `sync`, `upnp`, `web-ui` and similar) or reword their descriptions.

	```sh
	gh label clone liminal-hq-labs/mailslot -R liminal-hq-labs/<name>
	```

4. **Set the topics.** Always `liminal-hq-labs` and `experimental`, plus the language, toolkit and domain.

	```sh
	gh repo edit liminal-hq-labs/<name> --add-topic liminal-hq-labs,experimental,<language>,<domain>
	```

5. **Match the repository settings.** Merge commits only, branches deleted on merge, issues and projects on, discussions and wiki off, and workflows limited to read-only tokens.

	```sh
	gh repo edit liminal-hq-labs/<name> \
		--enable-merge-commit --enable-squash-merge=false --enable-rebase-merge=false \
		--delete-branch-on-merge \
		--enable-issues --enable-projects --enable-discussions=false --enable-wiki=false
	gh api -X PUT repos/liminal-hq-labs/<name>/actions/permissions/workflow \
		-f default_workflow_permissions=read
	```

6. **Security.** Secret scanning and push protection are on by default for public repositories. Confirm both under Settings → Code security, or turn them on:

	```sh
	gh repo edit liminal-hq-labs/<name> --enable-secret-scanning --enable-secret-scanning-push-protection
	```

7. **Add it to the profile.** List the project under "What we're trying" in the [Labs profile](../profile/README.md).

### Dependabot is opt-in

Labs repositories do not ship a `.github/dependabot.yml`, so there are no automatic update pull requests. A project that wants weekly updates adds one itself, with GitHub Actions and the project's own ecosystem (`cargo`, `npm`, `pip`, `docker` and so on):

```yaml
# Weekly dependency updates for GitHub Actions and Cargo.
#
# (c) Copyright 2026 Liminal HQ, Scott Morris
# SPDX-License-Identifier: Apache-2.0 OR MIT

version: 2
updates:
  - package-ecosystem: 'github-actions'
    directory: '/'
    schedule:
      interval: 'weekly'
    open-pull-requests-limit: 5
    labels:
      - 'ci'

  - package-ecosystem: 'cargo'
    directory: '/'
    schedule:
      interval: 'weekly'
    open-pull-requests-limit: 5
    labels:
      - 'build'
```
