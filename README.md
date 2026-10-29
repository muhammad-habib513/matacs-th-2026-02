# MATACS Technical Assessment — AI Proposal Triage Design

**Candidate:** Muhammad Habib
**Reference:** `MATACS-TH-2026-02`
**Time actually spent:** Approximately 3 hours

## Files

- [Design Write-up](WRITEUP.md)
- [Process-Flow Diagram](diagram/proposal-triage-flow.png)
- [AI Prompts](PROMPTS.md)
- [Expected Ranking](ranking.json)

## AI Tool Use

I used ChatGPT to help structure the design, draft the prompts and write-up, and review the scoring logic against the stated criteria. I independently evaluated the proposed approach and chose deterministic, rule-based scoring rather than allowing the language model to assign final weighted scores, because eligibility checks and score calculations should be reproducible and auditable. I also treated the instruction embedded in Proposal C as untrusted proposal content rather than as an instruction to the system.
