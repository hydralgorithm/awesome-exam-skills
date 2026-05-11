# Roadmap

Skills planned for this repository. This list is not a commitment — it reflects what's being considered based on what would actually be useful.

---

## In progress / next up

None currently. `pyq-analyzer` is the first skill; getting it stable and well-tested before adding more.

---

## Planned skills

| Skill (tentative name) | What it would do |
|------------------------|-----------------|
| `syllabus-mapper` | Take a raw syllabus PDF and break it into weighted study units based on typical exam coverage patterns |
| `answer-reviewer` | Review a student's written answer against the question and expected marks — flag missing key points, over-explanation, and structure issues |
| `formula-sheet-builder` | Extract all formulas from uploaded notes/textbooks, classify by chapter, and generate a printable reference sheet |
| `time-allocator` | Given an exam schedule and a list of subjects with confidence levels, output a day-by-day prep plan |

---

## How to suggest a skill

Open a GitHub issue with the label `skill-proposal`. Include:
- What problem the skill solves
- What inputs it would take (uploaded files, text, etc.)
- What outputs it would produce
- Which subjects/exam types it applies to

Good proposals include a concrete example: "I want to upload my notes and get back X."

---

## What makes a good skill for this repo

- Solves a real, recurring exam-prep problem — not a one-off
- Works across multiple subjects (or has a clear classification system for subject-specific behavior)
- Produces structured, actionable output — not just "here are some tips"
- Follows the repo's philosophy: show data, let the student decide
