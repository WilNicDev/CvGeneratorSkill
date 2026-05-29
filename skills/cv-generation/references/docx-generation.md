# Docx Generation

## Overview

Generate a styled Word document from a JSON data file using the `generate_docx.py` script.

## Prerequisites

- `uv` must be available on the system. Dependencies (`python-docx`) are declared inline in the script via PEP 723 metadata and resolved automatically by `uv run`.

## Workflow

1. Assemble CV data as JSON (schema below).
2. Write the JSON to `cv_data.json`.
3. Run: `uv run <skill-root>/scripts/generate_docx.py cv_data.json "<output-path>.docx"`

## JSON Schema

The input JSON file must follow this structure:

```json
{
  "name": "William Nicora",
  "target_headline": "Senior Enterprise Architect | Governance, Cloud, Portfolio Modernisation",
  "contact": {
    "phone": "+32479983254",
    "email": "william@digitalarchitecture.today",
    "city_country": "Vilvoorde, Belgium",
    "linkedin": "linkedin.com/in/wnicora"
  },
  "languages_line": "English C2 · French C2 · Italian C2 · Dutch B2",
  "profile_summary": [
    "First paragraph of tailored summary...",
    "Second paragraph...",
    "Third paragraph..."
  ],
  "key_skills": [
    "Enterprise Architecture",
    "Cloud Architecture & Adoption (AWS, Azure)",
    "... (exactly 12 entries)"
  ],
  "career_highlights": [
    {
      "company": "PwC",
      "text": "Appointed as Enterprise Architect to..."
    }
  ],
  "experiences": [
    {
      "date_range": "Oct 2025 to April 2026",
      "company": "Federale Assurance",
      "role": "Enterprise Architect",
      "employment_type": "Contract",
      "company_context": "Federale Assurance is a Belgian insurance company...",
      "responsibilities": [
        "Conducted an Application Portfolio Optimisation..."
      ],
      "achievements_heading": "Key Achievements:",
      "achievements": [
        "Designed IT-fit and business-fit assessment questionnaires..."
      ],
      "use_bullet_achievements": true,
      "additional_skills": ["Skill not in text above", "Another skill"],
      "platforms_and_tools": ["Tool not in text above"]
    }
  ],
  "early_career": [
    {
      "date_range": "1997 to 2000",
      "company": "Wang Global / Getronics",
      "role": "Consultant, Microsoft Practice",
      "employment_type": "FTE",
      "note": ""
    }
  ],
  "education": [
    {
      "programme": "Designing and Building AI Products and Services",
      "institution": "MIT",
      "year": 2023
    }
  ],
  "certifications": [
    {
      "certification": "Certified Enterprise Architect",
      "issuer": "The Open Group",
      "expired": false
    }
  ],
  "languages": [
    {
      "language": "English",
      "proficiency": "Fluent"
    }
  ],
  "additional_skills_tools": {
    "skills": ["Skill A", "Skill B"],
    "tools_and_platforms": ["Tool X", "Tool Y"]
  }
}
```

### Field Notes

- `target_headline`: The headline displayed directly below the candidate name. Format: `{Primary Role} | {2-3 key domain keywords}`. When tailoring, align to the target role.
- `contact`: Contact information. Uses `city_country` (e.g. "Vilvoorde, Belgium") instead of a full address. No postal code.
- `languages_line`: Compact languages string for the top of the CV (e.g. "English C2 · French C2 · Italian C2 · Dutch B2"). Set to `null` or omit if languages should not appear near the top. When this field is present, the standalone `languages` array is still included in JSON but the script will skip rendering the separate Languages section.
- `profile_summary`: An array of strings, one per paragraph. The original text from `profile.md` is included verbatim. An optional additional paragraph tailored to the job offer may be appended. Each paragraph is rendered as Cambria 11pt.
- `key_skills`: Exactly 12 entries. Rendered as a comma-separated list under "Core Expertise" heading (no table).
- `career_highlights`: Exactly 3 entries.
- `experiences`: All non-early-career experiences, ordered as they should appear. Each **must** include both `responsibilities` and `achievements` (if the database has them).
- `early_career`: **Only** entries with `early_career: true` in the database. Reverse chronological. Each includes `date_range` (year range string, e.g. "1997 to 2000") and is rendered as a bullet point.
- `achievements`: Required per experience when the database entry has them. Rendered as bullet points if `use_bullet_achievements` is true, otherwise as plain paragraphs.
- `additional_skills`: Optional per experience. Skills from the experience's `skills` front-matter not already referenced in responsibilities or achievements text. Rendered as "Additional skills:" followed by a comma-separated list. Omit or set to empty array if all skills are already mentioned.
- `platforms_and_tools`: Optional per experience. Tools/platforms from the experience's `tools_and_platforms` front-matter not already referenced in responsibilities or achievements text. Rendered as "Platforms & tools:" followed by a comma-separated list. Omit or set to empty array if all are already mentioned.
- `education`: Ordered with the master's degree first, then remaining entries by most recent year first. Each entry includes `year` (integer).
- `certifications`: All certifications from the database. Each entry includes `expired` (boolean). Rendered in the same section as education.
- `languages`: All languages from the database.
- `additional_skills_tools`: Skills and tools/platforms **not already mentioned anywhere** in the CV (not in experience text, not in per-experience "Additional skills" or "Platforms & tools" sub-sections). Only items appearing nowhere else belong here. Omit the key entirely (or set to null) if nothing to add. Either sub-list can be an empty array if only one category has entries.

## Python Script

The generation script is at `scripts/generate_docx.py` (relative to the skill root). Run it directly from its location:

```bash
uv run <skill-root>/scripts/generate_docx.py cv_data.json "<output-path>.docx"
```

**Do not inline the script in this markdown file.** Edit `scripts/generate_docx.py` directly when making changes to the document generation logic.
