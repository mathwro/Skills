# Contributing skills

## Add a skill

Create one directory per skill under `skills/`:

```text
skills/<skill-name>/
└── SKILL.md
```

Use `templates/SKILL.md.template` as the starting point. The `name` frontmatter value must:

- contain only lowercase letters, numbers, and single hyphens;
- be 1–64 characters long; and
- exactly match the parent directory name.

`description` is also required and may be up to 1024 characters. Describe both what the skill does and when an agent should use it. Include concrete terms users are likely to mention.

## Write effective instructions

- Start with the task the skill enables and its activation conditions.
- Give the agent a deterministic procedure, including required checks.
- State important boundaries, failure modes, and security constraints.
- Include examples only when they clarify a non-obvious decision.
- Keep `SKILL.md` under 500 lines when possible.
- Move detailed reference material to `references/` and link it with a relative path.
- Keep scripts self-contained and document their runtime requirements.
- Never include secrets, credentials, or destructive commands without an explicit safety boundary.

## Validate locally

Install the reference validator according to the [skills-ref instructions](https://github.com/agentskills/agentskills/tree/main/skills-ref), then validate each skill:

```bash
skills-ref validate skills/<skill-name>
```

Check what the Skills CLI discovers from the repository root:

```bash
npx skills add . --list
```

The CLI discovers skills from `skills/` and accepts either the repository shorthand (`OWNER/REPOSITORY`) or a direct skill path on GitHub.
