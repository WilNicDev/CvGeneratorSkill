# CV Template Structure

Exact layout of the CV template. Every generated CV must follow this structure.

## ATS & Layout Principles

This template is designed to be **ATS-safe and recruiter-friendly** for senior profiles with long career histories. All layout decisions serve two goals: (1) machine-parseable by Applicant Tracking Systems, and (2) scannable by a human recruiter in under 30 seconds.

**Mandatory layout rules:**
- Clean, single-column layout. No sidebars.
- No tables for core CV content (skills, experience, education, etc.).
- No icons, charts, graphics, profile photos, or decorative elements.
- No text boxes, multi-column sections, or header/footer-only contact information.
- Use standard headings (`Heading 1`, `Heading 2`) and body text (`Normal`) so the document remains ATS-readable.
- Contact information must appear in the document body, not only in headers/footers (ATS often ignores headers/footers).

## Document Layout

- **Page size:** A4 (210 x 297 mm)
- **Margins:** Top 1 cm, Bottom 1 cm, Left 1.27 cm, Right 1.75 cm
- **Default font:** Cambria 11pt (body text and headings)
- **Maximum pages:** 4

## Section Order

The top third of page one must contain the highest-value information. Sections are ordered accordingly:

1. Candidate Name
2. Target Headline
3. Contact Line
4. Languages Line (when strategically relevant)
5. Professional Summary
6. Core Expertise (top 12 skills)
7. Career Highlights (top 3 selected impacts)
8. Professional Experience (detailed roles — all non-early-career experiences)
9. Earlier Career (condensed list — **only** experiences with `early_career: true`)
10. Education / Qualifications
11. Additional Skills (appendix — skills from `skills_tools.md` not already mentioned in experiences)
12. Tools & Platforms (appendix — tools from `skills_tools.md` not already mentioned in experiences)

## Section Specifications

### 1. Candidate Name

