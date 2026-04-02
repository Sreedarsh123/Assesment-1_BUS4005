# P08 · Escalation Summary (Serious Incident)

**Section:** 08 — HSE / Operations
**Workflow step:** Step 3 of 3 — triggered from P04 if incident is classified as serious
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a senior operations manager.

Using ONLY the verified incident report provided below, generate a concise escalation summary for senior leadership.
Do not add commentary, opinion, or information not present in the verified report.
Do not speculate on liability, causation, or regulatory outcomes.

Verified incident report: [VERIFIED_REPORT]
Incident classification: [CLASSIFICATION]
Regulatory notification required (Y/N): [REG_NOTIF]

Required sections:
1. Executive Summary (3–4 sentences — what happened, impact, current status)
2. Immediate Actions Taken (from the verified report only)
3. Regulatory Obligations (state "Not applicable" if REG_NOTIF is N — state applicable obligations if Y)
4. Recommended Leadership Actions (maximum 3 bullet points — operational decisions only)
5. Next Update Due (propose 24 hours from incident time if not specified)

Tone: concise, serious, executive-appropriate.
Length: maximum 200 words.
This document is CONFIDENTIAL — restrict distribution to senior leadership only.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[VERIFIED_REPORT]` | Approved output from P04 — manager-signed incident report | Full text of the signed P04 incident report |
| `[CLASSIFICATION]` | Duty manager classification | "Serious — worker injury requiring medical treatment" |
| `[REG_NOTIF]` | Legal/compliance team advice | "Y — notification to SafeWork required within 24 hours" |

---

## 🏢 Intended Workflow or Task

Step 3 of a 3-prompt incident management chain. This prompt is only triggered when an incident is classified as serious.

- **Trigger:** P04 incident report is signed off AND classified as serious (injury, significant property damage, regulatory notification required)
- **Actor:** Operations manager runs prompt; operations director reviews and distributes
- **Timing:** Within 1 hour of serious incident classification
- **Next step:** Operations director approves and distributes to executive team via secure channel

```
P04 (Incident report) → Manager classifies as SERIOUS → [P08 RUNS]
    → Operations director reviews CONFIDENTIAL brief → Approves
    → Distributed to executive team via secure channel → Regulatory notification actioned if required
    → Next update scheduled within 24 hours
```

---

## ❗ Problem Being Solved

Operations managers spend 45–60 minutes writing escalation briefs for senior leadership after serious incidents — often while simultaneously managing the active incident response. This creates a critical vulnerability: the person most needed on the ground is diverted to document writing.

With AI drafting from the already-verified P04 report (5 min) and director review (10 min), total brief production time drops to **15 minutes — a ~70% saving**. Leadership receives a concise, consistent brief within one hour of escalation, enabling faster decision-making and regulatory response.

---

## ⚡ Automation Potential

**Level: Low-Medium (due to high stakes)**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Medium — triggered only for serious incidents (~5–10% of all incidents) |
| Data availability | High — feeds directly from verified and signed P04 report |
| Human judgment needed | Very High — operations director validates every word before distribution |
| Integration possibility | Output sent via secure channel to named executive distribution list |
| Estimated time saving | ~70% (50 min → 15 min per escalation brief) |

**Human-in-the-loop role:** Operations director must review and approve every word of the escalation brief before it is distributed. No auto-distribution under any circumstances. This is a CONFIDENTIAL document with legal and reputational implications.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model adds speculation or opinion on liability to executive brief | High | Explicit constraint: "Do not add commentary, opinion, or information not in the verified report. Do not speculate on liability, causation, or regulatory outcomes." |
| Regulatory obligation mis-stated or invented | High | Legal/compliance team provides REG_NOTIF classification; legal reviews regulatory obligations section before distribution |
| Confidential incident data exposed via insecure channel | High | CONFIDENTIAL marking in prompt output; distribution restricted to named leadership via secure channel only |
| Brief distributed before director sign-off | High | Governance policy: operations director approval is mandatory; output not auto-distributed under any circumstances |

**Overall risk rating: HIGH** — this is the highest-stakes document in the library. Director approval is non-negotiable. Legal review required for regulatory obligations section.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 9 March 2025
**Prompt:** `Write an escalation brief for senior leadership about this incident: [INCIDENT_NOTES]`
**Output:** Model added speculative commentary on liability ("the organisation may face significant legal exposure") and predicted regulatory outcomes ("likely to result in a SafeWork investigation") — neither was in the incident notes. Output also drew on general incident management knowledge to fill narrative gaps.
**Observed effect:** Output was legally dangerous for executive distribution. Would have created false expectations about regulatory outcomes and potential liability admissions. Completely unusable without full rewrite.
**Lesson learned:** High-stakes escalation prompts must ground exclusively in a verified upstream document — not raw staff notes. The model's general knowledge about incident management is a liability, not an asset, in legal contexts.

---

### v1.1 — Verified report grounding + liability speculation prohibition + CONFIDENTIAL marking + regulatory section added ✅ Current
**Date:** 15 March 2025
**Change:** Changed input from raw incident notes to verified P04 report output (chained prompt). Added explicit prohibition on liability and causation speculation. Added regulatory obligation section with Y/N input. Added CONFIDENTIAL marking. Added director review gate to workflow.
**Output:** 3/3 test briefs assessed as distribution-ready by operations director after 8-minute review. Zero speculative commentary. Regulatory section correctly flagged as "Not applicable" or cited applicable obligations based on REG_NOTIF input.
**Observed effect:** Operations directors reported significantly higher confidence in distributing the AI-assisted brief compared to v1.0. Chaining from the verified P04 report eliminated the main source of fabricated content.
**Lesson learned:** Chained prompts improve output quality and safety. Using the verified upstream document (P04) as the grounding input for P08 means the model cannot add information that wasn't already reviewed and approved by a human. The chain itself is a governance mechanism.

---

## 🔗 Related Prompts

- **Triggered by:** P04 (Incident Report) — only if classified as serious
- **Parent section:** [08-hse-escalation/README.md](README.md)
- **Library index:** [README.md](../README.md)
