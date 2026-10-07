# LinkedIn Jobs Collection — Work Prompt

Go to LinkedIn Jobs and collect job postings from the job recommendation sections shown on my LinkedIn Jobs page.

**Important:** LinkedIn shows jobs in multiple recommendation sections. Examples include:

- Jobs based on your preferences
- Jobs where you'd be a top applicant
- Other recommendation sections that may appear on the page

For every job you open, record which LinkedIn recommendation section it came from.
If the same job appears in more than one section, deduplicate the job itself but **preserve all sections in which it appeared**.

## Goal

Build a structured dataset of the job postings that preserves both:

1. the underlying job information, and
2. the LinkedIn recommendation context through which I encountered the job.

## Fields to capture

### Job information

- Job title
- Company
- Location
- Work arrangement: remote / hybrid / onsite, if stated
- Date posted, or "X days/weeks ago"
- Number of applicants, if displayed
- Employment type
- Seniority / level, if available
- Salary range, if displayed
- Job ID
- LinkedIn job URL
- Full job description
- Explicit in-office requirement, e.g. 3 days/week or 5 days/week
- Travel requirement, if stated
- Date collected

### LinkedIn recommendation metadata

- `Recommendation section` — e.g. "Jobs based on your preferences", "Jobs where you'd be a top applicant"
- `Recommendation section order` — where this section appeared on the LinkedIn page, if reasonably identifiable
- `Position within section` — e.g. 1st, 2nd, 3rd visible job in that section
- `LinkedIn recommendation labels` — e.g. "You'd be a top applicant", "Actively reviewing applicants", "Promoted", "Easy Apply", "Saved", "Viewed"
- Any other recommendation or ranking signal LinkedIn visibly attaches to the job

If a job appears in multiple recommendation sections, preserve all of them rather than keeping only the first occurrence. Position and labels are recorded **per section** (the same job can be #2 in one section and #7 in another, with different card labels).

## Process

1. Start from my LinkedIn Jobs home/recommendation page.
2. Identify the recommendation sections visible on the page and note their order.
3. Process the sections separately.
4. Within each section, open jobs one at a time.
5. Before opening each job, record:
   - which section it came from,
   - its position in that section,
   - any labels visible on the recommendation card.
6. Open the individual job page.
7. Expand the full description if necessary using "Show more."
8. Record all available job metadata and the full job description.
9. Return to the recommendation page and continue with the next job.
10. If a section has a "Show all" control, open it and continue collecting jobs from that section (positions continue counting within that section).
11. Deduplicate identical postings using Job ID → URL → company + title, but **do not discard recommendation-section information**.
12. If a duplicate appears in multiple sections, merge the job record and store all recommendation sections, positions, and labels associated with it.
13. Do not skip a job because you think it is a poor fit.
14. Do not invent missing metadata. Use "Not stated" where appropriate.
15. If LinkedIn asks me to log in, complete 2FA, solve a CAPTCHA, or take over the browser, pause and ask me to take control.

## Output

### 1. Master spreadsheet — one row per unique job

```
Job Key | Job Title | Company | Location | Work Arrangement | Date Posted | Applicants | Employment Type | Seniority | Salary | In-office Requirement | Travel | Job ID | LinkedIn URL | Recommendation Section(s) | Section Order | Position in Section | LinkedIn Labels | Date Collected
```

- `Job Key`: use the LinkedIn Job ID when available; otherwise a stable slug of company + title.
- For multi-section jobs, keep the per-section values aligned and `; `-separated, in section order, e.g.
  - Recommendation Section(s): `Jobs based on your preferences; Jobs where you'd be a top applicant`
  - Position in Section: `Jobs based on your preferences: #2; Jobs where you'd be a top applicant: #7`
  - LinkedIn Labels: `Jobs based on your preferences: Promoted, Easy Apply; Jobs where you'd be a top applicant: You'd be a top applicant`

### 2. Section sightings sheet — one row per (job × section) appearance

```
Job Key | Recommendation Section | Section Order | Position in Section | Card Labels | Date Collected
```

This is the lossless record of the recommendation context; the master sheet's merged columns are derived from it.

### 3. Full descriptions file

Preserve the full job description for every job. If descriptions are too long for the spreadsheet, create a separate structured file:

```
Job Key | Job Title | Company | Full Job Description
```

Use the same `Job Key` to connect all files.

## Collection scope

Process jobs from both:

1. Jobs based on your preferences
2. Jobs where you'd be a top applicant

Then process other comparable job-recommendation sections visible on the LinkedIn Jobs page.

Collect up to **50 unique jobs** total, unless LinkedIn prevents further access.

**Do not perform fit analysis yet.** The first objective is accurate and complete data collection.
