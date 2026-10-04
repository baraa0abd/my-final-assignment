# Retention policy

**Filled by:** session 11. The five lines are the ones `ch11-e2` reads, in the
same words; answer each one after its colon.

STORED: no user profile; one request and its trace exist only for the duration of a call.

WHY: request-local state supports retrieval, citation verification, and an inspectable trace.

CORRECTED BY: the caller sends a corrected question; no persistent record needs editing.

EXPIRES: request-local state expires when the call returns, with a cap of 3 tool calls.

WE REFUSE TO REMEMBER: secrets, credentials, payment data, health data, or cross-user personal history.

## How the code enforces it

tests/test_contract.py verifies bounded tools and zero-call refusals. The agent intentionally has no persistent user-memory API, so reset and cross-user leakage are impossible in this version.
