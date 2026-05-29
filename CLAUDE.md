# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This project uses AI skills to generate tailored CVs for specific job offers, based on structured data in a `cvDatabase/` folder. Each user populates their own `cvDatabase/` with their career data. A sample database is provided in `cvDatabase-sample/` as a starting point.

## Critical Rule: Grounding

Generated CVs must be **strictly grounded** to the data in `cvDatabase/`. Information from a job profile may only be used to **filter, reorder, or highlight** existing experiences, skills, and qualifications — never to invent or add skills, experiences, certifications, or achievements that are not present in the database.

## cvDatabase Structure

All source data lives in `cvDatabase/` using markdown files with YAML front-matter:

```
cvDatabase/
├── profile.md              # Name, title, contact info, professional summary
├── skills_tools.md         # Master catalogue of skills (by category) and tools & platforms (by category)
├── languages.md            # Language proficiencies with CEFR levels
├── experience/             # One file per role, named {start-date}_{company-slug}.md
├── education/              # One file per programme
└── certifications/         # One file per certification
```

New users should copy `cvDatabase-sample/` to `cvDatabase/` and replace the placeholder content with their own career data. See the sample files for the expected YAML front-matter fields and body structure.

### Experience file conventions

- Front-matter includes: `company`, `role`, `start_date`, `end_date`, `employment_type`, `industry`, `skills`, `tools_and_platforms`, `topics`, `headline`, `career_highlight`, and `early_career` (boolean — true for early roles that should be summarised rather than detailed).
- The `skills` field lists competencies, capabilities, and methodologies using display-ready names that must match the master catalogue in `skills_tools.md`. Skills are used both for CV output and for filtering experiences by relevance to a job offer.
- The `tools_and_platforms` field lists named products, platforms, cloud services, databases, operating systems, and programming languages used in the role (e.g. `SAP Hybris`, `AWS (EC2, S3, RDS)`, `Adobe Commerce (Magento)`, `C#`). These must also match entries in the `tools_and_platforms` section of `skills_tools.md`.
- The `topics` field contains short lowercase domain/technology/industry labels (e.g. `retail`, `B2C`, `IoT`) that aid filtering but are not formal skills or tools.
- Body contains company context, responsibilities, and achievements.

## CV Generation Workflow

1. **Input:** A job offer (text, URL, or file) describing the target role.
2. **Match:** Map job requirements to `skills`, `tools_and_platforms`, and `topics` across experience, education, and certification files to identify the most relevant roles, skills, and qualifications.
3. **Compose:** Assemble a tailored CV using `profile.md` for the header/summary, selected experiences (reordered/highlighted by relevance), matching education and certifications, relevant skills, and languages.
4. **Output:** Word document (.docx). Can also generate Markdown if requested.

## Skills

All skills live in the `skills/` folder. This project includes three Claude Code skills:

- **`skills/cv-import/`** — Extract career data from an existing CV (PDF or Word) and populate `cvDatabase/`
- **`skills/cv-generation/`** — Main CV generation workflow (ingest job offer, match, compose, generate .docx)
- **`skills/review-experience/`** — Quality assurance for experience files (duplication detection, skill consistency, terminology checks)

Recommended workflow: **import** (populate database) -> **review** (QA each experience) -> **generate** (tailored CV for a job offer)
