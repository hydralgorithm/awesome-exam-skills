# awesome-exam-skills

> A collection of Claude skills for exam preparation and academic intelligence.

Each skill is a standalone system prompt you drop into Claude (via Projects, API, or any skill runner) to give it structured, repeatable exam-prep capabilities.

---

## Skills

| Skill | What it does | Status |
|-------|-------------|--------|
| [pyq-analyzer](./skills/pyq-analyzer/) | Turns uploaded PYQ papers into pattern analysis, confidence scores, and a prioritized study plan | ✅ Stable |

More skills coming. See [ROADMAP.md](./ROADMAP.md).

---

## How to use any skill

### Option 1: Claude.ai Projects (recommended)

1. Open [claude.ai](https://claude.ai) → create or open a **Project**
2. Under **Project Instructions**, paste the full contents of the skill's `SKILL.md`
3. Start a conversation and follow the skill's usage instructions

### Option 2: API system prompt

```python
import anthropic

with open("skills/pyq-analyzer/SKILL.md", "r") as f:
    skill = f.read()

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=4096,
    system=skill,
    messages=[
        {"role": "user", "content": "I've uploaded 5 DBMS PYQ papers. Analyze them."}
    ]
)
```

### Option 3: Any Claude-compatible skill runner

Drop any `SKILL.md` into tools that accept Claude system prompts — agent frameworks, custom UIs, etc.

---

## Repository structure

```
awesome-exam-skills/
├── README.md               ← You are here
├── CONTRIBUTING.md         ← How to contribute a skill or improvement
├── ROADMAP.md              ← Planned skills
├── LICENSE
└── skills/
    └── pyq-analyzer/
        ├── SKILL.md        ← The skill itself (paste this into Claude)
        ├── README.md       ← What it does, how to use it, limitations
        └── examples/
            └── README.md   ← Sample outputs
```

As new skills are added, each gets its own folder under `skills/`.

---

## Philosophy

These skills are built around one idea: **show raw data, let the student decide.**

No skill in this repo will tell you to ignore a topic or guarantee something will appear. They surface patterns, score confidence, and flag anomalies — then get out of the way.

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). New skills, edge case reports, and output format improvements are all welcome.

---

## Author

Built by [Abdul Fattah](https://github.com/hydralgorithm) · [LinkedIn](https://linkedin.com/in/im-abdul-fattah)

If this helped you, a ⭐ on the repo goes a long way.
