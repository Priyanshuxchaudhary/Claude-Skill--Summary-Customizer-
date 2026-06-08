# summary-customizer

A Claude skill that tailors a resume's professional summary to a specific job description — producing a recruiter-optimized, strictly resume-grounded summary in exactly 275–285 characters.

---

## What It Does

Given a resume and a job description, this skill:

- Parses the JD for preferred attributes, core responsibilities, required qualifications, and company context
- Audits every resume bullet for signals that map to JD themes
- Selects and ranks signals by JD importance and "summary fit" (identity-level vs. project-level)
- Writes a three-part professional summary (identity → breadth → impact/anchor)
- Verifies the output against a grounding checklist and a hard 275–285 character limit
- Presents a traceability table and preferred-attributes coverage table alongside the final summary

---

## Trigger Phrases

This skill activates on requests like:

- "Tailor my summary to this job"
- "Rewrite my summary for this role"
- "Make my summary fit this JD"
- "Help me customize my professional summary"
- Uploading a resume + JD and asking for help with application materials

---

## Inputs Required

| Input | Description |
|-------|-------------|
| **Resume** | Full text or uploaded file (.pdf, .docx, or plain text) |
| **Job Description** | Full text including Responsibilities, Qualifications, and Preferred Attributes sections |

Both inputs are required before the skill begins. If either is missing, it will ask before proceeding.

---

## Output Format

**1. The final summary** — bold, standalone, 275–285 characters

**2. Character count** — verified programmatically

**3. Traceability table** — maps every phrase in the summary to its exact resume source

| Phrase | Resume Source |
|--------|--------------|
| MBA PM and strategy consultant | MS Engineering Management + "Product Strategy Consultant" role |
| performance dashboards | SuperAdmin Analytics Dashboard, Retail Analytics Power BI |
| … | … |

**4. Preferred attributes coverage table** — shows which JD preferred attributes are covered and how

| Attribute | Covered? | Signal Used |
|-----------|----------|-------------|
| Prior consulting/PM/strategy experience | ✅ | "MBA PM and strategy consultant" |
| Dashboards/BI/data viz | ✅ | "performance dashboards" |
| Collaborative, adaptable | ⚠️ implied | Cross-industry breadth |

---

## Core Rules

### Resume Grounding (non-negotiable)
> If it is not in the resume, it cannot be in the summary.

Every phrase must trace to a specific bullet, role, or metric in the resume. No inferred claims, no stretched signals.

### Summary vs. Bullet Point Distinction

| ❌ Reads as a bullet | ✅ Reads as a summary |
|---|---|
| "diagnosed a 40% revenue gap and built a pricing roadmap adopted by C-suite" | "translates market analysis into pricing strategy and business model transformation" |
| "built dashboard tracking 13 churn-risk metrics for 60+ enterprise accounts" | "drives performance dashboards across enterprise SaaS" |

A summary answers **who you are as a professional** — not what you did at one company.

### Character Limit
**275–285 characters including spaces. Hard constraint.** The skill verifies with Python and iterates until the count is in range.

### Preferred Attributes First
Preferred attributes are the highest-leverage section of any JD for summary differentiation. The skill explicitly audits each one and prioritizes those the resume can support.

---

## Common Mistakes This Skill Prevents

1. **Adding signals not in the resume** — the most frequent summary error
2. **Confusing market sizing with revenue** — `$88M opportunity ≠ $175K revenue generated`
3. **Narrating one project** instead of describing a pattern of work
4. **Ignoring preferred attributes** — the section most candidates miss
5. **Keyword stuffing for ATS** — this summary is recruiter-facing; optimized for human readability
6. **Off-target character counts** — "close enough" is not acceptable
7. **Stating soft traits explicitly** — never "collaborative" or "intellectually curious"; show these through breadth instead

---

## Summary Structure

```
[Professional identity] who [breadth of work across domains/industries];
[nature of impact / type of work you do], and [scale anchor or closing signal].
```

**Example:**
> MBA PM and strategy consultant who drives product strategy, GTM, monetization, and performance dashboards across AI SaaS, B2B robotics, and healthcare; translates market analysis into pricing strategy and business model transformation, and co-founded a startup to $175K revenue.

---

## Related Skills

| Skill | When to Use |
|-------|-------------|
| `resume-bullet-maximizer` | Strengthen individual resume bullets |
| `resume-tailor` | Tailor bullets across Experience, Projects, and Skills sections |
| `resume-tailor-full-stack` | Full resume tailoring — bullets + summary in one pass |
| `full-resume-tailor` | Complete tailored resume delivered as a formatted .docx |
