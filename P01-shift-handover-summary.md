# P01 · Shift Handover Summary

**Section:** 01 — Shift Management
**Workflow step:** Step 1 of 2 (feeds into incoming supervisor review → system log)
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are an operations shift supervisor.

Using ONLY the information provided below, generate a structured shift handover summary report.
Do not infer, assume, or add any information not explicitly present in the shift log.

Shift log: [SHIFT_LOG]

Required sections:
1. Shift Overview (date, shift time, crew size)
2. Key Activities Completed (bullet list)
3. Incidents or Anomalies (state "None recorded" if not mentioned)
4. Equipment Status (flag any issues — state "No issues recorded" if not mentioned)
5. Pending Tasks for Next Shift

Tone: factual, operational, third-person.
Length: maximum 250 words.
If any section lacks data, write "Insufficient data — follow-up required."
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[SHIFT_LOG]` | Outgoing supervisor's written or digital shift log | "Day shift 0600–1400. Crew of 8. Completed scheduled PM on Line 3. Conveyor belt on Line 2 flagged for vibration. No incidents. Cleaning schedule incomplete — night shift to finish." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 2-prompt shift management chain.

- **Trigger:** End of each shift — outgoing supervisor enters shift log
- **Actor:** Outgoing supervisor runs prompt; incoming supervisor reviews output
- **Timing:** Within 30 minutes of shift end
- **Next step:** Output reviewed and signed off by incoming supervisor; logged in Operations Management System (OMS)

```
Outgoing supervisor enters shift log → [P01 RUNS]
    → Incoming supervisor reviews draft → Signs off
    → Logged in OMS → Flags fed into maintenance (if equipment issues noted)
```

---

## ❗ Problem Being Solved

Manual shift handover reports take supervisors 20–25 minutes to write freehand. Inconsistent formats cause incoming supervisors to miss critical pending tasks, creating operational continuity risk.

With AI drafting (3 min) and human review (5 min), total time drops to **8 minutes — a ~65% saving**.

Annualised across a 3-shift, 5-site operation (15 handovers/day): approximately **3,200 hours saved per year** in report writing time alone. Beyond time, standardised handovers reduce the risk of missed equipment flags and incomplete task handoffs between shifts.

---

## ⚡ Automation Potential

**Level: Medium-High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same structure required every shift, every day |
| Data availability | Dependent on completeness of supervisor's shift log entries |
| Human judgment needed | Medium — incoming supervisor verifies equipment flags and pending tasks |
| Integration possibility | Output pasteable directly into OMS handover field |
| Estimated time saving | ~65% (22 min → 8 min per handover) |

**Human-in-the-loop role:** Incoming supervisor must review all equipment flags and pending tasks before signing off. Sign-off is mandatory before the report is logged in the system.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model adds speculative operational context not in shift log | High | Grounding constraint: "Using ONLY the information provided — do not infer or assume" |
| Incomplete shift log produces gaps in handover summary | Medium | "Insufficient data — follow-up required" fallback built into every section |
| Equipment flags missed if supervisor does not review carefully | High | Incoming supervisor mandatory sign-off before system logging |
| Sensitive operational data in shift log | Low | Data handled internally; shift logs processed within company systems only |

**Overall risk rating: MEDIUM** — requires incoming supervisor review before sign-off. Equipment and incident flags must be verified.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 1 March 2025
**Prompt:** `Summarise this shift log for handover.`
**Output:** Model added speculative operational context not present in the shift log; missed equipment flags entirely; format varied between runs.
**Observed effect:** Incoming supervisors could not rely on output — required full rewrite. Equipment flag for Line 2 conveyor was omitted entirely.
**Lesson learned:** Without required sections, the model optimises for fluency and narrative flow, not operational completeness. Structure must be imposed explicitly.

---

### v1.1 — Role framing + required sections + grounding constraint added ✅ Current
**Date:** 8 March 2025
**Change:** Added role framing (shift supervisor), five required sections with explicit labels, grounding constraint ("Using ONLY the information provided"), and "Insufficient data — follow-up required" fallback per section.
**Output:** Structured output matched required format in all 6 test runs. Equipment flags correctly captured. Supervisors reported 5-minute review was sufficient before sign-off.
**Observed effect:** Handover quality consistent across all test runs; no speculative content added; pending tasks section reliably populated.
**Lesson learned:** Required sections act as a mandatory checklist — the model cannot skip a section even when data is sparse. This is the core mechanism for operational completeness.

---

## 🔗 Related Prompts

- **Next in chain:** OMS system logging (manual step after supervisor sign-off)
- **Feeds into:** P02 (Maintenance Work Order) — if equipment flags are identified
- **Parent section:** [01-shift-management/README.md](README.md)
- **Library index:** [README.md](../README.md)
