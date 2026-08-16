# Skills for Software Engineers

Reusable agent skills distilled from production software-engineering work. The
skills are small, focused, and designed to work across different projects and
coding agents.

## Works well with Matt Pocock's skills

These skills are designed to complement
[Matt Pocock's Skills for Real Engineers](https://github.com/mattpocock/skills).
They follow the same preference for small, focused, and composable engineering
workflows, and can be installed alongside his collection. They remain
independent: Matt Pocock's skills are not required to use this repository.

## Available skills

### `to-docs`

Turn decisions made during implementation, review, or design work into durable
project guidance. The skill separates project-wide rules from local fixes,
already-documented guidance, and patterns that are not mature enough to adopt.

[View the skill](skills/to-docs/SKILL.md)

`to-docs` uses Matt Pocock's `writing-for-agents` skill when applying approved
documentation changes. The dependency is maintained separately and is not
installed automatically by skills.sh.

## Installation

Install the required `writing-for-agents` skill:

```bash
npx skills@latest add mattpocock/skills --skill writing-for-agents
```

Then install `to-docs`:

```bash
npx skills@latest add asterixcapri/skills --skill to-docs
```

To browse all the skills in this repository instead, run:

```bash
npx skills@latest add asterixcapri/skills
```
