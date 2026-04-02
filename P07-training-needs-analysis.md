# P07 · Training Needs Analysis Summary

**Section:** 07 — Human Resources / Training
**Workflow step:** Step 1 of 2 (feeds into development plan)
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are an operations training coordinator.

Using ONLY the competency assessment data provided below, generate a structured training needs analysis (TNA) summary.
Do not recommend training programs or courses by name unless they are explicitly stated in the assessment data.
Do not make assumptions about the employee's capability beyond the ratings and notes provided.

Employee ID: [EMP_ID]
Role: [ROLE]
Assessment date: [DATE]
Competency ratings (1–5 scale): [COMPETENCY_DATA]
Assessor notes: [NOTES]

⚠️ Privacy notice: This prompt contains personal employee performance data. Handle in accordance with organisational privacy policy. Do not submit to external AI platforms without anonymisation.

Required sections:
1. Competency Profile Summary (overall rating and key findings — 2–3 sentences)
2. Identified Gaps (competencies rated below 3 — list only, with rating)
3. Priority Training Areas (rank by gap severity — do not name specific training courses unless stated in data)
4. Recommended Development Approach (On-the-job / Formal training / Mentoring — select based on gap severity and role)
5. Target Review Date (suggest 3 months from assessment date if not stated)

Tone: developmental, professional, third-person.
Length: maximum 250 words.
If competency data is incomplete, write "Assessment incomplete — reschedule required."
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[EMP_ID]` | HR system (anonymised for prompt use) | "OPS-0047" |
| `[ROLE]` | HR system | "Production Technician — Level 2" |
| `[DATE]` | Assessment record | "14 March 2025" |
| `[COMPETENCY_DATA]` | Competency assessment form | "Machine operation: 4/5. Safety procedures: 2/5. Quality inspection: 3/5. Documentation: 2/5. Team communication: 4/5." |
| `[NOTES]` | Assessor written notes | "Strong practical skills. Needs development in documentation standards and safety compliance. Responds well to on-the-job coaching." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 2-step training and development workflow.

- **Trigger:** Competency assessment completed for an operator or technician
- **Actor:** HR coordinator enters assessment results; line manager reviews TNA output
- **Timing:** Within 5 business days of assessment completion
- **Next step:** Line manager reviews TNA, approves development approach, initiates development plan in LMS

```
Assessor completes competency assessment → HR coordinator enters data → [P07 RUNS]
    → Line manager reviews DRAFT TNA → Validates development approach → Approves
    → Development plan created in LMS → Employee notified → Review date set
```

---

## ❗ Problem Being Solved

HR coordinators spend 35–45 minutes per employee writing TNA summaries from competency assessment spreadsheets. Across a workforce of 80 operational staff assessed annually, this totals **47–60 hours per year**.

With AI drafting (4 min) and line manager review (10 min), total time per TNA drops to **14 minutes — a ~65% saving**, recovering approximately **42 hours annually**.

Inconsistent TNA quality across HR coordinators also creates development equity risk — employees assessed by different coordinators may receive materially different levels of development planning detail for equivalent competency gaps.

---

## ⚡ Automation Potential

**Level: Medium-High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same competency framework applied to all operational roles |
| Data availability | Medium — dependent on assessor completing all competency rating fields |
| Human judgment needed | Medium — line manager validates development approach and approves plan |
| Integration possibility | Output insertable into LMS (Learning Management System) development plan fields |
| Estimated time saving | ~65% (40 min → 14 min per TNA) |

**Human-in-the-loop role:** Line manager reviews and approves the development approach before the TNA is shared with the employee. Employee privacy must be maintained — TNA is not shared without line manager approval.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model recommends specific external training courses not in assessment data | High | Explicit constraint: "Do not name training courses unless stated in assessment data" |
| Incomplete competency data produces unreliable TNA | Medium | "Assessment incomplete — reschedule required" fallback |
| Employee privacy risk — personal performance data in prompt | High | Privacy notice embedded in prompt; data handling policy: use internal LLM or anonymise Employee ID before any external submission |
| Development approach mismatched to actual gap severity | Medium | Line manager review mandatory before TNA shared with employee |

**Overall risk rating: HIGH** — employee privacy is a primary concern. Line manager approval required before TNA is shared. Internal LLM strongly recommended for this workflow.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 7 March 2025
**Prompt:** `Write a training needs analysis for this employee based on their competency assessment: [ASSESSMENT_DATA]`
**Output:** Model recommended specific external training providers and commercial courses (e.g., "enrol in a nationally accredited certificate program") not relevant to the operational role and not stated in the assessment data.
**Observed effect:** Recommended courses were inappropriate for the role level and would have created unrealistic development expectations. Line managers rejected all 3 test outputs.
**Lesson learned:** Without a constraint on training course naming, the model defaults to well-known general training options from its training data. Domain-specific outputs require explicit constraints on what the model can and cannot reference.

---

### v1.1 — Training name constraint + privacy notice + structured competency fields + fallback added ✅ Current
**Date:** 13 March 2025
**Change:** Added structured competency rating placeholder, explicit training course naming constraint, privacy notice embedded in prompt, "Assessment incomplete — reschedule required" fallback for missing data.
**Output:** Output confined entirely to assessment data. Development approach matched gap severity appropriately. 4/4 test TNAs approved by line managers with only minor edits.
**Observed effect:** HR coordinator review time reduced to ~4 minutes. Privacy notice embedded in prompt raised awareness of data sensitivity with all prompt users.
**Lesson learned:** Embedding a privacy notice directly in the prompt — visible to the user at execution time — is a simple but effective governance mechanism for prompts that handle personal data.

---

## 🔗 Related Prompts

- **Feeds into:** LMS development plan (manual creation post-TNA approval)
- **Related:** P01 (Shift Handover) — if operational competency gaps affect shift safety
- **Parent section:** [07-hr-training/README.md](README.md)
- **Library index:** [README.md](../README.md)
