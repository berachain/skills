# File naming and size

## Naming

| Item | Convention | Example |
| --- | --- | --- |
| TypeScript modules and utilities | camelCase | `loadPrices.ts`, `createContext.ts` |
| Functions and hooks | camelCase | `loadPrices`, `useTokenDialog` |
| Types and interfaces | PascalCase | `LoadPricesArgs` |
| React components and component folders | PascalCase | `TokenDialog/index.tsx` |
| Feature folders and route segments | kebab-case unless locally specified | `reward-vaults/` |
| Fixed primitive constants | SCREAMING_SNAKE_CASE | `POLL_INTERVAL_MS` |
| Framework entry files | Framework-defined | `page.tsx`, `layout.tsx` |
| Tests | Module name plus configured suffix | `loadPrices.unit.test.ts` |

Use the consumer's filename lint rules. PascalCase symbols do not necessarily
imply PascalCase filenames: some repositories require camelCase for all source
modules, while others allow PascalCase class files.

Do not rename HTTP paths, external fields, generated files, migrations, or
framework entrypoints to satisfy a source filename convention. Update imports
and package exports together when renaming a module.

## File size

Target 250 lines; treat 300 as the maximum for new or substantially changed
handwritten files. Split along cohesive responsibilities, not arbitrary line
ranges. Do not compress formatting or introduce needless barrels to meet a count.

Generated files, vendored code, schemas, and static data are exempt. For a large
legacy file, avoid growth and prefer extracting the behavior being changed.
Do not expand a focused edit or mechanical function-style migration into an
unrelated full-file refactor solely to satisfy the limit.
