# Hint 2 - The EA Assistant can read and understand Architecture Decisions

The EA Assistant can read the Architecture Decisions in your workspace. After an import, you can just have him check each of these. The prompt below is a (very long) example how such a prompt could look like. You can paste it in full or run your own version; I have added a lot of details so that it would also be understandable for humans who read it. 

## Application Assessment: MaMa CRM

Evaluate **MaMa CRM** against the Top 20 criteria. Produce an evidence table, summary, and gap register.

### Sources (authorised only — no external knowledge)
| # | Source |
|---|--------|
| 1 | LeanIX ADRs on Application Review |
| 2 | LeanIX inventory — `MaMa CRM` fact sheet and all related fact sheets |
| 3 | `mama-crm-arc42.pdf` |

### Steps

**1 — Load criteria.** Call `get_architecture_decision` for Application Review Architecture Decisions. Read its `relatedDocuments` array. For every criterion ADR: the **`Decision`** field is the authoritative pass/fail threshold; the **`Context`** field is the rationale.

**2 — Load LeanIX inventory.** On the `MaMa CRM` fact sheet retrieve: `lifecycle`, `businessCriticality`, `functionalSuitability`, `technicalSuitability`, `lxSixRClassification`, `lxHostingType`, `lxApplicationTotalCostOfOwnership`, `lxStatusSSO`, `completion.percentage`, `tags`, `subscriptions` (all roles). Traverse: `relApplicationToITComponent` (+ each component's `relITComponentToProvider` for vendor), `relApplicationToBusinessCapability`, `relProviderApplicationToInterface`, `relConsumerApplicationToInterface`, `relApplicationToDataObject`, `relApplicationToOrganization`, `relApplicationToInitiative`, `relToParent`/`relToChild`. Record "0 edges" or "null" explicitly for every empty field or relation.

**3 — Evaluate each criterion.** For each of the 20, in order:
- Find evidence in the PDF (record §section + heading) and in LeanIX (record fact sheet + field + value).
- Assign **Pass** / **Gap** / **Conflict**. Gap is the default — Pass requires explicit proof. Conflicting sources block both verdicts; mark ⚠️ Conflict.
- Score confidence 1–10: corroborating proof from both sources = 9–10; one explicit source = 7–8; intent/indirect only = 5–6; conflict or inferred = 3–4; no evidence = 1–2. Flag 🔴 Low Confidence if ≤ 4.
- For ADR-35 (Security) and ADR-38 (Compliance): self-attestation in the PDF caps confidence at 8; only third-party verification reaches 9–10.

**4 — Output.**
- **Table**: `# | ADR ID | Criterion | Domain | Result | Rationale (≤3 sentences, every claim attributed) | PDF Evidence (§N heading) | LeanIX Evidence (FS · field · value) | Confidence`
- **Summary**: Pass/Gap/Conflict counts · average confidence · gate-criteria status for ADR-30 and ADR-49 (both must Pass for investment decisions).
- **Top 3 gaps**: ranked by regulatory exposure → operational risk → technical debt → governance. For each: gap title, ranking reason, recommended action + owner role, whether an active Initiative exists in LeanIX.
- **LeanIX data quality actions**: every null field and zero-edge relation, which ADR it affects, recommended action.
- **Discrepancies**: every criterion where PDF and LeanIX gave different values, with recommended resolution.
