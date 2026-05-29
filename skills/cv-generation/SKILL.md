---
name: cv-generation
description: >
  This skill should be used when the user asks to "generate a CV",
  "create a resume", "tailor my CV for a job", "make a CV for this job offer",
  or needs guidance on CV generation from the structured database.
  It contains the template structure, matching algorithm, and generation rules.
version: 0.8.0
---

# CV Generation Skill

Generate tailored CVs for specific job offers using structured data in `cvDatabase/`.

## Critical Rule: Grounding

Generated CVs must be **strictly grounded** to the data in `cvDatabase/`. Information from a job offer may only be used to **filter, reorder, or highlight** existing experiences, skills, and qualifications. Never invent or add skills, experiences, certifications, or achievements not present in the database.

## ATS & Layout Rules

The CV must be **ATS-safe and recruiter-friendly**, optimised for senior profiles with long career histories.

**Layout constraints:**
- Clean, single-column layout. No sidebars.
- No tables for core CV content (skills, experience, education, etc.).
- No icons, charts, graphics, profile photos, or decorative elements.
- No text boxes, multi-column sections, or header/footer-only contact information.
- Use standard headings and body text so the document remains ATS-readable.

**Top-third rule:** The highest-value information must appear in the top third of page one. The section order enforces this — see Section Order below.

## Constraints

- Maximum 4 pages in the final document.
- **All experiences from the database must always be included.** No experience is ever omitted.
  - Experiences with `early_career: false` (or absent) are rendered as **full detailed entries** in "Professional Experience", including both responsibilities and key achievements. Do not omit achievements to save space — trim responsibility count instead if needed.
  - Experiences with `early_career: true` are rendered in the condensed **"Earlier Career"** bullet list (company, role, employment type, optional one-line note).
- Profile summary must use the text from `profile.md` **verbatim**. When tailoring for a job offer, an additional paragraph may be appended that emphasises aspects most relevant to the target role — but the original text must not be rewritten or removed. The additional paragraph must remain grounded to facts in the database.
- Career Highlights (top 3) must be selected from the 3 experiences whose achievements best demonstrate fit for the target role. The **default text** for each highlight is the `career_highlight` field from the corresponding experience file — this text must be used verbatim unless a tailored alternative is approved by the user. When tailoring for a job offer, the skill may **propose an alternative highlight narrative** that better emphasises aspects relevant to the target role; however, alternative highlights require **explicit user approval** before being used in the generated CV. If the user does not approve an alternative, the standard `career_highlight` text from the experience file must be used. All highlight text — whether standard or alternative — must be grounded to facts in the experience files.
- Each detailed experience entry includes two optional sub-sections after key achievements:
  - **"Additional skills:"** — skills from the experience's `skills` front-matter that are **not** already referenced in the responsibilities or key achievements text of that experience.
  - **"Platforms & tools:"** — tools/platforms from the experience's `tools_and_platforms` front-matter that are **not** already referenced in the responsibilities or key achievements text of that experience. Omit this sub-section entirely if all tools are already mentioned.
- Two final sections at the bottom of the document:
  - **"Additional Skills"** — skills from the `categories` section of `skills_tools.md` that are **not already mentioned in any experience entry** (including the per-experience "Additional skills" sub-sections). Only skills that appear nowhere else in the CV belong here.
  - **"Tools & Platforms"** — tools/platforms from the `tools_and_platforms` section of `skills_tools.md` that are **not already mentioned in any experience entry** (including the per-experience "Platforms & tools" sub-sections). Only items that appear nowhere else in the CV belong here.

## Workflow

### Step 1 — Ingest the job offer

Accept the job offer as pasted text, an uploaded file, or a URL. Extract:
- Target role title and seniority
- Required and preferred skills
- Required and preferred tools/platforms
- Industry/domain
- Key responsibilities
- Years of experience expected

### Step 2 — Read the database

Read all files from the `cvDatabase/` folder:
- `profile.md` — name, title, contact, summary
- `skills_tools.md` — master skills catalogue with categories
- `languages.md` — language proficiencies
- `experience/*.md` — all experience files
- `education/*.md` — all education files
- `certifications/*.md` — all certification files

