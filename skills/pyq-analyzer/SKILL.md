---
name: pyq-analyzer
description: >
  Deep PYQ (Previous Year Question) pattern analysis skill. Analyzes uploaded exam papers
  to extract structural and content patterns, confidence scores, variant trends, and priority
  rankings. Produces Full Analysis (LaTeX PDF), Priority Analysis (chat), or Quick Brief (chat).
  Works for any subject — engineering, CSE, humanities, law, medicine, commerce.
  Use when user uploads PYQ papers and wants exam pattern analysis, topic prioritization,
  or study strategy based on historical question data.
---

# PYQ ANALYZER SKILL

## OVERVIEW

Analyze previous year question papers. Extract patterns. Score confidence. Rank priorities.
Output depends on analysis type chosen by user.

Core philosophy:
- Latest papers carry more weight than older ones
- Structural patterns (which chapter, which slot, which marks) are more reliable than content patterns (which specific topic)
- Show raw data — let student draw own conclusions
- Never say "ignore this topic" — say "lowest priority based on data"
- Always attach disclaimer: analysis = historical trends, not guarantees

---

## TOKEN EFFICIENCY

This skill is token-intensive by nature. The following rules keep token usage controlled WITHOUT compressing reasoning quality.

**Rule 1 — Silent processing.**
Phases 1 through 4 (intake, chapter mapping, pattern extraction, confidence scoring) are internal.
Do NOT narrate these steps to the user. Do NOT show intermediate reasoning, grouping decisions,
pattern counting, or fraction calculations as they happen.
Process silently. Surface only final results in Phase 5.

**Rule 2 — No reasoning compression.**
Research shows compressing chain-of-thought reasoning steps degrades accuracy on
pattern recognition and classification tasks. Do NOT apply terse/compressed style to
any decision-making step — chapter grouping, variant classification, flag generation,
confidence scoring. These must run at full reasoning depth internally.

**Rule 3 — Output compression is safe.**
Final output to user (Phase 5) can drop filler words, hedging, pleasantries.
Keep output tight. No "I have completed the analysis and would now like to present..."
Just present it. No preamble. No postamble.

**Rule 4 — No progress narration.**
Do not say "Now I am moving to Phase 3..." or "I have identified X chapters..."
during processing. One message when intake is done (the questions).
Silence during processing. One message or PDF as final output.

**Rule 5 — Web search only when needed.**
Do not web search every ambiguous question. Only search when auto-grouping
genuinely fails and chapter cannot be inferred from context.
Unnecessary searches are the single biggest token drain in this skill.

---

## PHASE 1 — INTAKE

### Step 1: Auto-detect from uploaded papers

Before asking anything, read all uploaded papers and extract:
- Subject name (from paper header)
- Institution name
- Exam type (end-semester, makeup, competitive, entrance, certification)
- Number of papers (count automatically — never ask)
- Year/date of each paper (sort chronologically, latest = highest priority)
- Total marks per paper
- Question structure (how many questions, sub-parts, marks per question)

### Step 2: Determine exam type

Classify into one of five types based on question content:

| Type | Description | Key signals |
|------|-------------|-------------|
| **T1: Numerical-technical** | Engineering, physics, math, economics | Formulas, calculations, units, data given |
| **T2: Theory-descriptive** | History, law, management, humanities | Essays, explain, discuss, case studies |
| **T3: Mixed** | Chemistry, commerce, some biology | Some calculation + mostly theory |
| **T4: Code/logic** | Discrete math, algorithms, formal languages | Proofs, trace, derive, complexity |
| **T5: CSE** | CS engineering subjects | Mix of define/explain + trace/diagram + code + numerical (scheduling, subnetting) + proof |

If unclear from papers alone → web search subject name to determine type.

### Step 3: Ask only what can't be inferred

After auto-detection, ask ONLY questions that couldn't be answered from the papers.

Typical questions that usually CAN be inferred (skip these if obvious):
- Subject name
- Exam type
- Number of papers

Questions that usually CANNOT be inferred (ask these):
- **Syllabus/chapter list** — "Do you have a syllabus or chapter list? If yes, share it. If no, I'll auto-group using web search."
- **Analysis type** — ALWAYS ask this, never infer:

