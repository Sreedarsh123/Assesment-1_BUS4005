# P04 · Incident Report First Draft

**Section:** 04 — Health, Safety & Environment
**Workflow step:** Step 1 of 3 (feeds into manager review → escalation decision)
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are an operations analyst.

Using ONLY the information provided below, write a structured incident report.
Do not infer, assume, or add information that is not explicitly stated in the notes.

Incident notes: [STAFF_NOTES]

Required sections:
1. Summary (2–3 sentences — what happened, when, where)
2. Timeline (bullet points with approximate times if known)
3. Root cause (only if clearly stated in the notes — otherwise write "Root cause: Insufficient data — follow-up required")
4. Immediate actions taken
5. Recommended next steps

Tone: factual, neutral, third-person.
Length: maximum 300 words.
If any section cannot be completed from the notes, write "Insufficient data — follow-up required."
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[STAFF_NOTES]` | Staff member's handwritten or verbal account | "Customer slipped near freezer aisle at 2pm. Wet floor sign was not present. Staff member called first aid..." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 3-prompt incident management chain.

- **Trigger:** Incident reported by staff member (in-store or via app)
- **Actor:** Duty manager runs prompt, reviews output, submits report
- **Timing:** Within 2 hours of incident
- **Next step:** Output feeds into manager sign-off, then escalation decision if required

```
Staff reports incident → Duty manager enters notes → [P04 RUNS]
    → Manager reviews draft → Signs off → Logged in system
    → If serious: P08 (Escalation summary) triggered
```

---

## ❗ Problem Being Solved

Manual incident reports take an average of **30 minutes** per incident for duty managers.

With AI drafting (5 min) and human review (10 min), total time drops to **15 minutes — a 50% saving**.

Annualised across a multi-site operation (est. 3 incidents/week across 5 sites): **195 hours saved per year.**

Beyond time, inconsistent reporting creates compliance risk — reports vary in format and completeness depending on which manager writes them.

---

## ⚡ Automation Potential

**Level: Medium**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same format required every incident |
| Data availability | Dependent on quality of staff notes |
| Human judgment needed | High — factual claims must be verified against evidence |
| Integration possibility | Output pasteable into incident management system |
| Estimated time saving | ~50% (30 min → 15 min per report) |

**Human-in-the-loop role:** Duty manager must verify all factual claims against CCTV logs, witness statements, and system records before submitting. This is non-negotiable due to legal and insurance implications.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model infers causation not stated in notes | High | Grounding constraint: "using ONLY the information provided below" + "do not infer or assume" |
| Incomplete staff notes produce low-quality draft | Medium | "Insufficient data — follow-up required" fallback built into prompt |
| Output used without human verification | High | Governance policy: manager sign-off is mandatory before submission. Prompt output is marked as DRAFT. |
| Legal/insurance implications if report is inaccurate | High | Legal review required for any report involving injury, significant property damage, or potential litigation |

**Overall risk rating: HIGH** — always requires human review and verification. Do not automate submission.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 8 March 2025
**Prompt:** `Write an incident report based on these notes: [STAFF_NOTES]`
**Output:** Fluent and well-structured — but fabricated a probable cause ("likely caused by a maintenance failure") that was not in the notes.
**Observed effect:** Would have created a misleading legal record. Report had to be completely rewritten.
**Lesson learned:** Open-ended prompts allow the model to fill gaps with plausible-sounding but unverified claims. Grounding constraint is essential for factual/legal contexts.

---

### v1.1 — Grounding constraint + fallback added ✅ Current
**Date:** 14 March 2025
**Change:** Added "Using ONLY the information provided" + "do not infer or assume" + "Insufficient data — follow-up required" fallback for each section.
**Output:** Model now leaves gaps explicit rather than filling them. Managers know exactly what requires follow-up.
**Observed effect:** 0 fabricated claims across 5 test runs. Manager review time down to ~10 min.
**Lesson learned:** Explicit grounding constraints are the single most important mitigation for hallucination in factual documentation prompts.

---

## 🔗 Related Prompts

- **Next in chain:** P08 — Escalation summary (triggered if incident is serious)
- **Parent section:** [04-hse/README.md](README.md)
- **Library index:** [README.md](../README.md)
