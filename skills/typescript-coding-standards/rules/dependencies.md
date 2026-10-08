# Dependencies and catalogs

## Pin dependency versions

Pin external packages in `dependencies`, `devDependencies`, and
`optionalDependencies` to exact versions, such as `"1.2.3"`. Do not use semver
ranges (`^`, `~`, `>=`, or `*`) or moving tags such as `latest`. A lockfile does
not replace an exact version declaration.

`peerDependencies` are exempt: use compatibility ranges when appropriate for
the package's supported consumers. An exact peer version is still allowed.
If a peer is also installed as a development dependency, pin that development
dependency separately.

## Use catalogs in monorepos

Declare external dependency versions centrally in the workspace catalog and
reference them from package manifests with `"catalog:"` or `"catalog:<name>"`.
Keep non-peer catalog entries pinned to exact versions; do not duplicate
versions across individual manifests.

Follow the repository's package manager and catalog layout:

- pnpm, as in monobera: `catalog` and named `catalogs` in
  `pnpm-workspace.yaml`.
- Bun, as in beep: `workspaces.catalog` (or named `workspaces.catalogs`) in
  the root `package.json`.

Reuse the appropriate existing catalog. Keep peer compatibility ranges in a
separate peer catalog when the repository uses one, such as monobera's
`catalog:peer`. Never use a ranged peer catalog entry for a non-peer dependency.

Preserve `workspace:` references for local workspace packages, such as
`"workspace:*"`; these link local packages rather than select a registry version.
When a package intentionally consumes a published internal package, use its
pinned catalog entry instead of changing it to a local workspace link.

When adding or upgrading a dependency, update its catalog entry and consuming
manifest as needed, then regenerate the lockfile with the repository's package
manager. Outside a monorepo, put the exact version directly in `package.json`.
