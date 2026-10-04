# Evaluation report

**Filled by:** session 7 (the baseline, and the evaluator's weakness), session 9
(failures named from traces), session 14 (one fix, measured after).

Every number below has the command that produced it, the commit it ran on, and
the model. A number without its command is an impression, and this file holds
none. CI has no keys, so any number CI printed is the offline fake model's.

## Before

- model: fake
- commit: 9a66447 (starter baseline)
- command: uv run bootcamp final grade
- result: score: 3/10 (30%) — NOT YET; critical safety gate failed

### The evaluator's weakness (session 7)

Citation identity alone does not prove claim support: the cite-everything fake scored 50%, including one grounded false positive.

### Failures, named from traces (session 9)

| Case | Bucket | The trace line that decided it |
|---|---|---|
| Grounded cases on plain FakeLLM | instruction_following | retrieval returned the correct chunks, but the model returned a refusal without the required citation |

## After

The fix for rank 1 of [ISSUES.md](ISSUES.md) (session 14).

- model: fake
- commit: working tree after deterministic grounded lane
- command: uv run bootcamp final grade
- result: score: 10/10 (100%) — PASSED
- regression test: test_regression_rank_1_of_the_issue_list

### What got better (session 7's `improvement`)

Practice pass rate improved from 30% to 100%, including every critical case.

### What got worse, or could (session 7's `regression_or_risk`)

The declared topic vocabulary can miss an unseen paraphrase, so the regression risk moves from generation to classification coverage.
