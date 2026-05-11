# Reddit Post — r/ClaudeAI or r/artificial or r/ChatGPTPromptEngineering

---

**Title:**
I built a Claude skill that turns your PYQ papers into a structured study plan with confidence scores — outputs a LaTeX PDF [GitHub]

---

**Body:**

Been using Claude for exam prep and got tired of asking it the same "analyze my PYQ" prompt every time and getting inconsistent results. So I turned it into a proper skill with defined phases, confidence scoring logic, and structured output formats.

This is the first skill in a repo I'm building for exam-prep Claude skills. More coming.

**What pyq-analyzer does:**

Upload 3–10 previous year question papers. The skill:

1. Auto-detects subject, institution, and exam structure from the paper headers
2. Maps every question across all papers to a chapter/topic
3. Runs 6 pattern layers: slot patterns, mark weights, question types, internal A/B/C splits, variant sub-types, rotation cycles
4. Scores confidence with fractions (e.g. `4/5 papers`) + recency tags so you can see both frequency and trend direction
5. Flags anomalies: "due" topics (historically consistent, recently absent), emerging variants, unverified groupings

**Three output types:**

- **Full Analysis** → multi-page LaTeX PDF with color-coded boxes per chapter (structural data, variant breakdowns, flags, plain-language summaries, tearout priority page)
- **Priority Analysis** → clean chat response with per-chapter breakdown + priority list
- **Quick Brief** → one-page action list, no explanations, just: study this → then this → then this

**Works for any subject** — engineering (numerical), CSE, humanities, law, medicine. There's a classification step that adapts the pattern extraction logic to the exam type.

**The philosophy behind it:**

> "Show raw data — let the student draw their own conclusions."

It never says "ignore this" or "this won't come." It says "lowest priority based on data" and attaches a disclaimer. You're still making the decisions.

---

**GitHub:** [link to your repo]

Tested on engineering mechanics (5 papers) and DBMS (4 papers). If you try it on your subject and something doesn't work right, open an issue — genuinely want to improve the chapter mapping for edge cases.

---

# Alternative — r/india or r/Indian_Academia subreddits

**Title:**
Built a Claude skill specifically for analyzing Indian engineering/university PYQs — pattern confidence scores + LaTeX PDF output [GitHub]

**Body:**

Made this for my own exam prep at MIT Bengaluru. Works on any uploaded PYQ set. It's the first skill in a repo I'm planning to keep adding to.

The key thing it does differently from just asking Claude to "analyze my PYQ":

- It maps every single question across all papers (not just skimming)
- Gives you fractions like `4/5 papers` instead of vague "this is frequently asked" — you can see exactly how thin or thick the data is
- Recency tags: if something appeared 4 times but not in the last 2 papers, it flags that separately
- Full Analysis mode generates a proper LaTeX PDF — not a chat wall of text

Particularly tuned for:
- CSE subjects (OS, DBMS, CN, DSA, TOC) — it knows the difference between trace-type and proof-type questions
- Engineering numericals — it tracks which specific formula/variant appeared, not just "gear trains was asked"

GitHub: [link]

If you're at an Indian university and have PYQs lying around, try uploading 3–5 papers and see what it picks up. Would love edge case reports.
