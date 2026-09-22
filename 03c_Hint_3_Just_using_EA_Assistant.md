# Hint 3
With all necessary files (Application Documentation and Application Assessment Criteria) being accessible to the EA Assistant, the Assistant can do the Assessment purely based on the PDF. It's sufficient to have them uploaded to the workspace files or fact sheet resources. The prompt below outlines this.

## Application Assessment: MaMa CRM

Evaluate **MaMa CRM** against the Top 20 assessment criteria. Produce an evidence table, summary, and gap register.

### Sources (authorised only — no external knowledge)
| # | Source |
|---|--------|
| 1 | `Application Review — Top 20 Criteria.pdf` — the 20 criteria definitions and pass/fail thresholds |
| 2 | LeanIX inventory — `MaMa CRM` fact sheet and all related fact sheets |
| 3 | `mama-crm-arc42.pdf` |

### Steps

**1 — Load criteria.** Read `Application Review — Top 20 Criteria.pdf`. For each criterion, the defined threshold is the authoritative pass/fail bar.

**2 — Load LeanIX inventory.** On the `MaMa CRM` fact sheet retrieve: `lifecycle`, `businessCriticality`, `functionalSuitability`, `technicalSuitability`, `lxSixRClassification`, `lxHostingType`, `lxApplicationTotalCostOfOwnership`, `lxStatusSSO`, `completion.percentage`, `tags`, `subscriptions` (all roles). Traverse: `relApplicationToITComponent` (+ each component's lifecycle for EOL), `relApplicationToBusinessCapability`, `relProviderApplicationToInterface`, `relConsumerApplicationToInterface`, `relApplicationToDataObject`, `relApplicationToOrganization`, `relApplicationToInitiative`, `relToParent`/`relToChild`. Record "0 edges" or "null" explicitly for every empty field or relation.

**3 — Evaluate each criterion.** For each of the 20, in order:
- Find evidence in the PDF (record §section + heading) and in LeanIX (record fact sheet + field + value).
- Assign **Pass** / **Gap** / **Conflict**. Gap is the default — Pass requires explicit proof. Conflicting sources block both verdicts; mark ⚠️ Conflict.
- Score confidence 1–10: corroborating proof from both sources = 9–10; one explicit source = 7–8; intent/indirect only = 5–6; conflict or inferred = 3–4; no evidence = 1–2. Flag 🔴 Low Confidence if ≤ 4.
- For security and compliance criteria: self-attestation in the PDF caps confidence at 8; only third-party verification reaches 9–10.

**4 — Output.**
- **Table**: `# | Criterion | Domain | Result | Rationale (≤3 sentences, every claim attributed) | PDF Evidence (§N heading) | LeanIX Evidence (FS · field · value) | Confidence`
- **Summary**: Pass/Gap/Conflict counts · average confidence · criteria with confidence ≤ 4.
- **Top 3 gaps**: ranked by regulatory exposure → operational risk → technical debt → governance. For each: gap title, ranking reason, recommended action + owner role, whether an active Initiative exists in LeanIX.
- **LeanIX data quality actions**: every null field and zero-edge relation, which criterion it affects, recommended action.
- **Discrepancies**: every criterion where PDF and LeanIX gave different values, with recommended resolution.