```
What kind of analysis do you want?

A) Full Analysis
   Complete pattern breakdown per chapter. Structural + content confidence scores.
   All variants with fractions and recency tags. Due flags, emerging flags.
   Priority ranking. Output: LaTeX PDF (detailed, multi-page).

B) Priority Analysis  
   Only high-confidence patterns. Most likely question types per chapter.
   Variants collapsed to top pick + backups. Output: Chat response.
   Option to export as PDF at the end.

C) Quick Brief
   One page. Ranked action list only. No explanations, no fractions.
   Just: study this → then this → then this.
   Output: Chat only.
```

Ask both questions in one message. Do not bombard — one message, two questions max.

---

## PHASE 2 — CHAPTER MAPPING

### Step 1: Group questions by chapter

For each question across all papers:
- Read question text
- Assign to a chapter/topic group

Priority order for grouping:
1. **Syllabus provided** → use it as ground truth. Map each question to its syllabus chapter.
2. **No syllabus, question is clear** → auto-group by topic keywords in question text.
3. **No syllabus, question is ambiguous** → web search the topic to identify which chapter it belongs to before grouping.
4. **Web search also fails** → flag question as ⚠️ UNVERIFIED GROUPING. Include in analysis but note uncertainty. Dampen confidence scores for that chapter.

### Step 2: Build the question map

For every question create a record:

```
Paper: [year/date]
Question: [slot — e.g. Q2A]
Marks: [number]
Chapter: [assigned chapter]
Type: [NUM / THEORY / JUSTIFY / IDENTIFY+EXPLAIN / DESIGN / PROOF / DIAGRAM/TRACE / CODE / DEFINITION / CASE-STUDY]
Content summary: [1 line description of what was asked]
Variant: [sub-type within type, if applicable]
```

Sort all records by paper date, latest first.

---

## PHASE 3 — PATTERN EXTRACTION

Run all six pattern layers on the question map.

### Layer 1: Slot Pattern
Which chapter consistently appears in which question slot (Q1, Q2, Q3 etc.)
- Count how many papers place this chapter in this slot
- Note: if slot varies across papers, record all slots seen

### Layer 2: Mark Pattern
What mark weight does this chapter consistently receive
- Record marks per paper for this chapter
- Note if marks changed between older and newer papers (recency rule applies)

### Layer 3: Type Pattern
What question type(s) appear for this chapter
- Count NUM / THEORY / JUSTIFY / IDENTIFY+EXPLAIN etc. per chapter
- Separate structural type (this chapter always has a numerical) from content type (which specific numerical)

### Layer 4: Internal Split Pattern
Within a chapter's question slot, what is the A/B/C split
- Example: Q2 always = A(belt NUM 4 marks) + B(gear NUM 3 marks) + C(theory 3 marks)
- This is the most valuable pattern — capture it explicitly
- Record the split as a template: [SubQ → Type → Marks]

### Layer 5: Variant Pattern
Within each question type, what specific sub-variants appear
- Example: gear numerical has 3 variants: design for min/max speed, find unknown teeth, find speed+direction+centre distance
- Count each variant across papers
- Apply variant rules (see below)

### Layer 6: Rotation Pattern
Topics that rotate within a slot across papers
- Example: Q1C alternates between accessories-reasons and justify-statements
- Record all rotations seen
- Note which rotation appeared most recently

---

## PHASE 4 — CONFIDENCE SCORING

### Structural Confidence
Measures: how reliably does this chapter appear in this slot with these marks

Display as: **X/Y papers follow this pattern**

Recency rule:
- Latest 2 papers BOTH confirm pattern → add note: [confirmed in latest 2]
- Latest 2 papers BOTH contradict pattern → add note: [contradicted in latest 2] — treat as unreliable regardless of overall fraction
- Latest paper confirms, older paper contradicts → add note: [latest confirms, older papers mixed]

### Content Confidence
Measures: how reliably does a specific question type/variant appear

Display as: **X/Y papers** + recency tag

Recency tags:
- `[confirmed in latest 2]` — appeared in both most recent papers
- `[only in latest]` — new in most recent paper → flag as ⚠️ EMERGING
- `[not in latest 2]` — absent from both recent papers → fading pattern
- `[last seen: YEAR]` — most recent appearance year
- `[last seen: 2+ years ago]` — historically present, recently absent → check for 🔔 DUE flag

### Due Flag Logic
A chapter/variant gets a 🔔 DUE flag when ALL of:
- Appeared in 3+ of the older papers
- Absent from the 2 most recent papers
- No syllabus change detected that would explain absence

Due flag message: "🔔 DUE — consistent historically, absent in recent papers. Could return."

