# Repository instructions

This repository publishes agent skills through skills.sh.

## Published skills

- Store every published skill in `skills/<skill-name>/`.
- Use lowercase kebab-case and make the directory name match the frontmatter
  `name`.
- Keep third-party skills installed for this project in `.agents/skills/`; do
  not publish them from `skills/`.
- For an installable skill dependency invoked at runtime, check availability at
  the first step that needs it. If neither it nor an accepted equivalent is
  available, stop before the dependent action and give the exact installation
  command; keep earlier independent steps runnable.

## Documentation

When adding, removing, or renaming a published skill, update the skill list and
installation examples in `README.md` in the same change.

## Language and attribution

Write skill content, repository documentation, commit messages, and pull
request titles and descriptions in English.

Attribute commits only to human contributors. Omit `Co-authored-by` trailers and
any other attribution to AI agents, coding assistants, or tools.

## Completion

Before completing a change:

1. Run `git diff --check`.
2. Run `npx skills@latest add . --list` and confirm that the discovered skill
   names exactly match the `Available skills` section in `README.md`.
3. Report any check that was not run or could not complete.
