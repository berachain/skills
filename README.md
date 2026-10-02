# Berachain Skills

Reusable agent skills for use across repositories, organized for the
[skills CLI](https://github.com/vercel-labs/skills#skill-discovery).

## Skills

| Skill | Scope |
| --- | --- |
| [typescript-coding-standards](skills/typescript-coding-standards/SKILL.md) | Shared TypeScript conventions for services, workers, packages, React, and Next.js |

## Local installation

From this checkout, list the discoverable skills:

```sh
npx skills add . --list
```

From a consuming project, install using the path to this checkout:

```sh
npx skills add /path/to/skills --skill typescript-coding-standards
```

Once hosted, the same command accepts the actual GitHub `owner/repo` instead of
the local path. No npm package or agent-specific plugin manifest is required.

## Structure

```text
skills/
  typescript-coding-standards/
    SKILL.md
    rules/
      typescript.md
      file-naming-and-size.md
      comments.md
      modules.md
      runtime.md
      react.md
      testing.md
      tooling.md
```

Each skill owns its instructions and resources. Keep `SKILL.md` concise, link
conditional detail from it, and keep references inside the skill so installation
does not depend on sibling repositories. New skills go in `skills/<name>/` with
YAML `name` and `description` frontmatter.

Skills provide shared defaults while respecting each repository's formatter,
runtime error conventions, package boundaries, and test scripts. React and
Next.js instructions apply only to code using those frameworks.

## License

[MIT](LICENSE) © Berachain Foundation.
