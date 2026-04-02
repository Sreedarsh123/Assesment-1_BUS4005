# P06 · Daily Production Report

**Section:** 06 — Production
**Workflow step:** Step 1 of 2 (feeds into operations manager review)
**Current version:** v1.2
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.2 — current)

```
You are a production analyst.

Using ONLY the production data provided below, generate a structured daily production report.
Do not calculate derived metrics unless the formula is explicitly provided below.
Do not recommend actions beyond what is explicitly mentioned in the operator notes.

Date: [DATE]
Shift: [SHIFT]
Target output (units): [TARGET]
Actual output (units): [ACTUAL]
Downtime (minutes): [DOWNTIME_MIN]
Downtime reason: [DOWNTIME_REASON]
Scrap count (units): [SCRAP]
Operator notes: [NOTES]

OEE formula (if applicable):
- Availability = (Shift time - Downtime) / Shift time
- Performance = Actual output / Target output
- Quality = (Actual output - Scrap) / Actual output
- OEE = Availability × Performance × Quality

Required sections:
1. Production Summary (actual vs target — state variance in units and %)
2. OEE Indicators (calculate only if downtime, target, and scrap are all provided — show formula used)
3. Downtime Analysis (cause and duration — state "Not recorded" if absent)
4. Quality Summary (scrap count and scrap rate)
5. Actions Required (list only items explicitly mentioned in operator notes — state "None recorded" if absent)

Tone: concise, data-driven, third-person.
Length: maximum 250 words.
If any data field is missing, note "Data not provided — review required."
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[DATE]` | MES export | "18 March 2025" |
| `[SHIFT]` | MES export | "Day shift — 0600 to 1400" |
| `[TARGET]` | Production schedule | "480" |
| `[ACTUAL]` | MES export | "412" |
| `[DOWNTIME_MIN]` | MES export | "55" |
| `[DOWNTIME_REASON]` | Operator log | "Conveyor jam — Line 2" |
| `[SCRAP]` | QC log | "14" |
| `[NOTES]` | Operator handover notes | "Conveyor cleared at 11:15. Maintenance notified. Cleaning incomplete — request night shift complete." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 2-step production reporting chain.

- **Trigger:** End of each production shift — MES data exported by shift supervisor
- **Actor:** Shift supervisor enters MES data into prompt; operations manager reviews output
- **Timing:** Within 1 hour of shift end
- **Next step:** Report sent to operations manager; downtime and scrap data fed into monthly KPI aggregation for P10

```
Shift ends → Supervisor exports MES data → [P06 RUNS]
    → Operations manager reviews draft → Approves and distributes
    → Downtime and quality data aggregated monthly → Feeds into P10 (KPI Executive Summary)
```

---

## ❗ Problem Being Solved

Shift supervisors spend 20–30 minutes compiling daily production reports from MES data exports. Inconsistencies in OEE calculation methods between supervisors create unreliable trend data — different supervisors apply different formulas or definitions of shift time.

With AI drafting using a fixed, stated OEE formula (3 min) and supervisor review (5 min), total time drops to **8 minutes — a ~70% saving**. Standardising the OEE formula across all shifts produces reliable, comparable trend data for monthly KPI reporting.

Across a 3-shift, 5-site operation (15 reports/day), this recovers approximately **3,100 hours of supervisor time annually**.

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Very High — identical structure required every shift, every day |
| Data availability | High — structured MES data export |
| Human judgment needed | Low-Medium — supervisor validates anomalies and actions only |
| Integration possibility | High — output directly insertable into BI dashboard or reporting system |
| Estimated time saving | ~70% (25 min → 8 min per report) |

**Human-in-the-loop role:** Shift supervisor reviews and approves the report before it is distributed to the operations manager. Supervisor approval is required before the report is shared or logged.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model calculates OEE using incorrect formula or assumptions | High | OEE formula explicitly stated in prompt; constraint: "calculate only if all inputs provided — show formula used" |
| Actions invented beyond what is stated in operator notes | High | Constraint: "Do not recommend actions beyond what is explicitly mentioned in operator notes" |
| Missing MES data fields produce unreliable OEE | Medium | "Data not provided — review required" fallback per missing field |
| Report distributed without supervisor approval | Medium | Governance: supervisor approval required before distribution to operations manager |

**Overall risk rating: MEDIUM** — supervisor review and approval required. OEE calculation must be verified against stated formula.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 6 March 2025
**Prompt:** `Write a daily production report from this data: [PRODUCTION_DATA]`
**Output:** Model estimated OEE using assumed industry standard shift time (8 hours) not stated in the data. Recommended additional maintenance actions not mentioned by the operator.
**Observed effect:** OEE figure differed from site standard calculation by 4.3 percentage points — a significant discrepancy for trend analysis. Actions section included fabricated recommendations.
**Lesson learned:** Any calculated metric in an operational prompt must either specify the formula or prohibit estimation entirely. General knowledge defaults produce inconsistent results.

---

### v1.1 — Structured data fields + OEE formula constraint added
**Date:** 11 March 2025
**Change:** Added individual structured placeholders for all data fields. Added OEE formula explicitly to prompt. Added constraint: "Do not calculate unless all inputs provided."
**Output:** OEE calculation matched site standard exactly. However, model still added recommended actions not mentioned in operator notes — suggested a full conveyor inspection not referenced in notes.
**Observed effect:** Supervisors still needed to edit actions section. Formula constraint alone did not prevent scope creep in the actions section.
**Lesson learned:** Each constraint needs its own explicit instruction. An OEE formula constraint does not cascade to prevent fabricated action recommendations — a separate negative constraint is required.

---

### v1.2 — Action scope constraint + formula transparency + missing data fallback added ✅ Current
**Date:** 16 March 2025
**Change:** Added "Do not recommend actions beyond what is explicitly mentioned in operator notes." Added "show formula used" to OEE calculation instruction. Added "Data not provided — review required" fallback per missing field.
**Output:** 5/5 test reports accepted by operations managers without modification. OEE formula shown in output for full traceability. Actions section contained only items from operator notes.
**Observed effect:** Supervisor review time reduced to ~5 minutes. Formula transparency allowed operations managers to audit OEE calculations independently.
**Lesson learned:** Formula transparency in the output is a governance requirement — it allows humans to verify calculated results without re-running the calculation themselves.

---

## 🔗 Related Prompts

- **Feeds into:** P10 (KPI Executive Summary) — monthly aggregation of daily production data
- **Related:** P02 (Maintenance Work Order) — downtime events may trigger maintenance work orders
- **Parent section:** [06-production/README.md](README.md)
- **Library index:** [README.md](../README.md)
