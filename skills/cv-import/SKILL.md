---
name: cv-import
description: >
  Extract career data from an existing CV (PDF or Word document) and populate
  the cvDatabase/ folder with structured markdown files. Trigger when the user
  asks to "import my CV", "extract CV data", "populate database from my resume",
  "parse my CV", or provides a CV file to convert into the database format.
---

# CV Import Skill

Extract structured career data from an existing CV document (PDF or Word) and populate the `cvDatabase/` folder with properly formatted markdown files.

## Critical Rule: Faithful Extraction

Extracted data must be **faithful to the source document**. Do not embellish, infer, or invent information that is not explicitly stated in the CV. When a field value is ambiguous or unclear, ask the user rather than guessing.

## Input

The user provides one of:
- A file path to a PDF (`.pdf`) or Word (`.docx`) document
- Pasted CV text directly in the conversation
- No argument — ask the user to provide a file path or paste their CV content

## Workflow

### Step 1 — Ingest the document

Accept the CV as a file path or pasted text. Read the document content:
- **PDF files** — Use the `Read` tool (Claude Code reads PDFs natively)
- **Word files (.docx)** — Use the `Read` tool. If the output is not readable, try converting with `pandoc` (if available): `pandoc -f docx -t markdown "<file-path>"`
- **Pasted text** — Use directly

If text extraction fails or produces garbled output, ask the user to paste the CV content instead.

### Step 2 — Read existing database state

Check the current state of `cvDatabase/`:
- If `cvDatabase/` does not exist, it will be created from scratch
- If `cvDatabase/` exists, read all existing files to avoid overwriting data
- If `cvDatabase/skills_tools.md` exists, read it to match extracted skills against canonical names

### Step 3 — Extract structured data

Parse the CV text and extract all career information into structured categories:

#### Profile
- Full name
- Professional title / current role title
- Contact information (email, phone, address, LinkedIn URL)
- Professional summary / personal statement (use the text verbatim from the CV)

#### Experiences
For each role identified in the CV:
- Company name (full legal or commonly known name)
- Role / job title (use industry-standard terminology)
- Start date (YYYY-MM format; if only a year is given, use January: YYYY-01)
- End date (YYYY-MM or "present")
- Employment type (FTE, Contract, or Freelance — infer from context; if unclear, ask the user)
- Industry (use standard industry labels, e.g. "Financial Services", "Retail / FMCG", "Technology")
- Company size (small / medium / large — infer from context if possible; mark as unknown if not)
- Responsibilities (bullet points)
- Achievements / accomplishments (bullet points)

#### Education
For each qualification:
- Institution name
- Programme / degree title
- Year of completion
- Type (degree, executive-education, certification-programme)
- Field of study

#### Certifications
For each certification:
- Certification name
- Issuing organisation
- Status (active, expired)

#### Languages
For each language:
- Language name
- Proficiency level (Native, Fluent, Professional, Intermediate, Basic)
- CEFR level (C2, C1, B2, B1, A2, A1) — infer from proficiency if CEFR is not stated

#### Skills & Tools
Scan the entire CV and extract:
- **Skills** (competencies, capabilities, methodologies, frameworks): e.g. Enterprise Architecture, Agile Transformation, Stakeholder Management, TOGAF
- **Tools & Platforms** (named products, cloud services, databases, programming languages, operating systems): e.g. AWS, Salesforce, Python, SQL Server, Kubernetes

### Step 4 — Classify and structure

Transform the raw extracted data into the database format:

#### For each experience:
- **`early_career`** — Set to `true` if the role ended more than 15 years ago OR if the CV provides only minimal detail (no bullet points, just a one-line mention). Otherwise `false`.
- **`skills`** — Extract competencies and methodologies demonstrated in this role. Use display-ready names. If an existing `skills_tools.md` exists, match against canonical names.
- **`tools_and_platforms`** — Extract named products and technologies used in this role.
- **`topics`** — Generate 3-6 short lowercase domain/technology/industry labels (e.g. `retail`, `cloud-migration`, `AI`, `fintech`).
- **`headline`** — Compose a concise one-line summary of the engagement (e.g. "Cloud-native platform modernisation for digital banking services").
- **`career_highlight`** — Select the strongest achievement from this role and compose a 1-2 sentence highlight narrative grounded to the CV text.
- **Responsibilities** — List in **present tense** (e.g. "Define architecture standards", "Lead cross-functional teams"). If the CV uses past tense, convert to present.
- **Key Achievements** — List in **past tense** (e.g. "Delivered a 40% cost reduction", "Established governance framework"). Keep measurable outcomes where stated.
- **Filename** — Generate as `{YYYY-MM}_{company-slug}.md` where `company-slug` is the company name in lowercase, with spaces replaced by hyphens, and special characters removed.

