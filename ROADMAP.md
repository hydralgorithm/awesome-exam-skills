# Roadmap

This document tracks the current state of the project and what's being considered for future development.

---

## Current Status

| Skill | Version | Status |
|-------|---------|--------|
| `pyq-analyzer` | v1.0 | Stable — accepting bug reports and edge case contributions |

---

## Under Consideration

These are not commitments. They're directions being evaluated based on real student needs. If any of these would be useful to you, upvote or comment on the relevant GitHub issue.

| Skill (tentative) | Problem it solves | Inputs | Outputs |
|-------------------|-------------------|--------|---------|
| `syllabus-mapper` | Raw syllabus PDFs are hard to turn into a study plan — topics aren't weighted by exam relevance | Syllabus PDF + optionally PYQ set | Weighted topic list, suggested time allocation per unit |
| `answer-reviewer` | Students don't know which key points they're missing in written answers | Question + student's answer + mark scheme (optional) | Feedback on missing points, over-explanation, structure issues; estimated marks |
| `formula-sheet-builder` | Formulas are scattered across notes and textbooks | Uploaded notes / textbook chapters | Consolidated formula sheet organized by chapter, ready to print |
| `time-allocator` | Students struggle to build a realistic revision schedule given multiple subjects and varying confidence levels | Exam schedule + subject list + self-assessed confidence per subject | Day-by-day revision plan |

---

## Proposing a New Skill

If you have a skill idea that isn't listed here, open a GitHub issue with the `skill-proposal` label.

A strong proposal includes:
- The specific student problem it solves (not solvable by just asking Claude directly)
- What inputs it takes
- What structured output it produces
- Whether it generalizes across subjects or is subject-specific
- A concrete example: "A student uploads X and expects back Y"

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full proposal process.

---

## What's Not on the Roadmap

- Skills that generate practice questions from scratch (generative, not analytical — different problem)
- Skills that require internet access to function (all skills must work with uploaded files only)
- Subject-specific skills that only work for one university's paper format
