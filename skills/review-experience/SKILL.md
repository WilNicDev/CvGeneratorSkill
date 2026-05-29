---
name: review-experience
description: >
  Review and improve a job experience file in cvDatabase/experience/. Detects
  duplications between responsibilities and achievements, checks skill consistency
  with skills_tools.md, ensures industry-standard terminology, identifies missing data,
  and proposes changes for user approval. Trigger when the user asks to review,
  audit, QA, or clean up an experience file.
---

# review-experience

Review a single experience file from `cvDatabase/experience/` against quality rules and the master skills catalogue in `cvDatabase/skills_tools.md`.

## Input

The user provides either:
- A filename (e.g. `2023-01_pwc.md`)
- A company name or keyword (search `cvDatabase/experience/` for a match)
- No argument — list available experience files and ask which one to review

## Review Steps

Run every step below, then present a **single consolidated report** with proposed changes. Do NOT apply any changes until the user approves.

### 1. Missing or Incomplete Data

Check front-matter for missing or empty fields. Every experience file should have:

| Field | Required | Notes |
|-------|----------|-------|
| `company` | Yes | Full legal or commonly known name |
| `role` | Yes | Industry-standard job title |
| `start_date` | Yes | YYYY-MM |
| `end_date` | Yes | YYYY-MM or "present" |
| `employment_type` | Yes | FTE, Contract, Freelance |
| `industry` | Yes | Use standard industry labels (e.g. "Financial Services", "Retail / FMCG") |
| `company_size` | Recommended | small / medium / large |
| `skills` | Yes | At least 3 competencies/capabilities/methodologies, display-ready names matching `skills_tools.md` |
| `tools_and_platforms` | Recommended | Named products, platforms, cloud services, databases, OS, programming languages used in this role |
| `topics` | Recommended | Short lowercase domain/technology/industry labels for filtering (e.g. `retail`, `B2C`, `IoT`) |
| `headline` | Yes | One-line summary of the engagement |
| `early_career` | Yes | true/false |

For each missing field, **ask the user** to provide the value rather than guessing.

### 2. Duplication Detection — Responsibilities vs Key Achievements

Compare the Responsibilities and Key Achievements sections:
- Flag any achievement that is a **near-restatement** of a responsibility (same action, same outcome, just reworded).
- Flag responsibilities that read like achievements (contain measurable outcomes or impact language that belongs in Achievements).
- Propose one of:
  - **Merge** — keep the stronger version in the correct section, remove the duplicate.
  - **Sharpen** — rewrite the achievement to focus on **measurable impact/outcome** while the responsibility states the **activity/scope**.

Present a side-by-side table:

```
| # | Responsibility excerpt | Achievement excerpt | Issue | Proposal |
```

### 3. Skill Consistency Check

Read `cvDatabase/skills_tools.md` and cross-reference:

#### 3a. Skills in experience not in master catalogue
For each skill in the experience `skills:` list, check if it appears (exact or near-match) in any category of `skills_tools.md`. Flag mismatches:
- **Terminology mismatch** — the skill exists under a different name (e.g. experience says "AI Strategy & Integration" but skills_tools.md says "AI Integration & Strategy"). Propose renaming to match the canonical name.
- **Missing from catalogue** — the skill is genuinely absent from `skills_tools.md`. Propose adding it to the appropriate category.

#### 3b. Skills mentioned in body but not in front-matter
Scan the Responsibilities and Achievements text for skill-like terms that appear in `skills_tools.md` but are **not** listed in the experience `skills:` field. Propose adding them.

#### 3c. Skills listed but not evidenced
Flag any skill in the experience `skills:` list that is **not mentioned or implied** anywhere in the body text. Ask the user whether to remove it or add supporting text.

Present findings as:

```
| Skill | Issue | Proposal |
```

### 4. Skills vs Tools & Platforms Classification

Verify that items are in the correct field:

- **`skills`** should only contain competencies, capabilities, and frameworks/methodologies (e.g. Enterprise Architecture, ArchiMate, SABSA, PRINCE2, Stakeholder Management). These must match `skills_tools.md`.
- **`tools_and_platforms`** should only contain named products, platforms, cloud services, databases, operating systems, and programming languages (e.g. Salesforce, AWS, SAP, Adobe Commerce, SQL Server, C#, .NET, Unix).

Flag any misclassification:
- A named product in `skills` → propose moving to `tools_and_platforms`
- A competency/methodology in `tools_and_platforms` → propose moving to `skills`
- A tool/platform mentioned in the body but absent from `tools_and_platforms` → propose adding

#### 4b. Tools & Platforms Catalogue Consistency

For each item in the experience `tools_and_platforms:` list, check if it appears (exact or near-match) in the `tools_and_platforms` section of `skills_tools.md`. Flag:
- **Terminology mismatch** — the tool exists under a different name. Propose renaming to match the canonical name.
- **Missing from catalogue** — the tool is genuinely absent from `skills_tools.md`. Propose adding it to the appropriate category.

Present findings as:

```
| Item | Current field | Correct field | Proposal |
```

### 5. Terminology & Wording Quality

- **Job title**: Is `role` using industry-standard terminology? (e.g. "Enterprise Architect" not "EA Guy")
- **Industry label**: Is `industry` using recognised sector names?
- **Headline**: Is it concise, specific, and free of buzzword overload?
- **Bullet quality**: Are bullets strong and action-led? (start with an action verb, describe scope, state outcome where applicable)
- **Tense rules** (strict):
  - **Responsibilities** must use **present tense** (e.g. "Define IT standards", "Lead architecture initiatives") — they describe the ongoing scope of the role.
  - **Key Achievements** must use **past tense** (e.g. "Established governance framework", "Delivered AI blueprints") — they describe completed outcomes.

Flag issues and propose rewrites.

### 6. Topic Coverage

Review the `topics` field:
- Suggest additional topics if the body text clearly covers a domain, technology, or industry that has no corresponding topic (e.g. role at a retailer but no `retail` topic).
- Flag topics that don't relate to any content in the file.
- Ensure topics are lowercase, short labels — not duplicates of skills already in the `skills` list.

## Output Format

Present the review as a structured report:

```
## Experience Review: {company} — {role}

### Missing Data
(list or "All fields present")

### Duplications
(table or "No duplications found")

### Skill Consistency
(table or "All skills consistent")

### Skills vs Tools Classification
(table or "All items correctly classified")

### Terminology & Wording
(issues + proposed rewrites, or "No issues")

### Topic Coverage
(suggestions or "Topics are well-covered")

### Summary of Proposed Changes
1. ...
2. ...
```

After presenting the report, ask:
> "Which changes would you like me to apply? (all / list numbers / none)"

Only then apply the approved changes to the experience file (and to `skills_tools.md` if new skills or tools were approved).
