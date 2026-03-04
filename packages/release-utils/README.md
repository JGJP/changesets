# @changesets/release-utils

Utilities for programmatically running changesets version and publish workflows. Useful for building custom release scripts and CI integrations on top of changesets.

## Installation

```bash
npm install @changesets/release-utils
```

## API

### `readChangesetState(cwd?: string)`

Reads the current changeset state from disk, including any pending changesets and pre-release state.

```ts
import { readChangesetState } from "@changesets/release-utils";

const { changesets, preState } = await readChangesetState(process.cwd());
```

**Returns:** `{ changesets: NewChangeset[], preState: PreState | undefined }`

### `version(cwd?: string)`

Runs the changesets version command programmatically — applies pending changesets to package versions and changelogs.

```ts
import { version } from "@changesets/release-utils";

await version(process.cwd());
```

### `publish({ script, cwd? })`

Runs a publish script and determines which packages were published by comparing git tags before and after.

```ts
import { publish } from "@changesets/release-utils";

const result = await publish({
  script: "pnpm publish -r",
  cwd: process.cwd(),
});

if (result.published) {
  console.log("Published:", result.publishedPackages);
}
```

**Returns:** `{ published: true, publishedPackages: { name, version }[] } | { published: false }`

### `getChangelogEntry(changelog: string, version: string)`

Extracts the changelog section for a specific version from a changelog string.

```ts
import { getChangelogEntry } from "@changesets/release-utils";

const { content, highestLevel } = getChangelogEntry(changelogContents, "1.2.0");
```
