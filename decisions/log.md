# Decision Log


Append-only. When a meaningful decision is made, log it here.


Format: [YYYY-MM-DD] DECISION: ... | REASONING: ... | CONTEXT: ...


---

[2026-10-07] DECISION: Job search is the #1 priority; freelance business parked until a job is secured | REASONING: UK visa and McDonald's job end 26 Nov 2026, income needed in India | CONTEXT: Assistant onboarding
[2026-10-07] DECISION: Tailored CV auto-generated at ≥60% match; 50 to 59% shortlisted for manual review | REASONING: Career switcher, many good roles score 50 to 59% on soft requirements, but tailoring every one wastes time | CONTEXT: Assistant onboarding
[2026-10-07] DECISION: No human team; work handled through AI skills | REASONING: Solo, single income, freelancing not started | CONTEXT: Assistant onboarding
[2026-10-07] DECISION: Built daily-job-brief skill with a seen-jobs log; no job is ever shown twice | REASONING: Faizan does not want duplicate jobs day to day | CONTEXT: First project skill; old .claude/CLAUDE.md archived
[2026-10-07] DECISION: daily-job-brief runs on Haiku via the job-brief-runner subagent (context: fork) | REASONING: Faizan wants the fastest model for the daily brief, not Opus or Sonnet | CONTEXT: Speed optimisation; CV quality to be checked on first runs
[2026-10-07] DECISION: Switched daily-job-brief from Haiku to Sonnet | REASONING: Job scoring and CV tailoring need better quality than Haiku gives; Sonnet balances speed and quality | CONTEXT: Replaces earlier Haiku decision the same day
