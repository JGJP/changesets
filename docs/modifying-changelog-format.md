# Modifying The Changelog Formats

Changesets comes with a default format for the changelogs for packages which is relatively basic in what information it displays, however this is customisable. Here we will talk about how to modify the changelog, so that it contains extra meta-information.

## Setting What Formatting Functions to Use

To change how the changelog is generated, you use the `changelog` setting in the `./changeset/config.json`. This setting accepts a string, which points to a module. You can reference an npm package that you have installed, or a local file where you have written your own functions.

For example, `changesets` has a package, `@changesets/changelog-git`. To use it, you would first need to install the package.

```
yarn add @changesets/changelog-git
```

Next, change your `.changeset/config.json` to point to the new package:

```
"changelog": "@changesets/changelog-git"
```

If you want to write your own, you can reference a file path. For example, you can create a new file `.changeset/my-changelog-config.js`, then you can reference it in the `.changeset/config.json` file as:

```
"changelog": "./my-changelog-config.js"
```

## Writing Changelog Formatting Functions

The changelog formatting is done by two different functions. `getReleaseLine` and `getDependencyReleaseLine`. These must be provided in an object as the export of your generation file. A basic file setup for the changelog generation functions would be:

```js
async function getReleaseLine() {}

async function getDependencyReleaseLine() {}

module.exports = {
  getReleaseLine,
  getDependencyReleaseLine,
};
```

These functions are run during the `changeset version` and are expected to return a string (or a promise with a string).

If you are using typescript to write your changelog functions, you can use the type. First install `@changesets/types`, and then:

```ts
import { ChangelogFunctions } from "@changesets/types";

async function getReleaseLine() {}

async function getDependencyReleaseLine() {}

const defaultChangelogFunctions: ChangelogFunctions = {
  getReleaseLine,
  getDependencyReleaseLine,
};

export default defaultChangelogFunctions;
```

### `getReleaseLine`

Called once per changeset per package. It receives:

```ts
type getReleaseLine = (
  changeset: {
    // The summary text from the changeset markdown file
    summary: string;
    // The hash for the commit that introduced the changeset (if available)
    commit: string | undefined;
    // The unique id of the changeset
    id: string;
  },
  // The bump type this changeset applies to the current package: "major", "minor", or "patch"
  type: VersionType,
  // The options object from your config (see "Adding Options" below), or null if none provided
  changelogOpts: Record<string, any> | null
) => Promise<string>;
```

### `getDependencyReleaseLine`

Called once per package when its dependencies are updated. It receives:

```ts
type getDependencyReleaseLine = (
  // All changesets that caused the dependency updates
  changesets: NewChangesetWithCommit[],
  // The dependencies that were updated, with their new versions
  dependenciesUpdated: { name: string; newVersion: string }[],
  // The options object from your config, or null if none provided
  changelogOpts: Record<string, any> | null
) => Promise<string>;
```

Both functions should return a string (the formatted changelog entry), or an empty string to produce no output.

## Adding Options to Changelog Functions

You can pass options to your changelog functions by using an array in your `.changeset/config.json` instead of a plain string:

```json
{
  "changelog": ["@changesets/changelog-github", { "repo": "org/repo" }]
}
```

The second element of the array is passed as the `changelogOpts` parameter to both `getReleaseLine` and `getDependencyReleaseLine`. You can use this to pass any configuration your changelog generator needs — API tokens, repo URLs, formatting preferences, etc.

For a complete example of how options are used, see the source of [`@changesets/changelog-github`](https://github.com/changesets/changesets/tree/main/packages/changelog-github).
