# Berachain Skills

Shared Berachain coding conventions for AI coding agents. Install these skills
on your computer to use them across projects, or install them in a single repo.

## Skills

| Skill | Scope |
| --- | --- |
| [typescript-coding-standards](skills/typescript-coding-standards/SKILL.md) | Shared TypeScript conventions for services, workers, packages, React, and Next.js |

## Install on your computer

You need Node.js with npm (`npx`), Git, and a supported coding agent.
Run this command in your terminal:

```sh
npx skills add berachain/skills --global
```

Choose the skills and agents in the prompts. `--global` makes the installation
available across your projects. No local clone is needed.

To install all skills for a specific agent, use one of:

```sh
npx skills add berachain/skills --global --skill '*' --agent codex
npx skills add berachain/skills --global --skill '*' --agent claude-code
npx skills add berachain/skills --global --skill '*' --agent cursor
```

For SSH authentication, replace `berachain/skills` with
`git@github.com:berachain/skills.git`.

## Install in one project

Run from the consuming project's root, omitting `--global`:

```sh
npx skills add berachain/skills --skill typescript-coding-standards
```

## Check and update

```sh
# List skills installed on your computer
npx skills list --global

# Update this skill
npx skills update typescript-coding-standards --global
```

See the [skills CLI documentation](https://github.com/vercel-labs/skills) for
supported agents and installation options.

## Install local changes

From this checkout, preview available skills and install your local version:

```sh
npx skills add . --list
npx skills add . --global --skill typescript-coding-standards
```

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
      dependencies.md
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
