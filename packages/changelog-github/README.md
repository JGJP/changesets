# @changesets/changelog-github

A changelog entry generator for [changesets](https://github.com/changesets/changesets) that links to commits, pull requests, and contributors on GitHub.

## Installation

```bash
npm install @changesets/changelog-github
```

## Usage

Add the following to your `.changeset/config.json`:

```json
{
  "changelog": ["@changesets/changelog-github", { "repo": "org/repo" }]
}
```

Replace `"org/repo"` with your GitHub repository (e.g. `"changesets/changesets"`).

Then run `changeset version` as usual — your `CHANGELOG.md` entries will automatically include links to the relevant pull request, commit, and contributing user.

### Example Output

```md
## 1.2.0

### Minor Changes

- [#123](https://github.com/org/repo/pull/123) [`abc1234`](https://github.com/org/repo/commit/abc1234) Thanks [@username](https://github.com/username)! - Added a new feature
```

## Options

| Option | Type     | Required | Description                          |
| ------ | -------- | -------- | ------------------------------------ |
| `repo` | `string` | Yes      | GitHub repository in `"org/repo"` format |

## How It Works

When `changeset version` is run, this package resolves metadata for each changeset:

1. If the changeset summary contains a `pr: #123` or `commit: abc1234` reference, those are used directly.
2. Otherwise, it looks up the commit that introduced the changeset via the GitHub API.
3. The resulting changelog entry includes links to the pull request, commit hash, and the author who made the change.

### Inline Overrides

You can override the automatically detected PR, commit, or author by adding directives to the top of a changeset summary:

```md
---
"my-package": minor
---

pr: #456
commit: abc1234def
author: @someuser

Added a new feature that does something great.
```

## GitHub Authentication

This package uses [`@changesets/get-github-info`](https://github.com/changesets/changesets/tree/main/packages/get-github-info) to fetch data from the GitHub API. To avoid rate limiting, set a `GITHUB_TOKEN` environment variable with a [personal access token](https://github.com/settings/tokens) (no special scopes required for public repos).

```bash
export GITHUB_TOKEN=ghp_xxxxxxxxxxxx
```

## GitHub Enterprise

If you use GitHub Enterprise, set the `GITHUB_SERVER_URL` environment variable:

```bash
export GITHUB_SERVER_URL=https://github.mycompany.com
```

The package will use this as the base URL for all generated links.
