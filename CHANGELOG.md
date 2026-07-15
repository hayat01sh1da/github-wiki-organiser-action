# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-07-15

### 1. Added

- Initial composite action **Spreen Wiki Organiser**: checks out the caller's wiki (`wiki-repository`, defaulting to the calling repository's own wiki), installs the pinned [`spreen-wiki`](https://pypi.org/project/spreen-wiki/) PyPI package (0.1.0), and runs the `spreen` CLI (`update` / `count-report` / `llm-export` selected via `command`).
- Inputs mirroring the CLI flags (`group-by`, `language`, `home-overflow`, `template-dir`, `output`) plus action-side controls (`token`, `push`, `commit-message`, `slack-webhook-url`).
- Commit-and-push of wiki changes as `github-actions[bot]` when the run produced a diff, exposed via the `changed` output; optional Slack Incoming Webhook notification on pushed changes.
- `README.md` with usage, inputs/outputs, token & permissions guidance, and versioning policy; MIT `LICENSE.txt`.
