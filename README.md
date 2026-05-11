<div align="center">

# awesome-exam-skills

**A curated collection of Claude skills for exam preparation and academic intelligence.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![Skills](https://img.shields.io/badge/skills-1-blue.svg)](#skills)

</div>

---

Each skill in this repository is a structured system prompt engineered to give Claude repeatable, well-defined exam-prep capabilities. Drop any skill into Claude via Projects, the API, or any compatible skill runner — no setup beyond copy-paste.

## Skills

| Skill | Description | Exam Types | Status |
|-------|-------------|------------|--------|
| [pyq-analyzer](./skills/pyq-analyzer/) | Analyzes previous year question papers to extract patterns, score confidence, flag anomalies, and produce a prioritized study plan | T1–T5 (all subjects) | ![Stable](https://img.shields.io/badge/status-stable-brightgreen) |

> See [ROADMAP.md](./ROADMAP.md) for planned additions.

---

## Getting Started

Every skill follows the same installation pattern.

### Claude.ai Projects (recommended)

1. Navigate to [claude.ai](https://claude.ai) and open or create a **Project**
2. Under **Project Instructions**, paste the full contents of the skill's `SKILL.md`
3. Follow the skill-specific usage instructions in its `README.md`

### API

```python
import anthropic

with open("skills/pyq-analyzer/SKILL.md", "r") as f:
    skill = f.read()

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=4096,
    system=skill,
    messages=[{"role": "user", "content": "I've uploaded 5 DBMS papers. Analyze them."}]
)
```

### Other Environments

Any tool that accepts a Claude system prompt works — agent frameworks, custom UIs, local runners. Drop the `SKILL.md` content in as the system prompt.

---

## Repository Structure

```
awesome-exam-skills/
├── README.md               ← You are here
├── CONTRIBUTING.md         ← Contribution guidelines
├── ROADMAP.md              ← Planned skills and proposals
├── CODE_OF_CONDUCT.md      ← Community standards
├── LICENSE
└── skills/
    └── pyq-analyzer/
        ├── SKILL.md        ← The skill (paste this into Claude)
        ├── README.md       ← Usage, examples, limitations
        └── examples/
            └── README.md   ← Sample outputs
```

New skills each get their own folder under `skills/`. See [CONTRIBUTING.md](./CONTRIBUTING.md) for the required structure.

---

## Design Philosophy

All skills in this repository are built around a single principle:

> **Show raw data. Let the student decide.**

No skill will tell a student to ignore a topic or assert that something will or won't appear. Skills surface historical patterns, quantify confidence, and flag anomalies — then get out of the way. The student makes the call.

---

## Contributing

Contributions are welcome — whether you're reporting an edge case, improving an existing skill, or proposing an entirely new one.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for full guidelines. The short version:

- **Bug reports / edge cases** → open a GitHub issue
- **Improvements to an existing skill** → fork, edit `SKILL.md`, test on real papers, open a PR
- **New skill proposals** → open an issue with the `skill-proposal` label before building

---

## License

[MIT](./LICENSE) — use it, fork it, modify it, build on it.

---

<div align="center">

Built by [Abdul Fattah](https://github.com/hydralgorithm) · [LinkedIn](https://linkedin.com/in/im-abdul-fattah)

If this project helped you, consider starring the repository.

</div>