### Variant Rules
- If only 1 variant seen: show it + note "only variant observed"
- If 2–3 variants: show all with fractions
- If 4+ variants: show top 3 by frequency + "and [N] other variants observed"
- If a variant appeared only once and only in the oldest paper: low weight, note it

### Sample Size Warning
Always show: "⚠️ Based on [N] papers."
If N < 4: add "Low sample — patterns may not be reliable. Treat fractions as directional, not statistical."

---

## PHASE 5 — OUTPUT

### OUTPUT TYPE A: FULL ANALYSIS (LaTeX PDF)

Generate a complete LaTeX document. Compile to PDF.

**Page 1 — Cover**
- Subject name (large, centered)
- Institution name
- Papers analyzed: [N papers, date range]
- Analysis generated: [date]
- Disclaimer box:
  > "This analysis is based on historical question paper patterns only. It does not predict future exam content. All fractions represent past frequency — not probability. Study decisions remain yours."

**Page 2 — Master Summary Table**
One table, entire exam at a glance:

```
| Chapter | Slot | Total Marks | Structural | Dominant Type | Last Seen |
```

Sorted by structural confidence descending (highest confidence first).

**Page 3+ — Per Chapter Deep Dive**

One section per chapter. Each section contains:

1. **Chapter Header Bar** — chapter name + slot + total marks + structural fraction

2. **Internal Split Box** (blue) — the A/B/C template:
```
Q[slot]A → [Type] → [Marks] → [X/Y papers] [recency tag]
Q[slot]B → [Type] → [Marks] → [X/Y papers] [recency tag]
Q[slot]C → [Type] → [Marks] → [X/Y papers] [recency tag]
```

3. **Variant Breakdown Box** (purple) — per question in the split:
```
[Question type] variants:
- Variant name: X/Y papers [recency tag]
- Variant name: X/Y papers [recency tag]
  ⚠️ EMERGING — appeared only in [year]
```

4. **Flags Box** (orange) — due flags, emerging flags, unverified grouping warnings

5. **Rotation Note** (if applicable) — what rotates within this chapter's theory/justify slot

**Last Page — Priority Action List**
Tearout page. Pulled from all chapters. Format:

```
PRIORITY ORDER — Study in this sequence:

🔴 GUARANTEED (structural X/X):
1. [Chapter] — [what to prepare] — [marks at stake]
2. ...

🟡 HIGH PROBABILITY (structural X/X, content confirmed recent):
3. [Chapter] — [what to prepare]
...

🟢 MODERATE (structural confirmed, content varies):
...

⚪ LOWEST PRIORITY (based on pattern data):
- [Chapter/variant]: [fraction] [last seen year]
[disclaimer attached]
```

#### LaTeX Color Scheme:
- Blue boxes → structural/split data
- Purple boxes → variant breakdowns
- Orange boxes → flags (due, emerging, unverified)
- Red → guaranteed high priority items
- Green → caveman summary per chapter (2 lines, plain language, what this chapter tests)

---

### OUTPUT TYPE B: PRIORITY ANALYSIS (Chat)

Chat response. Structured but no LaTeX.

Format per chapter:
```
## [Chapter Name] | Slot: Q[X] | Marks: [N] | [fraction] papers

Structure: Q[X]A → [type+marks] | Q[X]B → [type+marks] | Q[X]C → [type+marks]

Most likely content:
- Q[X]A: [variant/topic] — [fraction] [recency tag]
- Q[X]B: [variant/topic] — [fraction] [recency tag]  
- Q[X]C: [variant/topic] — [fraction] [recency tag]

⚠️ Flags: [any due/emerging flags]
```

End with priority ranking list same as tearout page above.

Offer PDF export at the end: "Want this as a formatted PDF?"

---

### OUTPUT TYPE C: QUICK BRIEF (Chat)

One page max. No fractions. No explanations. Pure action.

```
⚠️ Based on [N] papers. Patterns only — not guarantees.

━━━━━━━━━━━━━━━━━━━━
STUDY IN THIS ORDER:
━━━━━━━━━━━━━━━━━━━━

🔴 DO FIRST — shows up every paper:
• [Chapter]: [what specifically to prepare]
• [Chapter]: [what specifically to prepare]

🟡 DO SECOND — very likely:
• [Chapter]: [what to prepare]

🟢 DO IF TIME ALLOWS:
• [Chapter]: [what to prepare]

━━━━━━━━━━━━━━━━━━━━
LOWEST PRIORITY (based on data — your call):
• [Topic]: not seen in recent papers
━━━━━━━━━━━━━━━━━━━━
```

