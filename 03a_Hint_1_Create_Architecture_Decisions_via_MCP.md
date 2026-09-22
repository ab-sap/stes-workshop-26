# Hint 1

Via MCP, you can import the Assessment Criteria into one or more Architecture Decisions and make it accessible to the EA Assistant. The prompt below is a (much longer than usual) example how a prompt to import the PDF Document into Architecture Decisions could look like. 


## Prompt: Import "Application Review – Top 20 Criteria" as LeanIX Architecture Decisions

> **Tooling:** Use the **LeanIX MCP Server** (`leanix-mcp`) for all workspace operations. No other MCP or manual API calls.
>
> **Source document:** *Application Review – Top 20 Criteria*

---

### 1. Parse the source

Extract all 20 rules. For each rule, capture:

- **Short name**
- **Rationale / intent**
- **Decision statement**
- **Consequences / trade-offs**

If the count ≠ 20, list what was found and **wait for confirmation** before proceeding.

### 2. Resolve the template

Use `get_architecture_templates` to resolve the template named **`Architecture Decision`**.

Then, via `get_architecture_template_components`, confirm the template exposes:

| Field | Type |
|---|---|
| `Context` | TEXT_AREA |
| `Decision` | TEXT_AREA |
| `Consequences` | TEXT_AREA |
| `Impacted Fact Sheets` | MULTI_FACTSHEET |
| `Approved by` | MULTI_USER |
| `Last Reviewed` | DATE_PICKER |
| `Related Documents` | — |

### 3. Resolve the current user

Use `search_users` (or equivalent) to fetch the UUID of the user running this import. This UUID will populate **`Approved by`** on every decision created below.

### 4. Create 20 Architecture Decisions

Call `create_architecture_decision` for each criterion, **status `ACCEPTED`**, with:

- **Title:** `App Review Criterion #<NN> – <Short Name>`
- **Context:** situation / rationale driving the criterion (why it matters in the application landscape)
- **Decision:** the measurable criterion / evaluation logic (what we assess and how)
- **Consequences:** what becomes easier or harder once this criterion is applied (trade-offs, follow-ups)
- **Approved by:** current user UUID (from step 3)
- **Last Reviewed:** `2026-09-17`

### 5. Create the Index decision

One additional `create_architecture_decision` call, **status `ACCEPTED`**:

- **Title:** `Application Review – Top 20 Criteria (Index)`
- **Context:** purpose and scope of the index
- **Decision:** numbered list of all 20 criteria with one-line summaries
- **Consequences:** how the index should be used and maintained
- **Approved by:** current user UUID (from step 3)
- **Last Reviewed:** `2026-09-17`
- **Related Documents:** link all 20 child decisions via `related_document_ids`

### 6. Report

Return a single table listing **ID, Title, and URL** for all 21 decisions. Skip duplicates by title.