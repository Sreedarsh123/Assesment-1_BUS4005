# P09 · Inventory Reorder Recommendation

**Section:** 09 — Inventory & Logistics
**Workflow step:** Step 1 of 2 (feeds into warehouse manager approval → PO creation)
**Current version:** v1.2
**Status:** ✅ Tested
**Last updated:** March 2025

---

## 📌 Prompt Text (v1.2 — current)

```
You are an inventory planning analyst.

Using ONLY the inventory data provided below, generate a structured reorder recommendation.
Do not suggest alternative suppliers or pricing unless stated in the data.
Do not estimate or assume any data field not provided.

Item code: [ITEM_CODE]
Item description: [DESCRIPTION]
Current stock level (units): [CURRENT_STOCK]
Reorder point (units): [REORDER_POINT]
Average daily usage (units/day): [DAILY_USAGE]
Lead time (days): [LEAD_TIME]
Economic Order Quantity (EOQ): [EOQ]
Preferred supplier: [SUPPLIER]
Additional notes: [NOTES]

Required sections:
1. Reorder Trigger Assessment (is current stock below reorder point? State Yes/No with numeric comparison)
2. Days of Stock Remaining
   Formula used: Days remaining = Current stock ÷ Daily usage = [CURRENT_STOCK] ÷ [DAILY_USAGE] = [result] days
   (State this formula explicitly in the output)
3. Recommended Order Quantity (state EOQ value if provided — otherwise write "EOQ not provided — manual review required")
4. Urgency Classification:
   - Critical: Days remaining ≤ Lead time
   - High: Days remaining ≤ Lead time × 1.5
   - Standard: Days remaining > Lead time × 1.5
5. Recommended Action (Place order now / Monitor — check again in [X] days / Escalate to procurement manager)

Tone: analytical, concise, third-person.
Length: maximum 200 words.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[ITEM_CODE]` | ERP inventory master | "BOLT-M12-SS-500" |
| `[DESCRIPTION]` | ERP inventory master | "M12 stainless steel hex bolts, box of 500" |
| `[CURRENT_STOCK]` | ERP stock level | "240" |
| `[REORDER_POINT]` | ERP reorder config | "300" |
| `[DAILY_USAGE]` | ERP usage history | "18" |
| `[LEAD_TIME]` | Supplier lead time record | "8" |
| `[EOQ]` | Procurement calculation | "450" |
| `[SUPPLIER]` | ERP vendor master | "FastenerCo Pty Ltd" |
| `[NOTES]` | Warehouse notes | "Supplier advised 3-day delay next month due to stock build." |

---

## 🏢 Intended Workflow or Task

Step 1 of a 2-step inventory replenishment workflow.

- **Trigger:** Daily ERP flag — stock level at or below reorder point for a flagged item
- **Actor:** Inventory controller runs prompt for each flagged item; warehouse manager approves Critical/High recommendations
- **Timing:** Daily — processed within 2 hours of ERP morning flag run
- **Next step:** Warehouse manager approves recommendations; purchase orders created in ERP for approved items

```
ERP flags item at/below reorder point → Inventory controller extracts data → [P09 RUNS]
    → Controller reviews recommendation → Escalates Critical/High to warehouse manager → Manager approves
    → PO created in ERP → Submitted to preferred supplier → Delivery tracked against lead time
```

---

## ❗ Problem Being Solved

Inventory controllers manually review 40–60 stock items daily that approach reorder points, spending 2–3 minutes per item to write reorder justifications. Across a 50-item daily review, this totals **100–150 minutes per day — approximately 400–600 hours annually**.

With AI-assisted recommendations, controller review time drops to **30 seconds per item** — a ~80% saving — recovering approximately **350 hours per year**. Standardised urgency classification also ensures Critical items are consistently escalated to the warehouse manager, reducing the risk of stockouts from missed manual escalations.

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Very High — identical calculation logic applied to every flagged item daily |
| Data availability | High — structured ERP data export for all flagged items |
| Human judgment needed | Low — warehouse manager reviews Critical/High flags only; Standard items auto-approved |
| Integration possibility | High — output triggers draft PO creation in ERP for approved items |
| Estimated time saving | ~80% (2.5 min → 0.5 min per item) |

**Human-in-the-loop role:** Warehouse manager reviews and approves all Critical and High urgency recommendations before PO creation. Standard recommendations for items over $500 also require manual approval. Below-threshold Standard items may be auto-approved per governance policy.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model invents EOQ, supplier, or pricing data not provided | High | Explicit constraint: "Do not suggest alternative suppliers or pricing unless stated in data" + EOQ fallback: "EOQ not provided — manual review required" |
| Urgency misclassification triggers unnecessary emergency order | High | Urgency classification logic explicitly stated in prompt; warehouse manager reviews all Critical/High flags |
| ERP data export errors cascade into reorder errors | Medium | Inventory controller spot-checks 5% of ERP exports before batch prompt run |
| Auto-approval of high-value orders without human review | Medium | Governance: all orders above $500 require warehouse manager manual approval; below $500 Standard items may be auto-approved |

**Overall risk rating: MEDIUM** — warehouse manager approval required for all Critical and High urgency items and all orders above $500.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 10 March 2025
**Prompt:** `Recommend a reorder for this inventory item based on these stock levels: [STOCK_DATA]`
**Output:** Model suggested alternative suppliers with speculative pricing ("estimated market price $0.85/unit based on typical fastener costs") not present in the data. Introduced significant procurement risk by fabricating supplier options and cost estimates.
**Observed effect:** Output would have bypassed preferred supplier agreements and introduced unapproved vendors into the procurement process. Completely unusable.
**Lesson learned:** Commercial prompts must explicitly prohibit market speculation and alternative supplier suggestions. The model's general commercial knowledge is a liability in procurement contexts.

---

### v1.1 — Supplier constraint + EOQ fallback added
**Date:** 14 March 2025
**Change:** Added explicit prohibition on alternative supplier suggestions and pricing. Added EOQ fallback ("EOQ not provided — manual review required"). Added structured placeholder fields for all inventory data.
**Output:** No fabricated supplier suggestions or pricing. EOQ correctly handled. However, days-of-stock calculation was shown as a result only — formula not displayed, creating traceability issues for audit.
**Observed effect:** Warehouse managers could not verify the calculation without re-running it manually. Audit trail incomplete.
**Lesson learned:** Auditability of calculations requires the formula to be explicitly stated in the output, not just the result. "Show your working" is a governance requirement in inventory management.

---

### v1.2 — Formula transparency + urgency classification logic added ✅ Current
**Date:** 17 March 2025
**Change:** Added explicit formula display requirement for days-of-stock calculation (formula shown in output with values substituted). Added explicit urgency classification logic (Critical / High / Standard thresholds based on lead time multipliers).
**Output:** 5/5 test items matched manual calculation exactly. Formula shown in every output. Urgency classification matched manual assessment in all 5 cases. Warehouse manager approved all 5 recommendations without modification.
**Observed effect:** Formula transparency reduced warehouse manager review time from 3 minutes to under 1 minute per Critical/High item — verification became a scan rather than a recalculation.
**Lesson learned:** Formula transparency in the output is both a governance requirement and a user experience improvement — it builds human trust in AI-calculated recommendations by making the reasoning inspectable.

---

## 🔗 Related Prompts

- **Data source:** ERP inventory management system (daily flag export)
- **Related:** P05 (Supplier Evaluation) — preferred supplier performance data informs reorder decisions
- **Parent section:** [09-inventory-logistics/README.md](README.md)
- **Library index:** [README.md](../README.md)
