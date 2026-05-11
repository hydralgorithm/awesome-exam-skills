# Examples

Sample outputs from the pyq-analyzer skill.

Each example shows the subject, input description, output type selected, and the actual output.

---

## Example 1 — Engineering Mechanics (T1: Numerical-Technical)

**Input:** 5 end-semester papers, Manipal Institute of Technology  
**Output type:** C — Quick Brief  

```
⚠️ Based on 5 papers. Patterns only — not guarantees.

━━━━━━━━━━━━━━━━━━━━
STUDY IN THIS ORDER:
━━━━━━━━━━━━━━━━━━━━

🔴 DO FIRST — shows up every paper:
• Belt Drives: calculate length (open/cross) + power transmitted — Q1, 10 marks
• Gear Trains: velocity ratio + find N or T for compound/reverted trains — Q2, 10 marks
• Friction: ladder/wedge/screw problems — Q3A, 6 marks

🟡 DO SECOND — very likely:
• Cams: draw displacement diagram for given follower motion — Q4A, 6 marks
• Flywheel: find mass/dimensions for given coefficient of fluctuation — Q4B, 4 marks

🟢 DO IF TIME ALLOWS:
• Governors: Watt/Porter/Proell — Q5B, appeared 3/5 papers

━━━━━━━━━━━━━━━━━━━━
LOWEST PRIORITY (based on data — your call):
• Hoisting Machinery: appeared 1/5, last seen 2020
• Clutches: appeared 1/5, last seen 2019
━━━━━━━━━━━━━━━━━━━━
```

---

## Example 2 — Data Structures & Algorithms (T5: CSE)

**Input:** 4 end-semester papers  
**Output type:** B — Priority Analysis (excerpt)

```
## Trees | Slot: Q3 | Marks: 10 | 4/4 papers

Structure: Q3A → DIAGRAM/TRACE → 5 marks | Q3B → THEORY → 5 marks

Most likely content:
- Q3A: AVL tree insertion/deletion with rotations — 3/4 papers [confirmed in latest 2]
- Q3A: B-tree insert and split — 1/4 papers [last seen: 2022]
- Q3B: Compare BST vs AVL vs B-tree (when to use which) — 2/4 papers [confirmed in latest]

⚠️ Flags: B-tree questions ⚠️ EMERGING — appeared in latest paper only after 2-year absence
```

---

## Contribute your own examples

If you've used this skill and got good results, open a PR adding your example here.

**Format:**
- Subject and exam type (T1–T5)
- Input: number of papers, institution if you're comfortable sharing
- Output type (A, B, or C)
- The actual output (anonymize if needed)

Real outputs from real papers are far more useful than synthetic examples.
