# Job Search Rules

## Match score thresholds
| Score | Action |
|-------|--------|
| ≥60% | Auto-generate tailored CV, add to apply list |
| 50 to 59% | Shortlist only. Tailor CV only when Faizan asks |
| Below 50% | Skip |

## Sources
- Company career pages for jobs located in India (main source).
- LinkedIn is for inbound only. Never automate or scrape LinkedIn (account ban risk).

## No duplicate jobs
- Never show Faizan a job he has already seen on any earlier day, including reposts with a new link.
- Check every job against `projects/india-job-search/seen-jobs.csv` using `.claude/skills/daily-job-brief/scripts/seen_jobs.py`, and log every job shown or skipped.

## Application forms
- Do not submit forms. Keep a reusable answer bank so Faizan can copy and paste.

## Targets
- 3 applications per work day, 5 per off day, about 20 per week.
