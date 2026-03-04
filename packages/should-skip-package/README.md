# @changesets/should-skip-package

Determines whether a package should be skipped during changesets versioning and tagging.

A package is skipped if any of the following are true:

- It is listed in the [`ignore`](https://github.com/changesets/changesets/blob/main/docs/config-file-options.md) array in the changesets config
- It is `private` and `allowPrivatePackages` is not enabled
- It has no `version` field in its `package.json`

## Installation

```bash
npm install @changesets/should-skip-package
```

## Usage

```ts
import { shouldSkipPackage } from "@changesets/should-skip-package";

const skip = shouldSkipPackage(pkg, {
  ignore: ["@scope/internal-tool"],
  allowPrivatePackages: false,
});

if (skip) {
  console.log("Skipping", pkg.packageJson.name);
}
```

## Parameters

- **`pkg`** — A `Package` object from [`@manypkg/get-packages`](https://github.com/Thinkmill/manypkg) containing `packageJson` and `dir`
- **`options.ignore`** — Array of package names to skip
- **`options.allowPrivatePackages`** — When `false`, private packages are skipped
