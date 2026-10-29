---
name: write-readme
description: Write or revise a software README.md with clear feature explanations, concrete usage examples, and only the essentials needed to get started. Use when creating a project README or simplifying one that reads like a technical manual.
---

# Write README

Write for someone discovering the software: help them understand what it does,
whether it fits their needs, and how to try it. Organize the README around what
the reader can accomplish. Keep the depth appropriate to the intended users;
developer tools may need code examples, while an app may need a short walkthrough.

## Establish the facts

Read the existing README and inspect the relevant entry points, examples, tests,
and package or build metadata. Establish the software's purpose, audience, main
capabilities, and shortest usable starting path. Trace claims and example syntax
to the implementation or maintained project documentation. Describe available
behavior; distinguish planned functionality when the user asks to include it.

Use the requested language, or preserve the README's language when unspecified.
Ask only about gaps that materially affect the result and cannot be resolved
from the project. If information is unavailable, omit unsupported claims and
report the gap to the user rather than inserting invented details.

## Explain through use

Open with the software's name and a direct explanation of what it lets the
reader do. Make its purpose understandable before requiring installation or
background knowledge.

Choose the capabilities that explain the software's value and distinct uses.
For each, say what the user can do and show a small example or scenario when it
adds understanding. Include the outcome: output, a visible effect, or a brief
explanation of the result. Keep related features together so one example can
explain a complete task.

For example, replace an implementation-centered feature such as
"Extensible multi-format serialization pipeline" with a reader-facing
explanation such as "Export the same report as CSV for a spreadsheet or JSON
for another application." In a real README, follow this with the project's
actual command, API call, or UI action when useful.

Give the shortest complete path to a first useful result, including prerequisites
and setup only where required. Keep examples easy to adapt: explain necessary
inputs, make placeholders obvious, and include required imports or configuration.
Prefer one useful end-to-end example over several fragments that leave the
reader to assemble the workflow.

## Use a recognizable structure

Use this default order, adapting heading names to the README's language:

1. **Project name and introduction.** Explain what the software does, who it is
   for, and the problem it helps solve in a short opening paragraph.
2. **Features.** Explain the main things users can accomplish, with short
   examples or scenarios where they clarify the capability.
3. **Installation.** Give the prerequisites and recommended installation steps
   in execution order. Include essential setup and a way to confirm readiness.
   Prefer the simplest supported path; link to alternative installation methods.
   For hosted products, use **Access** instead and explain how to open the
   product and obtain any required account or credentials.
4. **Quick start.** Take the reader from the installed or accessible product to
   one useful result. Show the command, code, or UI actions and expected outcome;
   refer to the installation steps rather than repeating them.
5. **Usage examples.** Show additional common tasks that demonstrate distinct
   capabilities. Give each example a task-oriented label and enough context to
   use it. If the quick start covers the project's meaningful usage, omit this
   section; keep feature explanations concise rather than duplicating examples.

For a small project, combine installation and the first useful result under
**Getting started** when that reads more naturally. Preserve this reading path
even when sections are combined: understand, install or access, try, explore.

Add **Documentation**, **Limitations**, **Contributing**, **Support**, or
**License** only when useful for this project or required by its conventions.
Keep these sections brief, linking to existing resources where appropriate.

## Keep only what earns its place

- Use plain language, concrete verbs, and short paragraphs. Explain technical
  terms when the intended reader needs them.
- Favor specific capabilities over promotional claims and generic adjectives.
- Include a limitation or caveat when it changes whether or how someone can use
  the software. Preserve required notices and relevant existing project links.
- Link to deeper documentation for exhaustive API references, configuration
  catalogs, architecture, and contributor workflows when those resources exist.
  Keep technical detail inline when it is essential to the first useful result.
- Remove repetition and sections that add no project-specific help. Add badges,
  tables of contents, roadmaps, or other supporting material only when requested
  or when they serve a clear need in this project.

When revising, retain useful facts and working links while reshaping the text
around reader tasks. Keep changes within the requested documentation scope.

## Check the result

Read the README as a newcomer: can they explain what the software does, recognize
its main uses, and reach a first useful result without guessing missing steps?
Check example syntax and links against the project; run examples when practical
and safe. Tell the user which examples could not be executed. Remove any passage
that neither explains a capability nor helps the reader choose or use the software,
apart from required notices. Deliver the edited README and a brief change summary.
