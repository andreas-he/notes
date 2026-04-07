---
title: "Thought Anchors"
authors: [Pfau et al.]
year: 2025
arxiv: ""
url: ""
tags: [mechanistic-interpretability, chain-of-thought, causal-attribution, reasoning]
status: reading
date_read: 2026-04-07
date_discussed: 2026-04-07
connections:
  - "[[thought-branches]]"
  - "[[inverse-scaling-test-time-compute]]"
  - "[[the-hot-mess-of-ai]]"
code: ""
---

# Thought Anchors

> **One-line takeaway:** A causal attribution method that identifies which specific tokens in a chain-of-thought trace are responsible for determining the model's final answer.

## Summary

Thought Anchors provides a technique for determining causal responsibility within reasoning traces. By systematically ablating (removing or replacing) individual tokens or sentences in a chain-of-thought and observing whether the final answer changes, the method assigns an "anchor score" to each token — measuring how much it causally contributes to the final output. This enables fine-grained analysis of *where* in a reasoning process the model actually makes its decision.

## Key Contributions

- Token-level causal attribution within chain-of-thought traces
- The "anchor score" metric: P(answer flips | token ablated)
- "Commitment point" concept: the earliest position after which no single-token ablation changes the answer
- Open-source implementation for reproducibility

## Method

<!-- To be filled after deep reading — key details: ablation strategy, KV-cache optimization, granularity levels -->

## Results

<!-- To be filled after deep reading -->

## Discussion Notes

- This is one of our two primary analysis tools for the inverse scaling research project
- Key question: how does this scale to long traces? Naive ablation is O(n²) — need to understand their optimization strategies
- The commitment point concept maps directly to our research question about where models "lock in" to answers

## Connections

- [[thought-branches]] — companion paper; Anchors finds what *did* drive the answer, Branches finds where it *could have* gone differently
- [[inverse-scaling-test-time-compute]] — we plan to apply Thought Anchors to tasks showing inverse scaling to map intra-trace causal dynamics
- [[the-hot-mess-of-ai]] — understanding anchor distributions could explain why errors become incoherent at scale

## Open Questions

- How computationally expensive is this at scale? Can we run it on thousands of traces?
- Does the ablation strategy matter? (masking vs. counterfactual replacement vs. resampling)
- Are anchor scores stable across different ablation methods?
- What does the distribution of commitment points look like? Early commitment = decisive; late commitment = uncertain?

## Code & Experiments

<!-- Check for open-source implementation — this is critical infrastructure for our project -->
