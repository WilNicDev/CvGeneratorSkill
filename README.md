# CvGeneratorSkill

AI-powered CV generation using agent skills. Tailors your CV to specific job offers by matching structured career data against job requirements — without inventing or fabricating any information.

## Overview

CvGeneratorSkill is a set of agent skills that generate professional, ATS-friendly CVs from a structured markdown database of your career history. Given a job offer, the skills analyse requirements, match them against your experiences, skills, and qualifications, and produce a tailored CV as a Word document (.docx).

The skills follow the **standard agent skill format** (markdown SKILL.md with YAML front-matter) and are compatible with any tool that supports this convention, including [Claude Code](https://claude.ai/code), [Claude Cowork](https://claude.ai), [Codex](https://github.com/openai/codex), and other AI coding agents. The skills are actively developed and tested using the Claude ecosystem (Claude Code CLI, Claude Code desktop app, and Claude Cowork).

**Key principle:** Generated CVs are strictly grounded to your data. Job offer information is only used to filter, reorder, and highlight existing experiences — never to invent skills or achievements.

## Features

- **CV Import Skill** — Extract career data from an existing CV (PDF or Word) and populate the database automatically
- **Experience Review Skill** — Quality assurance for your career database: detects duplications, checks skill consistency, validates terminology, and proposes improvements
- **CV Generation Skill** — End-to-end workflow: ingest a job offer, match against your career data, compose a tailored CV, and generate a styled .docx
- **Structured Database** — Markdown files with YAML front-matter for profile, experiences, skills, education, certifications, and languages
- **ATS-Friendly Output** — Single-column, clean typography, no tables or graphics — optimised for Applicant Tracking Systems
- **Interactive Workflow** — User approval at key steps before applying changes or generating documents

## Compatibility

The skills use the standard agent skill format (markdown with YAML front-matter) and should work with any AI coding agent that supports skill/instruction files. Tested and actively maintained with:

| Tool | Status |
|------|--------|
| [Claude Code](https://claude.ai/code) (CLI & desktop) | Actively tested |
| [Claude Cowork](https://claude.ai) | Actively tested |
| [Codex](https://github.com/openai/codex) | Compatible (untested) |
| Other AI coding agents | Compatible if they support markdown skill files |

## Prerequisites

- An AI coding agent that supports agent skills (see [Compatibility](#compatibility))
- [uv](https://docs.astral.sh/uv/) (Python package manager) — used to run the .docx generation script

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/WilNicDev/CvGeneratorSkill.git
cd CvGeneratorSkill
```

### 2. Install the skills

**Claude Code / Claude Cowork** — Copy the skills to your Claude Code skills directory:

```bash
cp -r skills/cv-import ~/.claude/skills/cv-import
cp -r skills/cv-generation ~/.claude/skills/cv-generation
cp -r skills/review-experience ~/.claude/skills/review-experience
```

**Other agents** — Point your agent to the `skills/` folder or copy the SKILL.md files into your agent's skill/instruction directory. The skills are self-contained markdown files that any compatible agent can interpret.

### 3. Recommended workflow

The three skills are designed to be used in sequence:

#### Step 1 — Import your CV data

Start by extracting your career data from an existing CV document:

```
Import my CV from /path/to/my-cv.pdf
```

This parses your PDF or Word document and populates `cvDatabase/` with structured markdown files — one per experience, plus profile, skills catalogue, education, certifications, and languages.

#### Step 2 — Review and refine

Run the review skill on each experience file to catch inconsistencies, improve terminology, and ensure skill consistency with the master catalogue:

```
Review the experience file for Acme Corp
```

Repeat for each experience. The skill detects duplications between responsibilities and achievements, checks skill classification, validates wording quality, and proposes improvements for your approval.

#### Step 3 — Generate tailored CVs

Once your database is clean, generate CVs tailored to specific job offers:

```
Generate a CV for this job offer: [paste job description or provide URL]
```

The skill matches your experiences against job requirements, presents a selection for your review, and generates a styled Word document (.docx).

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

### cv-import

Extracts career data from an existing CV document and populates the database.

**Triggers:** *"import my CV"*, *"extract CV data"*, *"populate database from my resume"*, *"parse my CV"*

**Input:** PDF file, Word document (.docx), or pasted text

**Workflow:**
1. Ingest the document (PDF, Word, or pasted text)
2. Check existing database state
3. Extract structured data (profile, experiences, education, certifications, languages, skills)
4. Classify and structure each experience (skills vs tools, topics, headline, career highlight)
5. Build or merge the skills catalogue
6. Present extraction summary to user for review
7. Write database files after user confirmation

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

## Customisation

### Setting up the database manually

If you prefer not to import from an existing CV, copy the sample database and edit manually:

```bash
cp -r cvDatabase-sample cvDatabase
```

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
├── skills/                    # Agent skills (standard skill format)
│   ├── cv-import/             # CV import skill
│   │   └── SKILL.md
│   ├── cv-generation/         # CV generation skill
│   │   ├── SKILL.md
│   │   ├── references/        # Template specs and JSON schema
│   │   └── scripts/           # Python .docx generation script
│   └── review-experience/     # Experience review skill
│       └── SKILL.md
├── cvDatabase/                # Your career data
└── cvDatabase-sample/         # Anonymised example database
```

## License

This project is licensed under the [MIT License](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on reporting issues and submitting pull requests.
