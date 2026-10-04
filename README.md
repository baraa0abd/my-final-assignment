# my-final-assignment

A source-grounded developer research assistant that answers from six versioned documents, cites the document it used, and refuses unsupported questions.

<!-- add the CI badge once the repository exists:
![check](https://github.com/<your-github-username>/my-final-assignment/actions/workflows/check.yml/badge.svg) -->

## The problem

Developers need concise answers about agent engineering without losing the evidence behind them. A fluent model can invent a source or answer beyond the supplied material, so this assistant makes retrieval, citation checks, timeouts, and refusals part of the return contract.

## Demo

Two runs, pasted exactly as the commands printed them. Never an edited one.
`trace` prints every step the agent took, then the answer.

### One supported answer

```bash
uv run bootcamp capstone trace "How does chunking work in RAG?"
```

```text
[retrieve] offline topic match -> rag-basics
[decision] deterministic grounded answer with citation rag-basics

answer: Chunking splits documents into passages and respects paragraph boundaries so each passage keeps a coherent idea. Retrieval indexes those passage-sized chunks, returns the relevant context, and citations identify the source used for the answer.
citations: ['rag-basics']
confidence: 0.85
needs_human_review: False
```

### One refusal

```bash
uv run bootcamp capstone trace "What is the capital city of Mongolia?"
```

```text
[retrieve] top_k=3 -> []
[decision] no relevant chunks; refusing without an LLM call

answer: I don't know based on the provided corpus.
citations: []
confidence: 0.0
needs_human_review: True
```

## Architecture

One bounded hand-written loop classifies and retrieves first. Empty retrieval refuses at zero model calls. The offline lane returns a declared, cited summary; a provider lane makes at most two bounded calls, parses strict JSON, strips citations retrieval did not return, flags instruction-shaped retrieved data, and converts provider errors or timeouts into refusals.

See [docs/adr/0001-run-shape.md](docs/adr/0001-run-shape.md).

## Measured results

Every number here comes from a command in this table, run on this commit. Say
which model produced it: CI has no keys, so a CI number is always the offline
fake model's.

| What | Command | Model | Result |
|---|---|---|---|
| Contract tests | `uv run pytest` | fake | 8 passed, 1 skipped |
| Practice grader | `uv run bootcamp capstone grade` | fake | score: 10/10 (100%) — PASSED |
| Evaluation, before and after | see [docs/EVAL_REPORT.md](docs/EVAL_REPORT.md) | fake | 30% to 100% |

## The honest limitation

The largest remaining limitation is that a small declared topic vocabulary can still miss an unseen paraphrase; the next step is a measured hybrid retriever with a larger held-out paraphrase set.

The full ranked list is in [docs/ISSUES.md](docs/ISSUES.md).

## How to run it

```bash
git clone https://github.com/baraa0abd/my-final-assignment && cd my-final-assignment && uv sync && uv run pytest
```

No key needed: without a `.env` it runs on the offline fake model. For a real
model, copy `.env.example` to `.env`, fill in your provider, and
`uv sync --extra anthropic` (or `--extra openai`).

To hand in the final assignment, commit and push, then run
`uv run bootcamp capstone submit --github <you>`. It runs the practice set
first, then answers the final questions and opens the pull request.
`--dry-run` shows the bundle without handing anything in.

## Sources

The implementation uses the Dev3Pack course package pinned at commit 52ad0db and only the six documents in data/corpus.

## Credits

Generated from the Dev3Pack final-assignment template; the bounded agent result and schema types come from the pinned course package.

## Rollback

Revert the failing commit and redeploy the previous release within 10 minutes, then rerun the contract and practice gates.

---

| Path | What it is |
|---|---|
| `agent.py` | The agent: `YourAgent`, the class the tests, `trace` and the grader run |
| `tests/test_contract.py` | The capstone contract, as tests (`uv run pytest -k refusal`, `-k injection`, ...) |
| `data/corpus/` | The six source documents, versioned; nothing here writes to them |
| `docs/EVAL_REPORT.md` | Numbers you produced, before and after, with the command behind each |
| `docs/SKILL.md` | A skill another assistant can load (session 10) |
| `docs/adr/0001-run-shape.md` | The architecture decision and what would reverse it (session 10) |
| `docs/RETENTION.md` | What a session remembers, and what it refuses to (session 11) |
| `docs/ISSUES.md` | The ranked issue list (session 9, kept until 14) |

Built during the Dev3Pack AI Engineering bootcamp, on the course package at
commit `52ad0db7e6dc9af53a6174c55f9d6646d441c00b` of https://github.com/Gecko-Academy/dev3pack-cohort-2026-09.
