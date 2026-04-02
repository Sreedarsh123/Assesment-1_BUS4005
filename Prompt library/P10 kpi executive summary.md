# P10 · End-of-Month KPI Executive Summary

**Section:** 10 — Performance Reporting
**Workflow step:** Step 1 of 2 (feeds into operations director review → executive distribution)
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are an operations performance analyst.

Using ONLY the KPI data provided below, generate a structured end-of-month executive dashboard summary.
Do not extrapolate trends, make forecasts, or draw strategic conclusions not directly supported by the data.
Do not fabricate positive commentary for metrics that are below target.

Month: [MONTH]
Site/Business Unit: [SITE]
KPI data: [KPI_TABLE]
Prior month data (for comparison): [PRIOR_MONTH]
Executive commentary from site manager (if any): [COMMENTARY]

Required sections:
1. Performance Headline (2–3 sentences — overall result this month, data-driven only)
2. KPI Summary Table (Metric | Target | Actual | vs Prior Month | Status)
3. Top 3 Wins (based on KPI data only — do not fabricate positive commentary for underperforming metrics)
4. Top 3 Areas for Focus (based on KPI data only)
5. Key Actions Underway (from site manager commentary only — state "None provided" if absent)
6. Next Month Outlook (state "Outlook not provided — site manager to add" if no data)

Tone: executive-appropriate, data-driven, third-person.
Length: maximum 350 words.
Mark this document: DRAFT — Director Review Required
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[MONTH]` | Reporting period | "March 2025" |
| `[SITE]` | Business unit | "Site 3 — Dandenong Manufacturing" |
| `[KPI_TABLE]` | BI system export | "OEE: Target 82%, Actual 76%. Safety incidents: Target 0, Actual 1. On-time delivery: Target 95%, Actual 91%. Scrap rate: Target <2%, Actual 1.8%." |
| `[PRIOR_MONTH]` | Prior month BI export | "OEE: 74%. Safety: 0 incidents. OTD: 93%. Scrap: 2.1%." |
| `[COMMENTARY]` | Site manager written input | "OEE improvement driven by Line 3 PM completion. Safety incident under investigation. OTD impacted by logistics partner delay in Week 2." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 2-step performance reporting chain.

- **Trigger:** Start of each month — performance team extracts prior month KPI data from BI system
- **Actor:** Performance analyst runs prompt per site; operations director reviews all site summaries
- **Timing:** Within 2 business days of month-end data availability
- **Next step:** Operations director reviews, edits outlook section, approves, and distributes to executive leadership team

```
Month ends → BI system closes data → Performance analyst exports KPI data → [P10 RUNS per site]
    → Operations director reviews all site DRAFT summaries → Edits outlook → Approves
    → Distributed to executive leadership team → Filed in board reporting system
```

---

## ❗ Problem Being Solved

Performance analysts spend 3–5 hours per site per month compiling the executive KPI dashboard narrative from raw BI data exports. Across 12 sites, this is **36–60 hours of analyst time monthly — approximately 430–720 hours annually**.

With AI-assisted drafting (10 min per site) and director review (10 min per site), total production time drops to **20 minutes per site — an ~85% saving**, recovering approximately **500 hours of analyst time annually** while improving narrative consistency across all sites.

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Very High — identical structure required every month, every site |
| Data availability | High — structured BI system export for all KPIs |
| Human judgment needed | Medium — director validates narrative tone, edits outlook, approves before distribution |
| Integration possibility | Output insertable directly into executive dashboard template or board reporting system |
| Estimated time saving | ~85% (4 hr → 20 min per site per month) |

**Human-in-the-loop role:** Operations director reviews and approves every site summary before executive distribution. Director adds or edits the outlook section based on strategic context not available in KPI data. DRAFT marking is a mandatory governance control.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model extrapolates trends or makes forecasts from current data | High | Explicit constraint: "Do not extrapolate trends, make forecasts, or draw strategic conclusions not in the data" |
| Model fabricates positive commentary for underperforming metrics | High | Explicit constraint: "Do not fabricate positive commentary for metrics below target" — wins drawn from KPI data only |
| Sensitive commercial performance data in prompt | Medium | Data handling policy: use internal/on-premise LLM; anonymise site identifiers if using external API |
| Draft distributed to leadership before director review | High | DRAFT marking in prompt output; governance: director approval mandatory before any distribution |

**Overall risk rating: HIGH** — director approval is mandatory before any distribution. This is a board-level governance document.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 11 March 2025
**Prompt:** `Write a monthly KPI summary for senior leadership: [KPI_DATA]`
**Output:** Model invented positive performance trends for a site where OEE had declined by 6 percentage points — described the result as "a strong quarter with improving operational efficiency." This was factually incorrect based on the data provided.
**Observed effect:** Would have directly misled the executive team about site performance. Operations director flagged the discrepancy immediately. Output required complete rewrite. Trust in the tool was significantly damaged.
**Lesson learned:** Executive-facing prompts are the highest-risk category for positive framing bias. The model optimises for a professionally confident tone — which, without a fabrication constraint, produces invented optimism for poor performance data.

---

### v1.1 — KPI table structure + fabrication constraint + trend extrapolation prohibition + DRAFT marking added ✅ Current
**Date:** 18 March 2025
**Change:** Added structured KPI table placeholder with prior month comparison. Added explicit constraint: "Do not fabricate positive commentary for metrics below target." Added trend extrapolation and forecasting prohibition. Added DRAFT marking with director review requirement. Wins and focus areas now explicitly constrained to KPI data only.
**Output:** 4/4 test summaries approved by operations director with only minor edits to the outlook section (which requires strategic context the prompt does not have). Zero fabricated data points. Performance headline accurately reflected actual KPI results, including underperformance.
**Observed effect:** Operations director reported significantly higher confidence in the accuracy of summaries. Director's editing time reduced to ~5 minutes per site — focused exclusively on the outlook section.
**Lesson learned:** "Do not fabricate positive commentary" is a more specific and more effective constraint than the general grounding instruction alone. Naming the specific failure mode in the constraint is more powerful than a generic prohibition.

---

## 🔗 Related Prompts

- **Data sources:** P06 (Daily Production Report) — aggregated monthly into KPI table input
- **Related:** P05 (Supplier Evaluation) — supplier KPIs may feed into site OTD metrics
- **Parent section:** [10-performance-reporting/README.md](README.md)
- **Library index:** [README.md](../README.md)