A plain paragraph — no table, no background shading, no banner.
- Name in Cambria 22pt bold, dark blue (#1F3864).
- Spacing after: 0pt (tight coupling with headline below).

### 2. Target Headline

A plain paragraph immediately below the name.
- Cambria 12pt bold, dark blue (#1F3864).
- Format: `{Primary Role} | {2-3 key domain keywords separated by commas}`
- Example: `Senior Enterprise Architect | Governance, Cloud, Portfolio Modernisation`
- When tailoring for a job offer, align the headline to the target role title and its most prominent themes. Must remain grounded to the candidate's actual expertise.
- Spacing after: 2pt.

### 3. Contact Line

Single paragraph, Cambria 10pt.
Format: `{City}, {Country} | {phone} | {email} | {linkedin_display}`
Where `linkedin_display` is the path portion only (e.g. `linkedin.com/in/wnicora`).
- Do **not** include a full postal address — city and country only.
- Separators: vertical bar ` | ` (not semicolons).
- Spacing after: 2pt.

### 4. Languages Line

Single paragraph, Cambria 10pt.
Format: `Languages: {Lang1} {CEFR} · {Lang2} {CEFR} · ...`
- Example: `Languages: English C2 · French C2 · Italian C2 · Dutch B2`
- Use middle dot ` · ` as separator.
- Include this line when multilingual proficiency is strategically relevant (e.g. European senior profiles, roles requiring multilingual communication). Omit for roles where languages are not a differentiator.
- When this line is included, the standalone "Languages" section (section 10 in the old layout) is **omitted** to avoid duplication.
- Spacing after: 6pt.

### 5. Professional Summary

Section heading: "Professional Summary" — Cambria 14pt bold, #1F3864, bottom border.
Body: Use the summary text from `profile.md` **verbatim**, preserving its natural paragraph structure. Cambria 11pt, single-spaced, 60 twips after each paragraph. When tailoring for a job offer, an additional paragraph may be appended that emphasises aspects most relevant to the target role — but the original text must not be rewritten or removed. The additional paragraph must remain grounded to facts in the database.

### 6. Core Expertise

Section heading: "Core Expertise" — Cambria 14pt bold, #1F3864, bottom border, 360 twips space-before.
Content: A single paragraph with all 12 skills as a comma-separated list, Cambria 11pt. **No tables.** Select the 12 most relevant skills from `skills_tools.md` for the target role, ordered by relevance.

### 7. Career Highlights

Section heading: "Career Highlights" — same heading style.
Content: Exactly 3 highlight paragraphs. Each starts with **Company name:** in bold, followed by a concise narrative (2-4 sentences) of the most impactful contributions at that role.

**Selection logic:** Choose the 3 experiences whose achievements best demonstrate fit for the target job offer. Any non-early-career experience can become a career highlight. For a generic CV (no job offer), select the 3 most impactful experiences.

**Highlight text source:** Each experience file contains a `career_highlight` field in its front-matter. This is the **default text** that must be used verbatim for the highlight narrative. When tailoring for a job offer, an **alternative highlight** may be proposed that better emphasises aspects relevant to the target role — but this alternative requires **explicit user approval** before it can replace the standard text. If not approved, the `career_highlight` field text is used. All text — standard or alternative — must be grounded to facts in the experience file.

### 8. Professional Experience

Section heading: "Professional Experience" — same heading style, 360 twips space-before.

**All experiences where `early_career` is `false` (or absent) must be included — none may be omitted.** This includes mid-career roles like Microsoft, Huntsman, Electrabel, Bpost, Infrax — not just the recent ones. **Always order by reverse chronological date** — never reorder by relevance.

**Visual separator:** A light-grey horizontal rule (4-point, #BFBFBF) is rendered between consecutive experience entries for readability. No rule before the first entry.

For each experience:

**Role header:** Bold, Cambria 11pt.
Format: `{date_range}: {company} – {role} ({employment_type})`
Date format: `Mon YYYY to Mon YYYY` (e.g. "Oct 2025 to April 2026"). Use "Present" for current roles.
Space-before: 160 twips for the first entry; 40 twips for subsequent entries (after the horizontal rule).

**Company context:** One paragraph (Normal style, Cambria 11pt) with a brief description of the company. Take from the experience file body.

**Responsibilities:** Bold heading "Responsibilities:" followed by a bullet list (ListParagraph / List Bullet style) of each responsibility from the experience file. Same visual format as Key Achievements. Include all responsibilities from the database. When tailoring for a job, reorder so the most relevant appear first; trim only if strictly necessary for the 4-page limit.

**Key Achievements:** Bold heading "Key Achievements:" followed by the achievements from the experience file. **Always include this sub-section if the experience has achievements** — do not omit to save space. Render as bullet points (ListParagraph style) or as individual paragraphs depending on the experience.

**Additional skills:** Bold heading "Additional skills:" followed by a comma-separated inline list of skills from the experience's `skills` front-matter that are **not** already referenced in the responsibilities or key achievements text of that experience. Omit this sub-section if all skills are already mentioned above. **Spacing:** 120 twips space-before this paragraph to visually separate it from the key achievements above.

**Platforms & tools:** Bold heading "Platforms & tools:" followed by a comma-separated inline list of tools/platforms from the experience's `tools_and_platforms` front-matter that are **not** already referenced in the responsibilities or key achievements text of that experience. Omit this sub-section entirely if all tools/platforms are already mentioned above.

### 9. Earlier Career

Section heading: "Earlier Career" — same heading style, 360 twips space-before.

**All experiences where `early_career: true` in the front-matter must be included.** These are pre-2000 roles (Technical Systems, Chorus, Riverland, Wang/Getronics). None may be omitted.

Content: Bullet list (ListParagraph style), reverse chronological. Each entry on one line:
`{date_range}: {Company} – {Role} ({employment_type})`
Optionally append ` — {one-line note}` for notable contributions.

### 10. Education / Qualifications

Section heading: "Education / Qualifications" — same heading style.
Content: Bullet list (ListParagraph / List Bullet style), one entry per bullet:
`{programme}: {institution} ({year})`
Order: master's degree first, then remaining entries by most recent year first.

**Certifications** sub-section follows immediately after education entries. Bold heading "Certifications:" then bullet list:
`{certification}: {issuer}`
Append " (expired)" after the certification name when `status: expired` in the database.

### 11. Additional Skills

Section heading: "Additional Skills" — same heading style, 360 twips space-before.

This section lists skills from the `categories` section of `skills_tools.md` that are **not already mentioned anywhere** in the CV — not in responsibilities, not in key achievements, and not in the per-experience "Additional skills" sub-sections. Only skills that appear nowhere else in the document belong here.

**How to determine what goes here:**
1. Collect every skill already mentioned in the CV: from responsibilities text, key achievements text, and "Additional skills" sub-sections of all experience entries.
2. From the `categories` section of `skills_tools.md`, list skills NOT in set (1).
3. Exclude items that are clearly outdated or irrelevant to the candidate's current profile.

**Layout:** Comma-separated inline list (not bullet points) to save space. Cambria 11pt. If empty, omit the entire section.

### 12. Tools & Platforms

Section heading: "Tools & Platforms" — same heading style, 360 twips space-before.

This section lists tools and platforms from the `tools_and_platforms` section of `skills_tools.md` that are **not already mentioned anywhere** in the CV — not in responsibilities, not in key achievements, and not in the per-experience "Platforms & tools" sub-sections. Only items that appear nowhere else in the document belong here.

**How to determine what goes here:**
1. Collect every tool/platform already mentioned in the CV: from responsibilities text, key achievements text, and "Platforms & tools" sub-sections of all experience entries.
2. From the `tools_and_platforms` section of `skills_tools.md`, list items NOT in set (1).
3. Exclude items that are clearly outdated or irrelevant to the candidate's current profile.

**Layout:** Comma-separated inline list (not bullet points) to save space. Cambria 11pt. If empty, omit the entire section.

## Tailoring Guidelines

When tailoring for a job offer:

1. **Target headline:** Align the headline to the target role title and its most prominent themes. Must remain grounded to the candidate's actual expertise.

2. **Professional summary:** Use the text from `profile.md` verbatim. Optionally append one additional paragraph that emphasises aspects most relevant to the job. Keep the "hands-on, pragmatic, lean" identity. Do not rewrite or remove the original text.

3. **Core Expertise:** Select the 12 skills that best match the job requirements. If the job mentions skills not in the catalogue, find the closest existing skill.

4. **Career Highlights:** Select the 3 experiences whose achievements best match the job profile. Use the `career_highlight` field from each experience file as the default text. If a tailored alternative would better serve the target role, propose it alongside the standard text and obtain explicit user approval before using it. Any non-early-career experience is eligible.

5. **Experience ordering:** Always reverse chronological. Relevance ranking is used only to select career highlights and core expertise — never to reorder experiences.

6. **Responsibility selection:** All experiences are always included. Within each, reorder responsibilities so the most job-relevant appear first. Trim responsibility count per role only if necessary for the 4-page limit — never trim achievements, and never drop an entire experience.

7. **Additional Skills / Tools & Platforms:** Prioritise listing skills and tools that are relevant to the target role but happen not to appear in the experience entries.

8. **Languages line:** Include the compact languages line near the top when multilingual proficiency is relevant to the target role. When included, omit the standalone Languages section to avoid duplication.

9. **Page budget:** Header through Core Expertise typically takes ~0.5 pages. Career Highlights ~0.5 pages. Allocate the remaining space for Professional Experience (including all non-early-career roles), Earlier Career, Education, Additional Skills, and Tools & Platforms.
