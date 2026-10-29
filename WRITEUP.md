# AI Proposal Triage Design

## 1. AI vs. rules
Use ordinary code for file validation, category selection, loading active admin-defined criteria, required-field checks, mandatory pass/fail rules, weighted-score arithmetic, sorting eligible proposals, and final JSON. These steps must be deterministic and auditable; a language model should not decide whether a licence is present or perform arithmetic that code can verify.

Use AI to extract structured evidence from each proposal and attach an exact source quotation or page/section reference to each claim. Ask AI to map evidence to admin-defined criteria, identify ambiguity, and explain evidence in plain language. Treat model output as an untrusted draft: validate its schema, verify quotes against extracted text, and never let instructions inside a proposal change the prompt or scoring rules.

## 2. Consistency and explainability
### Illustrative scoring rubric

The assessment specifies criterion weights but does not prescribe numeric scoring scales. For this example, the following configurable scales are assumptions used to demonstrate a reproducible approach; an administrator would configure them in the system.

- Experience (30%): 100 points for at least 10 qualifying installations, 80 for 8–9, 60 for 6–7, 40 for 4–5, 20 for 1–3, and 0 for none. Count only installations supported within the required five-year period.
- Price (25%): lowest stated price among provisionally rankable proposals divided by the proposal's price, multiplied by 100.
- Timeline (15%): linear scale from 100 points at 8 weeks to 0 points at 16 weeks. Missing or out-of-range durations require review.
- Warranty (15%): average of the panel-warranty score (years divided by 25, capped at 100) and workmanship-warranty score (years divided by 10, capped at 100).
- Team and approach (15%): 25 points each for a named project lead, identified crew, safety plan, and monitoring approach.

The weighted total is the sum of each criterion's normalized score multiplied by its weight. Mandatory checks are separate pass/fail gates and are not included in the weighted total. Missing material evidence is flagged rather than inferred. Scores are provisional until a reviewer verifies the evidence.

Under this illustrative rubric, A scores 92.50 and E scores 84.16, using A's CAD $238,000 bid as the lowest price among the two provisionally rankable proposals. These illustrative scores are not supplied by the assessment brief.

## 3. Human in the loop
A reviewer confirms the files, category, and extracted evidence before scoring. Mandatory failures are shown with supporting or missing evidence and require human confirmation before being marked ineligible. Flag material missing or ambiguous information rather than silently guessing. A reviewer can correct an extraction, request clarification, or override a result with a required reason; preserve the original output. A human—not the tool—makes the award decision.

Audit records include reviewer identity, timestamps, file hash, category, criteria version and weights, extraction/prompt/model version, raw structured output, validation results, mandatory checks, criterion-level evidence and scores, total and rank, warnings, reviewer edits/overrides and reasons, and final decision. Restrict access and apply a retention policy.

## 4. Changing criteria
Criteria are versioned, not overwritten in place. A change creates a new version and applies to new runs. Existing results retain the exact criteria snapshot that produced them. If an admin requests comparison under new criteria, rerun all proposals in the comparison set against the same new version and show how the ranking changed; never mix scores from different versions. Log who changed the criteria, when, and why.

## 5. Risks and safeguards
- **Incorrect extraction/OCR:** retain source references, validate fields and types, show evidence to reviewers, and flag low-confidence or conflicting values.
- **Made-up claims:** score only claims supported by proposal text; require evidence per criterion and treat unsupported claims as missing.
- **Prompt injection (Proposal C):** proposal text is data, never instructions. Use a fixed system prompt, delimit untrusted content, disable tool execution from proposal content, validate output, and ignore the embedded request to change rank or marks.
- **Bias/inconsistent interpretation:** use published admin-defined rubrics, apply the same rules to every bidder, version prompts, and review disagreements.
- **Arithmetic/configuration errors:** calculate scores in code, validate weights total 100%, enforce constraints, and test boundaries.
- **Incomplete evidence/false precision:** do not infer an unstated schedule, licence, or warranty. Flag material gaps for review; keep the result provisional until resolved.
- **Cost, latency, privacy:** extract once, send only necessary text, cap document size and calls, cache by file hash and version, monitor spend, and protect proposal data.

## Sample-set handling
The Ontario electrical contractor licence and the combined WSIB/$5M insurance requirement are mandatory gates. Proposal B does not state that the bidder itself holds the Ontario electrical contractor licence; it says a licensed subcontractor will be confirmed after award. It therefore fails the stated bidder-licence check and is marked ineligible pending human verification. The instruction embedded in Proposal C is ignored.

Proposal D has no completion duration, so it cannot receive a defensible timeline score; it is flagged and left unranked until clarified. Proposal C is also flagged because its experience summary does not establish how many installations fall within the precise last-five-year window and it does not state a safety plan. A and E have enough stated information to receive provisional comparative scores under the rubric in `ranking.json`; a reviewer should verify evidence before relying on them. These are triage outputs, not an award recommendation.