### Step 3 — Match and rank (for career highlights and skills selection only)

Compute a relevance score for each experience against the job offer. This ranking is used **only** to:
- Select the **top 3 career highlights**
- Select the **top 12 key skills**
- Prioritise which skills/tools appear in the appendix

Score based on:
1. **Skills overlap** — how many of the experience's `skills` match the job requirements
2. **Tools/platforms overlap** — how many `tools_and_platforms` match
3. **Topics overlap** — how many `topics` match the job domain/industry
4. **Role similarity** — how close the role title/seniority is to the target

For education and certifications, match by `skills`, `topics`, and `field`.

**Ranking must never affect experience order.** Experiences are always listed in reverse chronological order.

### Step 4 — Present selection to user for review

Use `AskUserQuestion` to present the matched results and let the user adjust before generation. Present:

1. **All non-early-career experiences** — in reverse chronological order (fixed). All are always included; the user cannot remove or reorder entries.
2. **All early career roles** (`early_career: true`) in reverse chronological order.
3. **Top 12 core expertise skills** selected from the master catalogue, ranked by relevance.
4. **Education entries** to include.
5. **Certifications** to include.
6. **Career highlights** — proposed top 3 experiences to feature as highlights, with brief rationale for why each best matches the job profile. For each selected experience, show:
   - The **standard highlight** (from the experience's `career_highlight` field) — this is the default that will be used.
   - Optionally, a **proposed alternative highlight** that better emphasises aspects relevant to the target role. Clearly label it as "Alternative (requires approval)".
   - Ask the user to confirm whether to use the standard or the alternative for each highlight. If no explicit approval is given for an alternative, use the standard.
7. **Additional Skills** — skills not already covered by included experiences.
8. **Tools & Platforms** — tools/platforms from `skills_tools.md` not already covered by included experiences.

Ask the user to confirm or adjust (remove/add/reorder).

### Step 5 — Compose the CV content

Assemble the CV content as structured data following the template in `references/template-structure.md`.

For each detailed experience entry, after key achievements add:
- **"Additional skills:"** — skills from the experience's `skills` front-matter not already referenced in the responsibilities or key achievements text.
- **"Platforms & tools:"** — tools/platforms from the experience's `tools_and_platforms` front-matter not already referenced in the responsibilities or key achievements text. Omit if all are already mentioned.

To build the bottom "Additional Skills" section:
1. Collect all skills already mentioned anywhere in the CV (responsibilities text, key achievements text, and per-experience "Additional skills" sub-sections).
2. From the `categories` section of `skills_tools.md`, select skills that are **not** in set (1). Filter to those relevant to the target role (or include all for a generic CV).
3. Render as a comma-separated inline list. If empty, omit the section.

To build the bottom "Tools & Platforms" section:
1. Collect all tools/platforms already mentioned anywhere in the CV (responsibilities text, key achievements text, and per-experience "Platforms & tools" sub-sections).
2. From the `tools_and_platforms` section of `skills_tools.md`, select items that are **not** in set (1). Filter to those relevant to the target role (or include all for a generic CV).
3. Render as a comma-separated inline list. If empty, omit the section.

### Step 6 — Generate outputs

1. **Word (.docx)** — Write CV data as JSON to `cv_data.json`, then run: `uv run scripts/generate_docx.py cv_data.json "<output-path>.docx"`. Dependencies are declared inline via PEP 723 and resolved automatically by `uv`. See `references/docx-generation.md` for the JSON schema.
2. **Markdown** (optional) — Only generate a markdown version if the user explicitly requests it.

Output files to the workspace folder with naming convention:
`William Nicora - CV - {YYYY-MM} - {company-slug}.{ext}`

### Step 7 — Clean up temporary files

After the document has been generated successfully, delete the intermediate `cv_data.json` file from the workspace folder. This file is only needed during generation and should not be kept.

## Additional References

- **`references/template-structure.md`** — exact CV template layout and section specifications
- **`references/docx-generation.md`** — JSON schema and usage instructions
- **`scripts/generate_docx.py`** — Python script that generates the styled .docx from JSON data
