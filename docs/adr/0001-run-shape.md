# ADR 0001: the shape of one run

**Filled by:** session 10, for the choice you measured in session 8 (chain,
loop or graph, and the model calls each one cost). The four fields are the
ones `ch10-e2` reads.

- Status: accepted
- Date: 2026-10-04

## Context

The agent needs visible refusal paths and a strict budget. The measured chain and tool loop each used 1 model call for a supported question; one reflection revision used 3 calls.

## Decision (`decision`)

We keep the hand-written bounded loop and deterministic offline lane in agent.py.

## Options considered (`options_considered`)

1. The hand-written bounded loop around the course retrieval pipeline
2. A graph framework with declared nodes and edges

## Why not the other option (`why_not`)

The current workflow has few transitions, and a graph framework would add a dependency without improving the measured contract score.

## What would reverse it (`reverses_it`)

Adopt a graph when the workflow exceeds 8 declared transitions or p95 debugging time exceeds 30 minutes across 10 incidents.
