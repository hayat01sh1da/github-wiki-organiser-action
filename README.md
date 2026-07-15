# GitHub Wiki Organiser

[![Action - CI](https://github.com/hayat01sh1da/github-wiki-organiser-action/workflows/Action%20-%20CI/badge.svg)](https://github.com/hayat01sh1da/github-wiki-organiser-action/actions/workflows/action--ci.yml)

Organise (organize) a GitHub wiki from a scheduled workflow: regenerate `Home.md` and `_Sidebar.md` grouped by the Owner/Category declared on the first line of each page (English and Japanese labels built in), or export reports of the pages whose owner or category is unknown.  
This action is a thin adapter over the [`spreen-wiki`](https://pypi.org/project/spreen-wiki/) PyPI package (also published [as a RubyGem](https://rubygems.org/gems/spreen-wiki)): it checks out your wiki, runs the `spreen` CLI, commits and pushes when there is a diff, and optionally notifies Slack.

## 1. Usage

```yaml
name: Organise Wiki

on:
  schedule:
    - cron: "0 3 * * 1-5" # 12:00 JST every weekday
  workflow_dispatch:

permissions:
  contents: write # required to push to this repository's wiki

jobs:
  organise-wiki:
    timeout-minutes: 10
    runs-on: ubuntu-latest
    steps:
      - uses: hayat01sh1da/github-wiki-organiser-action@v0.1.0
        with:
          group-by: Owner
          language: English
          slack-webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

The wiki must already exist with at least one page (GitHub only creates the underlying `<repo>.wiki` git repository once the first page is saved).  
Persistent settings, custom labels and additional languages go into a [`.spreen.yml`](https://github.com/hayat01sh1da/spreen-wiki#readme) file at the root of the wiki itself — the CLI picks it up automatically.

## 2. Inputs

| Input               | Default                                  | Description                                                                                           |
|---------------------|------------------------------------------|-------------------------------------------------------------------------------------------------------|
| `command`           | `update`                                 | `update` (Home + Sidebar), `count-report` or `llm-export`.                                            |
| `wiki-repository`   | `<caller repository>.wiki`               | Wiki repository to organise, as `<owner>/<repo>.wiki`.                                                |
| `token`             | `github.token`                           | Token used to check out and push the wiki. See § 4.                                                   |
| `group-by`          | (CLI default: `Owner`)                   | Grouping criteria: `Owner` or `Category`.                                                             |
| `language`          | (CLI default: `English`)                 | Label language: `English` or `Japanese`.                                                              |
| `home-overflow`     | `false`                                  | `update` only: split Home into per-namespace pages under `wikis-by-owner/`.                           |
| `template-dir`      | (bundled templates)                      | Directory of `<group_by>/<language>.md` Home templates, relative to the wiki root.                    |
| `output`            | (CLI defaults)                           | `count-report` / `llm-export` only: export filename, relative to the wiki root.                       |
| `push`              | `true`                                   | Commit and push the wiki when the run produced a diff. Set `false` for dry runs or report-only usage. |
| `commit-message`    | `Organise wiki by Spreen Wiki Organiser` | Commit message used when pushing.                                                                     |
| `slack-webhook-url` | (disabled)                               | Slack Incoming Webhook URL; a notification is sent when changes were pushed. Pass it from a secret.   |

## 3. Outputs

| Output    | Description                                                                 |
|-----------|-----------------------------------------------------------------------------|
| `changed` | `'true'` when the run produced wiki changes that were committed and pushed. |

## 4. Token & Permissions

- **Own wiki (default)**: the built-in `GITHUB_TOKEN` can push to the calling repository's wiki as long as the job declares `permissions: contents: write` — no PAT needed.
- **Another repository's wiki**: pass a PAT (classic, `repo` scope) as `token`, and set `wiki-repository` to `<owner>/<repo>.wiki`.
- The action never prints the token or the webhook URL; keep both in secrets.

## 5. Versioning

- Pin the exact tag (`@v0.1.0`) or a commit SHA for now; a floating major (`@v1`) arrives with `v1.0.0` after validation on a real wiki, and from then on pinning `@v1` receives fixes automatically.
- Each action release pins an exact `spreen-wiki` package version internally, so existing tags keep their behaviour; package upgrades arrive via new action releases.

## 6. Development

The organiser itself lives in [hayat01sh1da/spreen-wiki](https://github.com/hayat01sh1da/spreen-wiki) — file behaviour issues there, and action-specific issues here.  
The publishing plan and design record is [spreen-wiki#106](https://github.com/hayat01sh1da/spreen-wiki/issues/106).

## 7. License

[MIT](./LICENSE.txt)
