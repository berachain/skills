# Tooling and verification

## Formatting and linting

Read root and package-level configuration and scripts before running tools.
Shared coding conventions do not require identical formatter settings or tools.

- Preserve the repository's indentation, line width, and import ordering.
- Use type-only imports where required and remove unused imports. Follow local
  environment, logging, and restricted-import rules.

Run the pinned local tools through the package manager declared by the repo.
Check existing lint and format scripts and scope writes to the requested change.
Finish with the non-writing check used by that repo's CI.

Do not hardcode a tool version from this skill, download a newer version, change
formatter settings, or expand/disable plugin coverage just to make a change
pass. Migrations may enforce a rule only in selected directories; new code should
still follow the convention. Review unsafe autofixes for semantic changes.

## Completing a change

After TypeScript edits, run the repository's configured lint/format checks on
the affected code, then the relevant typecheck and tests. Use the
[testing rule](testing.md) to choose meaningful coverage. Read package scripts
and CI for actual commands; do not copy a package manager or tool version from
another repo. Regenerate derived artifacts when the changed source requires it.

Resolve errors introduced by the change and report pre-existing failures or
unavailable checks explicitly. A style refactor must preserve types, runtime
behavior, framework contracts, and public API output.
