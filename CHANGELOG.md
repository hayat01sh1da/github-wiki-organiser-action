# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### 1. Fixed

- `README.md` described the action as running the `spreen` CLI.  
  The executable was renamed to `wiki-organise` in `spreen-wiki` 0.3.0 (RubyGem) / 0.2.0 (PyPI), and `action.yml` has invoked the new name since the pin moved to 0.2.0 — only the prose was left behind, documenting a command that no longer exists.

### 2. Changed

- Bumped the pinned `spreen-wiki` version 0.2.0 → 0.2.1, picking up the fix for `wiki-organise --version` reporting `0.1.0`.  
  The action does not invoke `--version`, so runtime behaviour is unchanged; the bump keeps the pin on the current release per the per-release pinning policy.

## [0.1.0] - 2026-07-15

### 1. Added

- Initial composite action **Spreen Wiki Organiser**: checks out the caller's wiki (`wiki-repository`, defaulting to the calling repository's own wiki), installs the pinned [`spreen-wiki`](https://pypi.org/project/spreen-wiki/) PyPI package (0.1.0), and runs the `spreen` CLI (`update` / `count-report` / `llm-export` selected via `command`).
- Inputs mirroring the CLI flags (`group-by`, `language`, `home-overflow`, `template-dir`, `output`) plus action-side controls (`token`, `push`, `commit-message`, `slack-webhook-url`).
- Commit-and-push of wiki changes as `github-actions[bot]` when the run produced a diff, exposed via the `changed` output; optional Slack Incoming Webhook notification on pushed changes.
- `README.md` with usage, inputs/outputs, token & permissions guidance, and versioning policy; MIT `LICENSE.txt`.
