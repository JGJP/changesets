# @changesets/changelog-github

A changelog entry generator for [changesets](https://github.com/changesets/changesets) that links to GitHub commits, pull requests, and users.

When you use this package, your changelogs will include links to the relevant PRs and commits, as well as credit to the contributing users — all automatically derived from your Git history and GitHub metadata.

## Installation

```bash
yarn add @changesets/changelog-github
```

## Configuration

In your `.changeset/config.json`, set the `changelog` option to use this package with your GitHub repository:

```json
{
  "changelog": ["@changesets/changelog-github", { "repo": "org/repo" }]
}
```

Replace `"org/repo"` with your GitHub repository path (e.g. `"changesets/changesets"`).

The `repo` option is **required**.

## How It Works

When changesets are consumed (via `changeset version`), this package generates changelog entries that automatically:

- **Link pull requests** — If the changeset commit is associated with a PR, a link to that PR is included.
- **Link commits** — A short-hash link to the commit is added.
- **Credit authors** — The GitHub user who authored the change gets a "Thanks @user!" mention with a link to their profile.
- **Linkify issue references** — Bare `#123` references in your changeset summary are converted to links to the corresponding GitHub issue/PR.
- **List updated dependencies** — For dependency bumps, linked commits and a list of updated packages are included.

## Changeset Summary Overrides

You can override the automatically detected PR, commit, or author by adding special directives at the top of your changeset summary:

```
---
"my-package": minor
---

pr: 1234
commit: abc1234
author: someuser

Added a new feature
```

Supported directives:

| Directive | Example | Description |
| --- | --- | --- |
| `pr` / `pull` / `pull request` | `pr: #1234` or `pr: 1234` | Override the associated pull request |
| `commit` | `commit: abc1234def` | Override the associated commit hash |
| `author` / `user` | `author: @someuser` | Override or add credited users (can be specified multiple times) |

These directives are stripped from the final changelog output.

## GitHub Enterprise

For GitHub Enterprise Server, set the `GITHUB_SERVER_URL` environment variable to your server's base URL. This defaults to `https://github.com` if not set. The variable is automatically available in GitHub Actions.

## Authentication

This package uses `@changesets/get-github-info` to fetch PR and commit metadata from the GitHub API. For public repositories this may work without authentication, but to avoid rate limits or access private repositories, set a `GITHUB_TOKEN` environment variable with a personal access token or GitHub Actions token.

## Example Output

A generated changelog entry might look like:

```markdown
## 1.2.0

### Minor Changes

- [#234](https://github.com/org/repo/pull/234) [`abc1234`](https://github.com/org/repo/commit/abc1234) Thanks [@someuser](https://github.com/someuser)! - Added a new feature

### Patch Changes

- Updated dependencies [`def5678`](https://github.com/org/repo/commit/def5678):
  - some-dependency@2.0.0
```
