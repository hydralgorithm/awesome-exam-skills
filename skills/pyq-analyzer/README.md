# pyq-analyzer

> Turn uploaded exam papers into actionable study intelligence.

Upload your previous year question papers. Get back pattern analysis, confidence scores, variant trends, and a prioritized study plan — as a formatted PDF or a clean chat summary.

Part of [awesome-exam-skills](../../README.md).

---

## What it does

Most students eyeball their PYQs and guess what's "important." This skill does it systematically:

- **Maps every question** across all uploaded papers to a chapter/topic
- **Extracts 6 pattern layers**: slot patterns, mark weights, question types, internal A/B/C splits, variant sub-types, and rotation cycles
- **Scores confidence** using fractions (e.g. `4/5 papers`) + recency tags so you can see both frequency and trend
- **Flags anomalies**: due topics (historically consistent, recently absent), emerging variants (new in latest paper), and unverified groupings
- **Outputs** either a detailed multi-page LaTeX PDF, a priority chat summary, or a one-page quick brief

Works for any subject — engineering, CSE, humanities, law, medicine, commerce.

---

## Installation

### Option 1: Claude.ai Projects (recommended)

1. Open [claude.ai](https://claude.ai) → create or open a **Project**
2. Under **Project Instructions**, paste the full contents of [`SKILL.md`](./SKILL.md)
3. Start a conversation, upload your PYQ PDFs, and ask Claude to analyze them

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

Drop `SKILL.md` into any tool that accepts Claude system prompts.

---

## Usage

1. Upload 3–10 PYQ papers (PDF or image format)
2. Claude auto-detects subject, institution, exam structure, and paper count
3. Claude asks two questions:
   - Do you have a syllabus? (optional — improves chapter mapping accuracy)
   - Which output type: Full PDF / Priority chat / Quick brief?
4. Claude processes silently (no narration) and delivers output

**Tip:** More papers = better patterns. 5+ papers recommended for reliable confidence scores. The skill warns you if sample size is low.

---

## Output types

| Type | Format | Best for |
|------|--------|----------|
| **A — Full Analysis** | LaTeX PDF (multi-page) | Deep prep, 5+ papers uploaded |
| **B — Priority Analysis** | Chat response | Quick decision-making |
| **C — Quick Brief** | Chat (one page) | Night-before cramming |

### Full Analysis PDF structure

```
Page 1  → Cover + disclaimer
Page 2  → Master summary table (all chapters at a glance)
Page 3+ → Per-chapter deep dive:
           • Internal split box (blue)  — Q2A→NUM→4 marks | Q2B→THEORY→3 marks
           • Variant breakdown (purple) — which specific numericals appeared, how often
           • Flags (orange)             — due alerts, emerging patterns
           • Plain-language summary     — "This chapter tests gear calculations and belt drives"
Last    → Priority tearout page (🔴 study first → ⚪ lowest priority)
```

### Quick Brief sample

```
⚠️ Based on 5 papers. Patterns only — not guarantees.

━━━━━━━━━━━━━━━━━━━━
STUDY IN THIS ORDER:
━━━━━━━━━━━━━━━━━━━━

🔴 DO FIRST — shows up every paper:
• Gear Trains: design for speed ratio + find unknown teeth
• Laplace Transforms: standard table + inverse using partial fractions

🟡 DO SECOND — very likely:
• Belt Drives: open/cross belt length + power transmitted

🟢 DO IF TIME ALLOWS:
• Cams: displacement diagrams (confirmed in 3/5, not in latest 2)

━━━━━━━━━━━━━━━━━━━━
LOWEST PRIORITY (based on data — your call):
• Clutches: appeared 1/5 papers, last seen 2021
━━━━━━━━━━━━━━━━━━━━
```

---

## Confidence scoring

The skill uses fractions, not percentages, because fractions make sample size visible:

- `5/5 papers` → you can see this is 5 papers confirming, not just "100%"
- `2/5 papers` → you can see this is thin data

Recency tags are layered on top:

| Tag | Meaning |
|-----|---------|
| `[confirmed in latest 2]` | Both recent papers had this |
| `[last seen: 2021]` | Historically appeared, absent recently |
| `[only in latest]` | New in most recent paper → ⚠️ EMERGING |
| `[not in latest 2]` | Fading pattern |

**Due flags** (🔔): A topic that appeared consistently then went absent. Could return — or the syllabus may have changed. The skill surfaces the data; you make the call.

---

## Supported exam types

| Type | Examples |
|------|---------|
| T1 Numerical-Technical | Engineering mechanics, thermodynamics, circuits, math |
| T2 Theory-Descriptive | History, law, management, literature, social sciences |
| T3 Mixed | Chemistry, commerce, some biology |
| T4 Code/Logic | Discrete math, algorithms, formal languages, TOC |
| T5 CSE | OS, DBMS, CN, DSA, compiler design, COA |

---

## Limitations

- Analysis quality scales with paper count. 1–2 papers = thin data (warned prominently)
- Cannot predict future exam content — only surfaces historical patterns
- Chapter mapping accuracy depends on question clarity. Ambiguous questions are flagged as ⚠️ UNVERIFIED GROUPING
- Full Analysis (PDF) requires Claude to have LaTeX execution capability (available in Claude.ai Projects with computer use, or API with tool use)

---

## Examples

See [`examples/README.md`](./examples/README.md) for sample outputs.

---

## Philosophy

> "Show raw data — let the student draw their own conclusions."

The skill never says "ignore this topic" or "this won't come." It says "lowest priority based on pattern data" and attaches a disclaimer. You're still the one making study decisions.