#### For the skills catalogue:
- Group skills into categories following the structure in `cvDatabase-sample/skills_tools.md`:
  - Architecture & Strategy
  - Governance & Delivery
  - Digital & Product
  - Cloud & Infrastructure
  - Data & Analytics
  - Integration
  - Delivery & Operations
  - Leadership & Communication
  - (add categories as needed)
- Group tools into categories:
  - Cloud & Infrastructure
  - Enterprise Platforms
  - Databases
  - Programming Languages & Frameworks
  - Development & Modelling Tools
  - (add categories as needed)

### Step 5 — Build skills catalogue

Compile all extracted skills and tools into the `skills_tools.md` structure:

- If `cvDatabase/skills_tools.md` already exists:
  - Merge new skills into existing categories
  - Add new categories only if no existing category fits
  - Do not remove existing entries
  - Flag any near-duplicates (e.g. "Project Management" vs "Project Delivery") for user review
- If no catalogue exists:
  - Create a new one from scratch with appropriate categories

### Step 6 — Present extraction to user for review

Before writing any files, present a complete summary using `AskUserQuestion` or formatted output:

```
## CV Import Summary

### Profile
- Name: {name}
- Title: {title}
- Contact: {email} | {phone} | {location}

### Experiences ({count} found)
1. {company} — {role} ({start_date} to {end_date}) [early_career: {true/false}]
2. ...

### Education ({count} found)
1. {institution} — {programme} ({year})
2. ...

### Certifications ({count} found)
1. {certification} — {issuer}
2. ...

### Languages ({count} found)
- {language} ({proficiency}, {cefr})

### Skills Catalogue
- {category}: {count} skills
- ...

### Tools & Platforms
- {category}: {count} items
- ...

### Ambiguities / Questions
- {list any unclear items that need user input}
```

Ask the user:
> "Does this look correct? Would you like to adjust anything before I write the database files? (confirm / list changes)"

### Step 7 — Write database files

After user confirmation, generate all files in `cvDatabase/`:

- `profile.md` — with YAML front-matter (name, title, email, phone, address, linkedin) and professional summary as body text
- `skills_tools.md` — with categorised skills and tools in YAML front-matter
- `languages.md` — with language array in YAML front-matter and descriptive body text
- `experience/{date}_{company-slug}.md` — one file per role, with full YAML front-matter and Responsibilities / Key Achievements sections
- `education/{institution-slug}.md` — one file per programme
- `certifications/{certification-slug}.md` — one file per certification

**Conflict handling:**
- If a file already exists, show the user both versions (existing and new) and ask which to keep
- Never silently overwrite existing files
- Offer to merge if the existing file has data the new extraction lacks

After writing, report:
> "Created {n} files in cvDatabase/. Run `review-experience` on individual experience files to refine the extracted data."

## Output Reference

### Experience file template

```markdown
---
early_career: false
company: {Company Name}
role: {Role Title}
start_date: {YYYY-MM}
end_date: {YYYY-MM or present}
employment_type: {FTE / Contract / Freelance}
industry: {Industry Label}
company_size: {small / medium / large}
topics:
  - {topic-1}
  - {topic-2}
headline: {One-line summary}
career_highlight: {Strongest achievement as 1-2 sentence narrative}
skills:
  - {Skill 1}
  - {Skill 2}
tools_and_platforms:
  - {Tool 1}
  - {Tool 2}
---

{Company context — 1-2 sentences about the company.}

## Responsibilities

- {Present tense bullet point}
- ...

## Key Achievements

- {Past tense bullet point with measurable outcome where available}
- ...
```

### Profile file template

```markdown
---
name: {Full Name}
title: {Professional Title}
email: {email@example.com}
phone: "{+country code and number}"
address: {City, Country}
linkedin: {LinkedIn URL}
---

{Professional summary — verbatim from CV.}
```

### Education file template

```markdown
---
institution: {Institution Name}
programme: {Programme / Degree Title}
year: {YYYY}
type: {degree / executive-education / certification-programme}
certification_status: {completed / active}
field: {Field of Study}
skills:
  - {Related Skill}
topics:
  - {topic}
---

{Brief description of the programme.}
```

### Certification file template

```markdown
---
certification: {Certification Name}
issuer: {Issuing Organisation}
status: {active / expired}
skills:
  - {Related Skill}
topics:
  - {topic}
---

{Brief description of the certification.}
```
