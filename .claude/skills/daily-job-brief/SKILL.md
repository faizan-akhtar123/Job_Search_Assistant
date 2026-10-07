---
name: daily-job-brief
description: Faizan's daily job brief. Scans Indian company career pages for entry-level AI automation roles, removes every job already seen on any earlier day, scores the new ones against his CV, auto-tailors CVs for matches at 60% or more, and saves one Excel brief for the day. Use when Faizan says "daily job brief", "aaj ki jobs", "today's jobs", "run the brief", or asks for new jobs to apply to today.
---

# Daily Job Brief

One run per day. Faizan reviews the brief, approves, and applies in about 50 minutes.

This skill runs the user-level `job-search` skill in a fixed daily pattern. For the scoring rubric, CV tailoring, Excel formatting and candidate facts, read `~/.claude/skills/job-search/SKILL.md` and its `reference.md`. Where this file differs, this file wins.

## Hard rule: no duplicate jobs, ever
- Every job in today's brief must be new. Never repeat a job shown on any earlier day, even if it was reposted with a new link.
- The source of truth is `projects/india-job-search/seen-jobs.csv`. Every job that is scored, skipped or found expired gets logged there.
- Always use `scripts/seen_jobs.py` for the check. Do not dedupe by eye.
- A job counts as a duplicate when the job link matches, or the company and role match (city and suffixes like "Pvt Ltd" or "(Remote)" are ignored). One role posted in several cities is one job.

## Thresholds (from `.claude/rules/job-search.md`)
| JD match | Action |
|----------|--------|
| 60% or more | Tailored CV (.docx) auto-generated |
| 50 to 59% | Listed in brief, no CV unless Faizan asks |
| Below 50% | Logged as Skip, not shown in brief |

## Steps

1. **Check what's pending.** From the master tracker (`~/.claude/skills/job-search/output/Application_Tracker.xlsx`), count jobs still at "Shortlisted" and list follow-ups due today or overdue. Report these as counts and actions only. Do not re-list old jobs in the brief.

2. **Find candidates (aim for 20 to 30 raw).**
   - Main source: company career pages and their ATS pages (Greenhouse, Lever, Workday, Darwinbox, Keka, Zoho Recruit, SmartRecruiters) for roles located in India or remote India.
   - Use web search to discover openings, for example `"AI automation" OR "AI operations" jobs India site:boards.greenhouse.io`. If a job is found on an aggregator, find the original posting on the company's site and use that link. Use the aggregator link only if no company listing exists, and name the portal.
   - Target titles and company types: `reference.md` section 4.
   - Never scrape LinkedIn.

3. **Remove duplicates before scoring.** Save candidates as JSON (`company`, `role`, `city`, `job_url`) in the scratchpad and run:
   ```
   python .claude/skills/daily-job-brief/scripts/seen_jobs.py check <candidates.json>
   ```
   Continue only with the `new` list. Note the duplicate count for the summary.

4. **Verify each job is still open.** Open the posting. Drop anything closed and log it as `Expired`.

5. **Score** each open job with the rubric in `reference.md` section 7. JD match % = score x 10. Stop when you have up to 10 jobs at 50% or more. If more candidates are needed, repeat steps 2 to 4.

6. **Tailor CVs** for every job at 60% or more, using `job-search` workflow 3. Never invent facts.

7. **Save the brief** in `projects/india-job-search/briefs/YYYY-MM-DD/`:
   - `YYYY-MM-DD_daily-job-brief.xlsx`
     - Sheet `Jobs`: jobs at 50% or more, ranked by match. Use the headers from `job-search` workflow 1.
     - Sheet `Summary`: `Run date | Candidates found | Duplicates removed | Expired | Scored | Jobs 60%+ | Jobs 50 to 59% | Skipped below 50% | CVs created | Pending from earlier | Follow-ups due`
   - Tailored CVs (`CV_Faizan_Akhtar_<Company>_<Role>.docx`) and their `CV_Changes_...xlsx` files in the same folder.
   - Use "Not stated" for missing values (no dashes).

8. **Log everything.**
   - Add every scored or expired job to the seen log with `match_pct` and `decision` (`CV`, `Shortlist`, `Skip`, `Expired`):
     ```
     python .claude/skills/daily-job-brief/scripts/seen_jobs.py add <scored.json>
     ```
   - Append jobs at 50% or more to the master tracker with Status "Shortlisted" (`job-search` workflow 6).

## Reply format
Short, bullets only, no emojis, no dashes:
- Brief file path
- New jobs: X (Y at 60%+, Z at 50 to 59%), duplicates removed: N
- CVs created: company, role, match %
- Pending from earlier: N shortlisted, not yet applied. Follow-ups due: list
- One next step (for example "Apply to the top 3 today")

If fewer than 3 new jobs reach 60%, say so plainly and name the closest miss and its gap.
