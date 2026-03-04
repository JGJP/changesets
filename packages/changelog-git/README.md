# @changesets/changelog-git

A changelog entry generator for [changesets](https://github.com/changesets/changesets) that includes abbreviated commit hashes.

This is a simpler alternative to [`@changesets/changelog-github`](https://github.com/changesets/changesets/tree/main/packages/changelog-github) that works without any external API calls — it only uses local git information.

## Installation

```bash
npm install @changesets/changelog-git
```

## Usage

Set it as your changelog generator in `.changeset/config.json`:

```json
{
  "changelog": "@changesets/changelog-git"
}
```

No additional options are required.

## Example Output

```md
## 1.2.0

### Minor Changes

- abc1234: Added a new feature

### Patch Changes

- Updated dependencies [def5678]
  - @scope/dependency@2.0.1
```

Each entry is prefixed with the first 7 characters of the commit hash that introduced the changeset. Dependency updates are listed with their corresponding commit references.
