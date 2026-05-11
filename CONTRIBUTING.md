# Contributing

Contributions welcome — whether you're improving an existing skill or proposing a new one.

---

## Types of contributions

### Improving an existing skill

- **Bug reports** — describe the subject, how many papers you uploaded, which output type you used, and what went wrong
- **Edge cases** — exam formats or subjects the skill didn't handle well
- **Output format improvements** — suggestions for LaTeX PDF layout, chat format clarity, etc.
- **New exam type support** — if your subject doesn't fit the existing classification, open an issue describing it

### Proposing or building a new skill

Open a GitHub issue with the label `skill-proposal` before building. Include:
- What problem it solves
- What inputs it takes
- What outputs it produces
- Which subjects or exam types it applies to

If the proposal gets a green light, fork the repo and build it using the structure below.

---

## Skill folder structure

Every skill lives in `skills/<skill-name>/` and contains exactly:

```
skills/
└── your-skill-name/
    ├── SKILL.md        ← The skill itself — the system prompt Claude runs
    ├── README.md       ← What it does, installation, usage, limitations
    └── examples/
        └── README.md   ← At least 1-2 real sample outputs with context
```

`SKILL.md` should have YAML frontmatter at the top:

```yaml
---
name: your-skill-name
description: >
  One paragraph. What does this skill do? When should it be used?
  What inputs does it take? What does it output?
---
```

---

## What makes a good skill for this repo

- Solves a real, recurring exam-prep problem — not a one-off
- Works across multiple subjects or has a subject classification system
- Produces structured, actionable output
- Follows the repo's philosophy: show data, let the student decide
- Has been tested against real exam materials (not synthetic examples)

---

## How to submit

1. Fork the repo
2. Create your skill folder under `skills/`
3. Test against at least 2–3 real exam papers or materials
4. Open a PR with:
   - What the skill does
   - What subject/exam type you tested on
   - Sample output in the PR description or in `examples/README.md`

---

## What's not useful

- Submissions that haven't been tested against real materials
- Generic LLM prompt tweaks without a clear rationale
- Skills that duplicate what an existing skill already covers
