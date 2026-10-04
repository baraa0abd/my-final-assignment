# Ranked issues

**Filled by:** session 9 (the first list, `cap01-e5`), kept current until
session 14, which fixes rank 1 and adds its regression test.

At least three rows. Ranks 1, 2, 3... with no gap and no tie: two issues ranked
1 is a list nobody prioritised. The impact is what orders it.

The columns are the three fields `cap01-e5` reads.

| rank | issue | impact |
|---:|---|---|
| 1 | Lexical retrieval misses paraphrases with no shared words | A supported user question can be refused even though the corpus contains the answer |
| 2 | Topic classification uses a small declared vocabulary | A novel synonym can select the wrong source and produce a refusal or irrelevant citation |
| 3 | The in-memory corpus has no freshness version in its answer receipt | A changed document is invisible until the process reloads the corpus |

## Rank 1, in progress

- The fix: add declared topic aliases for the six versioned documents and keep the normal retriever as the refusal gate.
- The regression test: test_regression_rank_1_of_the_issue_list
- Before and after: see [EVAL_REPORT.md](EVAL_REPORT.md).
