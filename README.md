# CvGeneratorSkill

AI-powered CV generation using [Claude Code](https://claude.ai/code) skills. Tailors your CV to specific job offers by matching structured career data against job requirements — without inventing or fabricating any information.

## Overview

CvGeneratorSkill is a set of Claude Code skills that generate professional, ATS-friendly CVs from a structured markdown database of your career history. Given a job offer, the skills analyse requirements, match them against your experiences, skills, and qualifications, and produce a tailored CV as a Word document (.docx).

**Key principle:** Generated CVs are strictly grounded to your data. Job offer information is only used to filter, reorder, and highlight existing experiences — never to invent skills or achievements.

## Features

- **CV Generation Skill** — End-to-end workflow: ingest a job offer, match against your career data, compose a tailored CV, and generate a styled .docx
- **Experience Review Skill** — Quality assurance for your career database: detects duplications, checks skill consistency, validates terminology, and proposes improvements
- **Structured Database** — Markdown files with YAML front-matter for profile, experiences, skills, education, certifications, and languages
- **ATS-Friendly Output** — Single-column, clean typography, no tables or graphics — optimised for Applicant Tracking Systems
- **Interactive Workflow** — User approval at key steps (career highlights selection, skill adjustments) before generating the final document

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed and configured
- [uv](https://docs.astral.sh/uv/) (Python package manager) — used to run the .docx generation script

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/wilnicdev/CvGeneratorSkill.git
cd CvGeneratorSkill
```

### 2. Set up your career database

Copy the sample database and replace with your own data:

```bash
cp -r cvDatabase-sample cvDatabase
```

Edit the files in `cvDatabase/` with your career information. See [Database Structure](#database-structure) for the expected format.

### 3. Install the skills

The skills are located in `cv-generation/` and `review-experience/`. To make them available in Claude Code, copy them to your Claude Code skills directory:

```bash
cp -r cv-generation ~/.claude/skills/cv-generation.1.0
cp -r review-experience ~/.claude/skills/review-experience
```

### 4. Generate a CV

Open Claude Code in the project directory and use natural language:

```
Generate a CV for this job offer: [paste job description or provide URL]
```

Or use the skill trigger phrases:

- *"Generate a CV for this job offer"*
- *"Create a resume tailored to this position"*
- *"Make a CV for this role"*

### 5. Review your experience data (optional)

```
Review the experience file for Acme Corp
```

## Database Structure

All career data lives in `cvDatabase/` as markdown files with YAML front-matter:

```
cvDatabase/
├── profile.md              # Name, title, contact info, professional summary
├── skills_tools.md         # Master catalogue of skills and tools/platforms
├── languages.md            # Language proficiencies with CEFR levels
├── acronyms.md             # Reference list of acronyms (optional)
├── experience/             # One file per role
│   └── {start-date}_{company-slug}.md
├── education/              # One file per programme
│   └── {institution-slug}.md
└── certifications/         # One file per certification
    └── {certification-slug}.md
```

### Experience files

Each experience file uses this structure:

```yaml
---
early_career: false
company: Acme Corp
role: Solutions Architect
start_date: 2023-01
end_date: present
employment_type: FTE          # FTE, Contract, or Freelance
industry: Financial Services
company_size: large           # small, medium, or large
topics:
  - fintech
  - cloud-migration
headline: Cloud-native platform modernisation
career_highlight: Led the architecture of a cloud-native platform serving 2M+ users...
skills:                        # Must match skills_tools.md catalogue
  - Solutions Architecture
  - Cloud Architecture
tools_and_platforms:           # Must match skills_tools.md catalogue
  - AWS (EC2, S3, Lambda, RDS)
  - Kubernetes
---

Company context paragraph.

## Responsibilities
- Bullet points in present tense...

## Key Achievements
- Bullet points in past tense...
```

See `cvDatabase-sample/` for complete examples of all file types.

## Skills Reference

### cv-generation

Generates a tailored CV from your career database for a specific job offer.

**Triggers:** *"generate a CV"*, *"create a resume"*, *"tailor my CV"*, *"make a CV for this job offer"*

**Workflow:**
1. Ingest the job offer (text, file, or URL)
2. Read all files from `cvDatabase/`
3. Match and rank experiences against job requirements
4. Present selection to user for review and approval
5. Compose CV content following the template structure
6. Generate styled .docx output

### review-experience

Reviews and improves experience files in `cvDatabase/experience/`.

**Triggers:** *"review experience"*, *"audit experience"*, *"QA experience file"*, *"clean up experience"*

**Checks performed:**
- Missing or incomplete front-matter fields
- Duplications between responsibilities and achievements
- Skill consistency with the master catalogue
- Correct classification (skills vs tools)
- Terminology and wording quality
- Topic coverage

## Customisation

### Adding experiences

Create a new file in `cvDatabase/experience/` following the naming convention `{YYYY-MM}_{company-slug}.md`. Use an existing file or the sample as a template.

### Managing skills and tools

The master catalogue in `cvDatabase/skills_tools.md` defines all valid skill and tool names. Experience files must reference skills and tools using the exact names from this catalogue. Add new entries to the catalogue before using them in experience files.

### Early career roles

Set `early_career: true` in the front-matter for older roles that should be condensed to a single bullet line rather than a full detailed entry.

## Project Structure

```
CvGeneratorSkill/
├── README.md                  # This file
├── LICENSE                    # MIT License
├── CONTRIBUTING.md            # Contribution guidelines
├── CLAUDE.md                  # Claude Code guidance
├── .gitignore
├── cv-generation/             # CV generation skill
│   ├── SKILL.md               # Skill definition and workflow
│   ├── references/            # Template specs and JSON schema
│   └── scripts/               # Python .docx generation script
├── review-experience/         # Experience review skill
│   └── SKILL.md
├── cvDatabase/                # Your career data (not tracked in git for forks)
└── cvDatabase-sample/         # Anonymised example database
```

## License

This project is licensed under the [MIT License](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on reporting issues and submitting pull requests.
