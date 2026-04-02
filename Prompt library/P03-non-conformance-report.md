# P03 · Non-Conformance Report (NCR) Draft

**Section:** 03 — Quality Assurance
**Workflow step:** Step 1 of 3 (feeds into QA review → corrective action)
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a quality assurance engineer.

Using ONLY the inspection findings provided below, write a structured Non-Conformance Report (NCR).
Do not infer root cause unless explicitly stated in the findings.
Do not recommend corrective actions beyond what is clearly implied by the findings.

Inspection findings: [INSPECTION_FINDINGS]
Product or component: [PRODUCT_ID]
Inspection standard: [STANDARD]

Required sections:
1. NCR Summary (what was found, where, when — 1–2 sentences)
2. Non-Conformance Description (technical detail from findings only)
3. Applicable Standard or Specification Clause (state "Refer to document — clause not specified" if not provided)
4. Disposition (Accept / Rework / Reject / Hold — select applicable based on findings only)
5. Root Cause (state "Under investigation — insufficient data" if not stated in findings)
6. Recommended Corrective Action (only if clearly implied by findings — otherwise write "To be determined following root cause investigation")

Tone: technical, neutral, third-person.
Length: maximum 350 words.
Mark this document: DRAFT — QA REVIEW REQUIRED
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[INSPECTION_FINDINGS]` | Inspector's written findings report | "Weld bead on joint WJ-047 shows surface porosity at 3 locations. Porosity diameter approx 1.5mm. Found during visual inspection at final QC stage." |
| `[PRODUCT_ID]` | Work order or job card | "WJ-047 — Structural weld, Beam Assembly B-12" |
| `[STANDARD]` | Applicable inspection standard | "AS/NZS 1554.1:2014 — Structural steel welding" |

---

## 🏢 Intended Workflow or Task

Step 1 of a 3-prompt quality assurance chain.

- **Trigger:** Inspector identifies a non-conformance during in-process or final inspection
- **Actor:** QA engineer runs prompt using inspector's written findings; reviews and releases NCR into QMS
- **Timing:** Within 4 hours of non-conformance being identified
- **Next step:** NCR logged in QMS; corrective action assigned; disposition actioned by production team

```
Inspector identifies non-conformance → Writes findings → QA engineer enters data → [P03 RUNS]
    → QA engineer reviews DRAFT NCR → Validates disposition and root cause → Signs off
    → NCR released into QMS → Corrective action assigned → Production disposes product
```

---

## ❗ Problem Being Solved

QA engineers spend 30–40 minutes per NCR writing technical descriptions, selecting dispositions, and drafting corrective action recommendations from inspection findings. Across a mid-sized manufacturing operation generating 20 NCRs per month, this totals **600–800 hours per year**.

With AI drafting (5 min) and QA review (9 min), total time drops to **14 minutes per NCR — a ~60% saving**, recovering approximately **480 hours annually**.

Inconsistent NCR formats also create audit risk — non-conformances documented with varying levels of technical detail are difficult to trend and analyse for root cause patterns.

---

## ⚡ Automation Potential

**Level: Medium**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — fixed NCR format required per ISO 9001 / AS/NZS standards |
| Data availability | Dependent on inspector's written findings quality |
| Human judgment needed | High — QA engineer validates disposition and root cause before release |
| Integration possibility | Output exportable into QMS (Quality Management System) NCR fields |
| Estimated time saving | ~60% (35 min → 14 min per NCR) |

**Human-in-the-loop role:** QA engineer must validate disposition selection and root cause statement before releasing the NCR into the QMS. DRAFT marking is a mandatory governance control — no NCR is released without QA sign-off.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model infers root cause not stated in findings | High | Explicit constraint: "Do not infer root cause unless explicitly stated" + fallback: "Under investigation — insufficient data" |
| Wrong disposition selected (e.g. Accept instead of Reject) | High | Disposition constrained to four options; QA engineer validates before QMS release |
| NCR released without QA review | High | DRAFT marking built into prompt output; governance: mandatory QA sign-off policy |
| Standard clause mis-cited or invented | Medium | Fallback: "Refer to document — clause not specified" if not provided in input |

**Overall risk rating: HIGH** — always requires QA engineer review and sign-off. Do not release NCR to QMS without human validation.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 3 March 2025
**Prompt:** `Write an NCR for this inspection finding: [INSPECTION_FINDINGS]`
**Output:** Model speculated root cause ("likely caused by insufficient pre-heat temperature") and recommended full production halt — neither was stated in the findings.
**Observed effect:** Output was legally inadmissible as an NCR — fabricated root cause would have invalidated the corrective action process. Required complete rewrite.
**Lesson learned:** Open-ended NCR prompts allow the model to apply general quality engineering knowledge to fill gaps. In legal/compliance documents, this is unacceptable. Grounding constraint is essential.

---

### v1.1 — Grounding constraint + disposition list + DRAFT marking + root cause fallback added ✅ Current
**Date:** 10 March 2025
**Change:** Added grounding constraint ("Using ONLY the inspection findings provided"), root cause fallback ("Under investigation — insufficient data"), disposition constraint (four options only), DRAFT marking, standard clause fallback.
**Output:** 4/4 test NCRs accepted by QA engineers with only minor edits. Zero fabricated root causes across all test runs. Dispositions correctly selected based on findings.
**Observed effect:** QA review time reduced to ~9 minutes. DRAFT marking correctly flagged all outputs as requiring review before QMS release.
**Lesson learned:** DRAFT marking is a governance control, not just formatting. Combined with a mandatory sign-off policy, it creates a reliable human-in-the-loop gate for compliance documents.

---

## 🔗 Related Prompts

- **Triggers corrective action:** QMS corrective action workflow (manual step post-NCR release)
- **Related:** P02 (Maintenance Work Order) — if non-conformance is equipment-related
- **Parent section:** [03-quality-assurance/README.md](README.md)
- **Library index:** [README.md](../README.md)
