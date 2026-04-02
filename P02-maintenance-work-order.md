# P02 · Equipment Maintenance Work Order

**Section:** 02 — Maintenance
**Workflow step:** Step 2 of 3 (triggered after fault is logged → feeds into technician assignment)
**Current version:** v1.2
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.2 — current)

```
You are a maintenance planning engineer.

Using ONLY the fault description and asset data below, generate a structured corrective maintenance work order.
Do not recommend actions beyond what is implied by the fault description.
Do not list parts or resources unless they are explicitly stated in the fault description.

Fault description: [FAULT_DESCRIPTION]
Asset ID: [ASSET_ID]
Asset type: [ASSET_TYPE]
Last service date: [LAST_SERVICE_DATE]

Required sections:
1. Work Order Summary (1–2 sentences)
2. Fault Classification (Mechanical / Electrical / Instrumentation / Other)
3. Recommended Actions (numbered list — 3–5 steps, based on fault description only)
4. Parts or Resources Required (list only if stated in fault description — otherwise write "To be confirmed after inspection")
5. Priority Level (Critical / High / Medium / Low — based on fault description only)
6. Estimated Downtime (state "Unknown — assessment required" if not specified)

Tone: technical, precise, third-person.
Length: maximum 300 words.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[FAULT_DESCRIPTION]` | Operator fault report in CMMS | "Conveyor belt on Line 2 making grinding noise. Belt appears misaligned. Line stopped at 14:30." |
| `[ASSET_ID]` | CMMS asset register | "CONV-L2-007" |
| `[ASSET_TYPE]` | CMMS asset register | "Belt conveyor — 15m, 3-phase motor" |
| `[LAST_SERVICE_DATE]` | CMMS service history | "12 January 2025" |

---

## 🏢 Intended Workflow or Task

Step 2 of a 3-step maintenance workflow chain.

- **Trigger:** Operator logs a fault in the CMMS (Computerised Maintenance Management System)
- **Actor:** Maintenance planner runs prompt, reviews output, assigns technician, releases work order
- **Timing:** Within 1 hour of fault being logged
- **Next step:** Technician receives released work order; completes job; closes out in CMMS

```
Operator logs fault in CMMS → Maintenance planner extracts fault data → [P02 RUNS]
    → Planner reviews draft work order → Assigns technician → Releases in CMMS
    → Technician completes job → Work order closed → Feeds into P06 (downtime section)
```

---

## ❗ Problem Being Solved

Maintenance planners spend 25–35 minutes per corrective work order writing fault classifications and recommended actions from scratch. Across a facility averaging 15 corrective work orders per week, this totals **375–525 hours annually**.

With AI drafting (5 min) and planner review (8 min), total time per work order drops to **13 minutes — a ~60% saving**, recovering approximately **280 hours per year** per facility.

Beyond time savings, inconsistent work order quality — missing fault classifications, vague action steps — causes technician delays and incomplete CMMS records, undermining maintenance trend analysis.

---

## ⚡ Automation Potential

**Level: Medium-High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — standardised CMMS format required every time |
| Data availability | Dependent on quality of operator fault description |
| Human judgment needed | High — planner verifies priority, assigns technician, validates parts |
| Integration possibility | Output fields map directly to CMMS work order form |
| Estimated time saving | ~60% (30 min → 13 min per work order) |

**Human-in-the-loop role:** Maintenance planner must verify priority classification against SLA matrix and validate recommended actions before releasing the work order to a technician. Planner sign-off is mandatory before release.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model recommends actions beyond stated fault | High | Explicit constraint: "Do not recommend actions beyond what is implied by the fault description" |
| Parts list fabricated from general engineering knowledge | High | Explicit constraint: "Do not list parts unless stated in fault description" + fallback: "To be confirmed after inspection" |
| Incorrect priority classification applied | High | Planner mandatory review against SLA matrix before CMMS release |
| Asset data mismatch between input and CMMS | Medium | Asset ID cross-checked against CMMS before prompt is executed |

**Overall risk rating: HIGH** — always requires planner review and sign-off before work order release to technician.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 2 March 2025
**Prompt:** `Write a maintenance work order for this fault: [FAULT_DESCRIPTION]`
**Output:** Model invented a spare parts list (bearings, belt tensioner, motor coupling) and recommended a full drivetrain overhaul for a minor belt misalignment fault.
**Observed effect:** Would have triggered unnecessary parts procurement and extended planned downtime. Output completely unusable without major rewrite.
**Lesson learned:** Without asset context and action scope constraints, the model defaults to general engineering best practice — which overscopes corrective work orders significantly.

---

### v1.1 — Asset placeholders + fault classification added
**Date:** 9 March 2025
**Change:** Added structured placeholders for asset ID, asset type, and last service date. Added fault classification section (Mechanical / Electrical / Instrumentation / Other).
**Output:** Fault classification improved significantly. However, model still over-scoped recommended actions — suggested full motor inspection for a belt alignment fault.
**Observed effect:** Planners still required significant editing of the actions section. Parts list still fabricated.
**Lesson learned:** Providing asset context improves classification but does not constrain action scope. A dedicated negative constraint is required.

---

### v1.2 — Negative action constraint + parts fallback added ✅ Current
**Date:** 15 March 2025
**Change:** Added "Do not recommend actions beyond what is implied by the fault description" and "Do not list parts unless explicitly stated — use 'To be confirmed after inspection' fallback."
**Output:** Recommended actions confined to fault-relevant steps. Parts section correctly flagged as "To be confirmed" when not stated. 5/5 test work orders accepted by maintenance planners with only minor edits.
**Observed effect:** Planner review time reduced to ~8 minutes. No fabricated parts lists across all test runs.
**Lesson learned:** Negative constraints ("do not...") are as effective as positive instructions in constraining model scope. Both are necessary in factual, asset-specific prompts.

---

## 🔗 Related Prompts

- **Triggered by:** P01 (Shift Handover) — if equipment flag identified during shift
- **Feeds into:** P06 (Daily Production Report) — downtime data captured after work order closed
- **Parent section:** [02-maintenance/README.md](README.md)
- **Library index:** [README.md](../README.md)