---

## EDGE CASES

| Situation | Handling |
|-----------|----------|
| Only 1-2 papers uploaded | Run analysis + attach prominent sample size warning. All fractions shown as X/2 — student can see how thin the data is. |
| Papers from different syllabi/institutions | Flag this upfront. Group patterns only from same syllabus. Cross-institution patterns noted separately as "observed across institutions." |
| Subject completely unfamiliar to AI | Web search subject + syllabus structure before attempting chapter mapping. If still unclear, show grouping attempt and note uncertainty. |
| Question type not in T1-T5 classification | Default to T2 (theory-descriptive) as safest fallback. Note the classification was defaulted. |
| Contradicting patterns latest vs older | Latest wins. Old pattern noted as "previously consistent, now changed." |
| Question slot structure changes between papers (e.g. older papers had 2 sub-questions, newer have 3) | Use latest paper's structure as reference template. Note the change. |
| User uploads syllabus mid-analysis | Re-run chapter mapping with syllabus. Re-derive patterns. |
| No question type fits standard types | Create a custom type label from question text. Use it consistently across all papers for that chapter. |

---

## EXAM TYPE SPECIFIC LENSES

### T1: Numerical-Technical
- Primary split to detect: numerical vs theory vs justify per chapter
- Variant focus: which formula/approach, what data is given vs derived
- Key pattern: mark weight strongly correlates with numerical complexity
- Watch for: identify-then-explain questions (indirect clues pointing to a concept)

### T2: Theory-Descriptive  
- Primary split: long answer vs short answer vs definition vs case study
- Variant focus: which specific topics rotate within each slot
- Key pattern: compulsory vs optional question structure
- Watch for: topic rotation cycles (topic appears every alternate year)

### T3: Mixed
- Apply T1 lens to numerical questions, T2 lens to theory questions separately
- Note the numerical-to-theory ratio per chapter — if it shifts in recent papers, flag it

### T4: Code/Logic
- Primary split: proof vs trace vs derive vs complexity analysis vs construct (DFA/grammar etc.)
- Variant focus: which algorithm/data structure/formal system appears
- Key pattern: construction questions (draw this, build this) often paired with explanation

### T5: CSE
- Primary split: define/explain vs trace/diagram vs code vs numerical vs proof
- Variant focus per sub-type:
  - Numerical: scheduling algorithm, subnetting, page replacement, complexity
  - Diagram/trace: B-tree, AVL, DFA, ER diagram, process state
  - Code: write function, write query, write program
  - Proof: algorithm correctness, recurrence, turing machine
- Key pattern: diagram/trace questions almost never repeat exact same input — prepare the method not the answer
- Watch for: scenario-based questions that test judgment not just knowledge ("given this situation, which algorithm/design/approach and why")

---

## QUALITY CHECKS BEFORE OUTPUT

Before generating any output, verify:

- [ ] Every question from every paper has been assigned a chapter
- [ ] Unverified groupings are flagged
- [ ] Papers sorted chronologically — latest = index 1
- [ ] Recency tags assigned to every content pattern
- [ ] Due flags checked for every chapter
- [ ] Sample size warning added if N < 4
- [ ] Global disclaimer present
- [ ] "Lowest priority" section has disclaimer attached
- [ ] No language saying "ignore this" or "won't come" anywhere in output
- [ ] LaTeX compiles without errors before presenting PDF (full analysis only)

---

## LATEX STYLE REFERENCE (Full Analysis)

Colors:
```latex
\definecolor{structblue}{RGB}{13, 71, 161}
\definecolor{variantpurple}{RGB}{74, 20, 140}
\definecolor{flagorange}{RGB}{230, 81, 0}
\definecolor{priorityred}{RGB}{183, 28, 28}
\definecolor{cavemangreen}{RGB}{27, 94, 32}
\definecolor{headerbg}{RGB}{26, 35, 126}
```

Box types:
- `structbox` (blue) — internal split / structural data
- `variantbox` (purple) — variant breakdowns
- `flagbox` (orange) — due/emerging/unverified flags
- `cavebox` (green) — plain language chapter summary
- `prioritybox` (red) — priority action list

Table style: booktabs. `\toprule \midrule \bottomrule`. No vertical lines.

Font: 12pt, A4, 2cm margins. Header: subject name left, date right.

Compile: run pdflatex twice for TOC and references to resolve.
