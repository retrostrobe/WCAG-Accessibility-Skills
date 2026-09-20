---
name: wcag-accessibility-skills
description: Audit HTML, local pages, or approved public URLs against WCAG 2.1 and 2.2. Use for /wcag-audit, criterion checks, repair proposals, accessibility reports, and delegation. Preserve manual review; never treat a clean static scan as a conformance or legal claim.
metadata:
  short-description: WCAG audit and manual-review skill
---

# WCAG Accessibility Skills

Use the local CLI to collect reproducible source-level accessibility evidence. Keep the report JSON and its `manualReview` queue intact in summaries, tickets, and handoffs.

## Command routing

Use slash commands in an agent conversation or the matching `wcag-*` executable in a shell.

| Request | Command |
|---|---|
| Audit one URL, file, or HTML fragment | `/wcag-audit <target> [--level A|AA|AAA] [--version 2.1|2.2] [--format json|markdown|html] [--output <path>] [--fail-on low|medium|high|critical|none]` |
| Inspect one WCAG 2.2 criterion | `/wcag-check <criterion>` |
| Store project defaults | `/wcag-configure <A|AA|AAA> <2.1|2.2>` |
| Produce a bounded repair proposal | `/wcag-fix <issue-id> --report <report.json>` |
| Render an existing report | `/wcag-report <report.json> [--format json|markdown|html] [--output <path>] [--fail-on low|medium|high|critical|none]` |
| Create a repair-task payload | `/wcag-delegate <issue-id> --report <report.json>` |
| Summarize a report | `/wcag-status --report <report.json>` |

From this repository, invoke the CLI with:

```bash
node bin/wcag-skill.js /wcag-audit ./page.html --level AA --version 2.2 --format json --output reports/audit.json
```

Read `README.md` for the complete CLI contract and examples.

## Operating boundaries

- Treat JSON as canonical; Markdown and HTML are renderings.
- Scan only supplied HTML, local files in scope, or approved public URLs. Do not scan authenticated or unapproved targets.
- A static scan cannot establish legal compliance, a WCAG conformance claim, meaningful alternatives, keyboard usability, rendered contrast, focus visibility, screen-reader interaction, captions, or full-process usability.
- `/wcag-fix` and `/wcag-delegate` propose work but never authorize file edits, tickets, pull requests, or production changes.
- Exit code `0` means no finding reached the selected threshold, `1` means valid finding evidence reached it, and `2` means an input or operational error. Never call exit code `2` a pass.

## Repair workflow

1. Run `/wcag-audit` and retain the JSON report.
2. Use `/wcag-check` when the success criterion needs context.
3. Use `/wcag-fix` for the proposed repair and evidence.
4. Make the smallest repair only with host authorization.
5. Re-run the same audit and complete the criterion-specific manual tests from `manualReview`.

Do not close work merely because a static finding disappears.
