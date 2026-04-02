# 📚 Prompt Library — Operations Workflow Automation

> **Assessment 1 | BUS4005 — Generative AI for Business**
> Student: Jo | Business Field: Multi-site Industrial Operations
> Model tested on: Claude Sonnet 4
> Last updated: March 2025

---

## What This Library Does

This prompt library supports workflow automation in **multi-site industrial operations**. It contains **10 documented, tested, and iterated prompts** organised by the business function they support.

Each prompt entry follows the same structure:
- The exact prompt text (with placeholders)
- The workflow task it supports
- The problem it solves
- Its automation potential
- Known risks and mitigations
- Version history and test results

---

## 📂 Folder Structure

```
prompt-library/
│
├── README.md                                        ← You are here (library index)
│
├── 01-shift-management/
│   └── P01-shift-handover-summary.md
│
├── 02-maintenance/
│   └── P02-maintenance-work-order.md
│
├── 03-quality-assurance/
│   └── P03-non-conformance-report.md
│
├── 04-hse/
│   └── P04-incident-report.md
│
├── 05-procurement/
│   └── P05-supplier-evaluation.md
│
├── 06-production/
│   └── P06-daily-production-report.md
│
├── 07-hr-training/
│   └── P07-training-needs-analysis.md
│
├── 08-hse-escalation/
│   └── P08-escalation-summary.md
│
├── 09-inventory-logistics/
│   └── P09-inventory-reorder.md
│
└── 10-performance-reporting/
    └── P10-kpi-executive-summary.md
```

---

## 📊 Library Summary Table

| ID | Prompt Name | Section | Automation Level | Risk Level | Version | Status |
|----|-------------|---------|-----------------|------------|---------|--------|
| P01 | Shift Handover Summary | Shift Management | Medium-High | Medium | v1.1 | ✅ Tested |
| P02 | Maintenance Work Order | Maintenance | Medium-High | High | v1.2 | ✅ Tested |
| P03 | Non-Conformance Report | Quality Assurance | Medium | High | v1.1 | ✅ Tested |
| P04 | Incident Report Draft | HSE | Medium | High | v1.1 | ✅ Tested |
| P05 | Supplier Performance Evaluation | Procurement | High | High | v1.1 | ✅ Tested |
| P06 | Daily Production Report | Production | High | Medium | v1.2 | ✅ Tested |
| P07 | Training Needs Analysis | HR / Training | Medium-High | High | v1.1 | ✅ Tested |
| P08 | Escalation Summary | HSE / Operations | Low-Medium | High | v1.1 | ✅ Tested |
| P09 | Inventory Reorder Recommendation | Inventory & Logistics | High | Medium | v1.2 | ✅ Tested |
| P10 | KPI Executive Summary | Performance Reporting | High | High | v1.1 | ✅ Tested |

**Automation levels:** Very High / High / Medium-High / Medium / Low-Medium / Low
**Risk levels:** High (always needs human review) / Medium (spot-check recommended) / Low (can automate with audit)

---

## 🔗 Prompt Chaining Map

Some prompts are designed to work in sequence. Outputs from one prompt feed directly into the next.

```
INCIDENT MANAGEMENT CHAIN
P04 (Incident report draft) → P08 (Escalation summary — if serious)

MAINTENANCE CHAIN
P02 (Maintenance work order) → feeds into CMMS system → P06 (Production report — downtime section)

PERFORMANCE REPORTING CHAIN
P06 (Daily production report) → aggregated into → P10 (KPI executive summary)
```

---

## ⚙️ Prompting Strategies Used

| Strategy | Prompts Applied | Why Chosen |
|----------|----------------|------------|
| Role framing | All 10 | Constrains model output register and tone to match professional context |
| Grounding constraint ("Using ONLY the information provided") | All 10 | Prevents hallucination in factual/legal/operational documents |
| Structured output templates (required sections) | All 10 | Ensures format consistency; sections act as mandatory checklist |
| Fallback language ("Insufficient data — follow-up required") | All 10 | Handles missing data explicitly rather than letting model fill gaps |
| Negative constraints ("do not infer / do not recommend beyond...") | P02, P03, P04, P05, P08, P10 | Prevents scope creep and speculation in constrained documents |
| Prompt decomposition and chaining | P04 → P08, P06 → P10 | Splits complex workflows; upstream output grounds downstream prompt |
| Formula transparency | P06, P09 | Calculation traceability required for operational auditability |
| DRAFT / CONFIDENTIAL marking | P03, P04, P08, P10 | Governance control — signals mandatory human review before use |

---

## 📝 Iteration Evidence Summary

All prompt versions are documented in individual prompt files. Key improvements across iterations:

| Prompt | Versions | Key Improvement |
|--------|----------|----------------|
| P01 | v1.0 → v1.1 | Added role framing + required sections; eliminated speculative content |
| P02 | v1.0 → v1.2 | Added asset placeholders; then negative action constraint — eliminated fabricated parts lists |
| P03 | v1.0 → v1.1 | Added root cause grounding + DRAFT marking — eliminated legally inadmissible speculation |
| P04 | v1.0 → v1.1 | Added grounding constraint — 0 fabricated claims vs fabricated causation in v1.0 |
| P05 | v1.0 → v1.1 | Separated KPI fields from qualitative notes — eliminated scorecard inaccuracies |
| P06 | v1.0 → v1.2 | Added OEE formula constraint then action scope constraint — 5/5 accepted without modification |
| P07 | v1.0 → v1.1 | Added training name constraint + privacy note — eliminated irrelevant course recommendations |
| P08 | v1.0 → v1.1 | Added verified report grounding + CONFIDENTIAL marking — eliminated liability speculation |
| P09 | v1.0 → v1.2 | Added supplier constraint then formula transparency — 5/5 matched manual calculation |
| P10 | v1.0 → v1.1 | Added fabrication constraint — eliminated invented positive trends for underperforming sites |

---

## 📖 References

Brown, T., Mann, B., Ryder, N., Subbiah, M., Kaplan, J., Dhariwal, P., Neelakantan, A., Shyam, P., Sastry, G., Askell, A., Agarwal, S., Herbert-Voss, A., Krueger, G., Henighan, T., Child, R., Ramesh, A., Ziegler, D. M., Wu, J., Winter, C., ... Amodei, D. (2020). Language models are few-shot learners. *Advances in Neural Information Processing Systems, 33*, 1877–1901. https://doi.org/10.48550/arXiv.2005.14165

OpenAI. (2023). *GPT-4 technical report*. arXiv. https://doi.org/10.48550/arXiv.2303.08774

Shanahan, M., McDonell, K., & Reynolds, L. (2023). Role play with large language models. *Nature, 623*, 493–498. https://doi.org/10.1038/s41586-023-06647-8

Wei, J., Wang, X., Schuurmans, D., Bosma, M., Ichter, B., Xia, F., Chi, E., Le, Q., & Zhou, D. (2022). Chain-of-thought prompting elicits reasoning in large language models. *Advances in Neural Information Processing Systems, 35*, 24824–24837. https://doi.org/10.48550/arXiv.2201.11903

White, J., Fu, Q., Hays, S., Sandborn, M., Olea, C., Gilbert, H., Elnashar, A., Spencer-Smith, J., & Schmidt, D. C. (2023). A prompt pattern catalog to enhance prompt engineering with ChatGPT. arXiv. https://doi.org/10.48550/arXiv.2302.11382
