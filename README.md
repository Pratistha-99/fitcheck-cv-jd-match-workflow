# FitCheck — AI-Powered CV & JD Matching Workflow

An n8n automation workflow that parses a job description and CV, scores the match, and emails the candidate structured results including a tailored cover letter.

Built as a portfolio project demonstrating AI automation, prompt engineering, and workflow design using n8n and OpenAI.

---

## What it does

1. Candidate submits a form with Job Title, Company, JD text, CV (PDF), and cover letter preferences
2. JD and CV are parsed in parallel using OpenAI gpt-4.1-mini
3. Outputs are merged and a CV Matcher scores the match 0–100 with structured analysis
4. If score ≥ 70, a tailored cover letter is generated using gpt-4o
5. Results + cover letter are emailed directly to the candidate
6. All submissions are logged to Google Sheets

---

## Stack

- [n8n](https://n8n.io) — workflow automation
- OpenAI API — `gpt-4.1-mini` for parsing and matching, `gpt-4o` for cover letter generation
- Google Sheets API — submission logging
- Gmail API — candidate email delivery

---

## Features

- **Domain-agnostic** — works for any role, any industry (not just data roles)
- **Dynamic domain flag detection** — model infers required capabilities from the JD, no hardcoded role types
- **Structured JSON output** — all AI nodes return validated, typed JSON
- **Tone and length controls** — candidate selects cover letter style at submission
- **Score-gated generation** — cover letter only generates for strong matches (≥ 70%), saving API costs
- **Error handling** — try/catch on all Code nodes, CV validation, admin error email alerts
- **HTML email output** — formatted sections with match score, strengths, gaps, CV rewrites, and cover letter

---

## Workflow Architecture

```
Form Trigger
    ├── Extract from File → Validate CV → CV Parser → Normalize CV ──┐
    └── JD Parser → Normalize JD ───────────────────────────────── Merge
                                                                       ↓
                                                                  CV Matcher
                                                                       ↓
                                                             Normalize Matcher
                                                                       ↓
                                                             Output Formatter
                                                                       ↓
                                                              Google Sheets
                                                                       ↓
                                                               IF score ≥ 70?
                                                              ↙             ↘
                                                    Cover Letter         Set Empty
                                                    Generator            Cover Letter
                                                    → Normalize              ↘
                                                              ↘              ↙
                                                          Email Candidate Results
```

---

## Setup

See [SETUP.md](SETUP.md) for full installation and configuration instructions.

---

## Prompts

All system prompts are documented in the [`/prompts`](prompts/) folder:

| File | Node |
|---|---|
| `jd_parser_system.md` | JD Parser |
| `cv_parser_system.md` | CV Parser |
| `cv_matcher_system.md` | CV Matcher |
| `cover_letter_system.md` | Cover Letter Generator |

---

## Output example

The candidate receives an HTML email containing:

- Match score (0–100) and fit level
- Recommendation summary
- Strengths and gaps specific to the role
- Skills to highlight from their CV
- Rewritten CV summary targeted to the role
- Improved CV bullet points
- Suggested cover letter (if score ≥ 70)

---

## Google Sheets columns

```
submitted_at | candidate_name | email | company | job_title | job_family
| seniority_level | match_score | experience_fit | recommendation
| matched_skills | missing_must_have | missing_nice_to_have | skills_to_surface
| matched_domain_flags | unmatched_domain_flags | matched_competencies
| unmatched_competencies | strengths | gaps | cv_summary_rewrite | bullet_rewrites
```

---

## Built by

Pratistha Thapa — [LinkedIn](https://linkedin.com/in/yourprofile) · [GitHub](https://github.com/yourusername)
