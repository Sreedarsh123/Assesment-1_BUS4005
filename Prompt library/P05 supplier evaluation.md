# P05 · Supplier Performance Evaluation

**Section:** 05 — Procurement
**Workflow step:** Step 1 of 2 (feeds into procurement review meeting)
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a procurement analyst.

Using ONLY the supplier data provided below, generate a structured quarterly supplier performance evaluation.
Do not make assumptions about supplier capability beyond the data provided.
Do not suggest alternative suppliers or pricing unless stated in the data.

Supplier name: [SUPPLIER_NAME]
Evaluation period: [PERIOD]
On-time delivery rate: [OTD_RATE]%
Defect rate (PPM): [PPM]
Number of NCRs raised: [NCR_COUNT]
Responsiveness score (1–5): [SCORE]
Additional notes: [NOTES]

Required sections:
1. Performance Summary (2–3 sentences)
2. KPI Scorecard (table: Metric | Target | Actual | Status)
3. Key Strengths (only if supported by data — otherwise write "Insufficient data to identify strengths")
4. Areas for Improvement (only if supported by data — otherwise write "Insufficient data to identify gaps")
5. Overall Rating (Approved / Conditional / At Risk / Suspended — based on data only)
6. Recommended Actions (maximum 3 — based on data and rating only)

Tone: analytical, professional, third-person.
Length: maximum 300 words.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[SUPPLIER_NAME]` | ERP vendor master | "Apex Fasteners Pty Ltd" |
| `[PERIOD]` | Evaluation quarter | "Q1 2025 (Jan–Mar)" |
| `[OTD_RATE]` | ERP delivery data | "87" |
| `[PPM]` | QMS defect data | "420" |
| `[NCR_COUNT]` | QMS NCR register | "3" |
| `[SCORE]` | Procurement team assessment | "3" |
| `[NOTES]` | Procurement officer notes | "Supplier responded slowly to NCR-041. Delivery delays linked to logistics partner, not manufacturing." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 2-step quarterly procurement review chain.

- **Trigger:** End of each quarter — procurement team extracts supplier KPI data from ERP and QMS
- **Actor:** Procurement analyst runs prompt per supplier; procurement manager reviews outputs
- **Timing:** Within 5 business days of quarter-end
- **Next step:** Outputs presented at quarterly supplier performance review meeting; ratings communicated to suppliers

```
Quarter ends → Procurement analyst extracts ERP + QMS data → [P05 RUNS per supplier]
    → Procurement manager reviews evaluations → Approves ratings
    → Supplier review meeting → Ratings communicated → Action plans issued if At Risk / Suspended
```

---

## ❗ Problem Being Solved

Procurement analysts spend 45–60 minutes per supplier evaluation writing narrative summaries from raw KPI data exports. Across a supply base of 30 suppliers evaluated quarterly, this totals **90–120 hours per year**.

With AI drafting (5 min) and procurement manager review (10 min), total time per evaluation drops to **15 minutes — a ~70% saving**, recovering approximately **90 hours annually**.

Inconsistent rating methodologies between analysts also create supplier relationship risk — suppliers may receive different ratings for similar performance depending on who writes the evaluation.

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same KPI framework applied every quarter per supplier |
| Data availability | High — structured data extracted directly from ERP and QMS |
| Human judgment needed | Medium — procurement manager validates overall rating before publication |
| Integration possibility | Output insertable directly into supplier scorecard template |
| Estimated time saving | ~70% (50 min → 15 min per evaluation) |

**Human-in-the-loop role:** Procurement manager validates the overall rating against contract SLA thresholds before the evaluation is shared with the supplier. Rating publication without manager approval is not permitted.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model fabricates performance trends not in the data | High | Constraint: "Do not make assumptions beyond the data provided" |
| Incorrect overall rating applied (e.g. Approved instead of At Risk) | High | Procurement manager validates rating against contract SLA matrix before publication |
| Sensitive commercial data in prompt input | Medium | Data handling policy: use internal/on-premise LLM for supplier evaluations; no external API submission of commercial KPIs |
| Bias in qualitative notes field influencing rating | Medium | Notes field reviewed for bias by procurement manager before prompt execution |

**Overall risk rating: HIGH** — procurement manager approval required before any rating is communicated to a supplier.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 5 March 2025
**Prompt:** `Write a supplier performance evaluation based on these results: [SUPPLIER_DATA]`
**Output:** Model blended qualitative notes from the notes field into KPI scorecard cells; created inaccurate on-time delivery and defect rate entries by mixing narrative with numeric data.
**Observed effect:** Scorecard figures did not match ERP data. Output was unusable as a supplier-facing document without full rewrite.
**Lesson learned:** Without structured placeholder fields separating quantitative KPI data from qualitative notes, the model conflates both types of data in the output. Prompt structure must mirror output structure.

---

### v1.1 — Structured KPI placeholders + rating constraint + grounding added ✅ Current
**Date:** 12 March 2025
**Change:** Added individual structured placeholders for each KPI field (OTD, PPM, NCR count, responsiveness score), separated from [NOTES]. Added four-option overall rating constraint. Added grounding constraint and market speculation prohibition.
**Output:** Scorecard figures matched raw ERP data exactly across 4 test evaluations. Ratings correctly applied based on data. No fabricated supplier commentary.
**Observed effect:** Procurement manager approved all 4 test evaluations after 10-minute review. No KPI figure discrepancies identified.
**Lesson learned:** Structured placeholder fields drive structured output — when the prompt separates quantitative and qualitative data, the model keeps them separate in the output. Prompt architecture determines output architecture.

---

## 🔗 Related Prompts

- **Data sources:** ERP system (delivery/inventory data), QMS (NCR/defect data)
- **Related:** P03 (NCR Draft) — NCR count data feeds into this evaluation
- **Parent section:** [05-procurement/README.md](README.md)
- **Library index:** [README.md](../README.md)
