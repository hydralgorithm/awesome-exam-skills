# Contributing to awesome-exam-skills

Thank you for your interest in contributing. This document covers everything you need to know — whether you're fixing a bug in an existing skill, improving documentation, or proposing a new skill from scratch.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Ways to Contribute](#ways-to-contribute)
- [Reporting Issues](#reporting-issues)
- [Improving an Existing Skill](#improving-an-existing-skill)
- [Proposing a New Skill](#proposing-a-new-skill)
- [Skill Folder Structure](#skill-folder-structure)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Quality Standards](#quality-standards)

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md). By participating, you are expected to uphold it.

---

## Ways to Contribute

| Type | How |
|------|-----|
| Bug report | Open a GitHub issue |
| Edge case report | Open a GitHub issue with the `edge-case` label |
| Documentation fix | Open a PR directly |
| Improve an existing skill | Fork → edit → test → PR |
| New exam type support | Open an issue first, then PR |
| New skill proposal | Open an issue with `skill-proposal` label |

---

## Reporting Issues

Open a GitHub issue. Please include:

- **Skill name** (e.g. `pyq-analyzer`)
- **Subject area** you were working with
- **Number of papers / files uploaded**
- **Output type** selected (A, B, or C where applicable)
- **What went wrong** — expected vs actual behavior
- A **sample of the problematic output** if possible (anonymize institution/personal details)

The more specific you are, the faster it can be diagnosed.

---

## Improving an Existing Skill

1. Fork the repository
2. Make your changes to the relevant `SKILL.md`
3. Test your changes against **at least 2–3 real exam papers or materials** — not synthetic examples
4. Update the skill's `README.md` and `examples/README.md` if your change affects behavior or output format
5. Open a pull request with:
   - A clear description of what changed and why
   - What subject / exam type you tested on
   - Before/after comparison of relevant output sections

### What changes are welcome

- Fixes to chapter mapping logic for specific subject areas
- New exam type classifications beyond the existing T1–T5
- Output format improvements (PDF layout, chat response structure)
- Token efficiency improvements that don't compromise output quality
- Edge case handling for unusual paper formats

### What changes are not welcome

- Changes that haven't been tested against real exam materials
- Generic LLM prompt style tweaks without a clear rationale
- Changes that cause a skill to make predictions or guarantees about future exam content

---

## Proposing a New Skill

**Open an issue before building.** Use the `skill-proposal` label.

Your proposal should answer:

1. **What problem does this skill solve?** Be specific — what does a student need to do that they can't do well with a plain Claude conversation?
2. **What inputs does it take?** (uploaded files, pasted text, structured data, etc.)
3. **What does it output?** (chat response, PDF, table, etc.)
4. **Which subjects or exam types does it apply to?** Is it general or subject-specific?
5. **Concrete example** — describe a real use case: "A student uploads their handwritten notes and wants back X"

Proposals that get a green light will be tagged `approved` on the issue. You can then build and submit a PR.

---

## Skill Folder Structure

Every skill lives under `skills/<skill-name>/` and must contain exactly these files:

```
skills/
└── your-skill-name/
    ├── SKILL.md        ← The skill itself (the Claude system prompt)
    ├── README.md       ← Documentation
    └── examples/
        └── README.md   ← At least 1–2 real sample outputs
```

### SKILL.md

Must begin with YAML frontmatter:

```yaml
---
name: your-skill-name
description: >
  What does this skill do? When should it be used?
  What inputs does it take? What does it output?
  Two to four sentences.
---
```

The rest of the file is the skill prompt itself — phases, rules, output formats, edge cases.

### README.md (skill-level)

Should cover:
- What the skill does (brief)
- Installation (link to root Getting Started or repeat the steps)
- Usage instructions specific to this skill
- Output types / formats
- Limitations
- Link to `examples/`

### examples/README.md

At least one complete real example showing:
- Subject and exam type
- Input description (number of papers, institution if comfortable sharing)
- Output type selected
- The actual output produced

Anonymize institution names or personal details if needed. Synthetic examples are not accepted.

---

## Pull Request Guidelines

- **One skill or one issue per PR** — don't bundle unrelated changes
- **Title format**: `[skill-name] brief description` (e.g. `[pyq-analyzer] add T6 exam type for professional certifications`)
- **PR description** must include what was changed, why, and test evidence
- PRs that touch `SKILL.md` without documented testing will not be merged
- Keep `SKILL.md` files plain text — no binary files, no external dependencies

---

## Quality Standards

All contributions are reviewed against these standards before merging:

- **Tested on real materials** — not synthetic or AI-generated exam papers
- **Follows the design philosophy** — show data, don't make predictions, let the student decide
- **No regressions** — existing functionality described in `README.md` still works
- **Documentation updated** — if behavior changes, docs change too
- **Honest about limitations** — if a skill doesn't work well for certain subjects or edge cases, that belongs in the `README.md`
