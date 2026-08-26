# Agent Skills

A collection of reusable skills for AI coding agents, packaged in the open [Agent Skills](https://agentskills.io/) format.

[![skills.sh](https://skills.sh/b/mathwro/Skills)](https://skills.sh/mathwro/Skills)

## Install

Install skills from this repository:

```bash
# See the skills available in this repository
npx skills add mathwro/Skills --list

# Install one skill
npx skills add mathwro/Skills --skill SKILL-NAME

# Install every skill
npx skills add mathwro/Skills --all
```

To install a skill for all projects and agents, add `--global`. To target particular agents, add one or more `--agent` options.

A single skill can also be installed directly:

```bash
npx skills add https://github.com/mathwro/Skills/tree/main/skills/SKILL-NAME
```

## Repository layout

```text
skills/
└── skill-name/
    ├── SKILL.md          # Required metadata and agent instructions
    ├── scripts/           # Optional executable helpers
    ├── references/        # Optional detailed documentation
    └── assets/            # Optional templates and static resources

templates/
└── SKILL.md.template     # Copy this when creating a skill
```

Each directory directly under `skills/` is a publishable skill. The directory name and the `name` field in its `SKILL.md` must match. Keep the required frontmatter valid and make the description specific enough for an agent to recognize when the skill applies.

## Create a skill

1. Copy `templates/SKILL.md.template` to `skills/<skill-name>/SKILL.md`.
2. Use a lowercase, hyphen-separated skill name.
3. Write the activation conditions and procedure in `SKILL.md`.
4. Put large or optional material in `references/`; keep `SKILL.md` focused.
5. Validate the skill with `skills-ref validate skills/<skill-name>` when `skills-ref` is installed.
6. List the repository before publishing:

   ```bash
   npx skills add . --list
   ```

See [CONTRIBUTING.md](CONTRIBUTING.md) for authoring rules.
