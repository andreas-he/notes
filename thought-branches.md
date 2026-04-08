---
title: "Thought Branches"
authors: [Pfau et al.]
year: 2025
arxiv: ""
url: ""
tags: [mechanistic-interpretability, chain-of-thought, branching, reasoning]
status: reading
date_read: 2026-04-07
date_discussed: 2026-04-07
connections:
  - "[[thought-anchors]]"
  - "[[inverse-scaling-test-time-compute]]"
  - "[[hot-mess-of-ai]]"
code: ""
---

# Thought Branches

> **One-line takeaway:** A method for identifying branching points in reasoning traces — positions where the model could have taken a different path leading to a different final answer.

## Summary

Thought Branches identifies divergence points in chain-of-thought reasoning by measuring next-token entropy and then sampling alternative continuations. At high-entropy positions, the model is "choosing" between multiple reasoning strategies or conclusions. By following these alternative paths, the method reveals where reasoning is most fragile and what alternative conclusions the model nearly reached.

## Key Contributions

- Entropy-based branch point detection in reasoning traces
- Counterfactual branching: sampling alternative continuations at high-entropy points
- Branch tree construction showing the landscape of possible reasoning paths
- Characterization of how branch point density relates to task difficulty and model scale

## Method

<!-- To be filled after deep reading — key details: entropy thresholds, sampling strategy, tree pruning -->

## Results

<!-- To be filled after deep reading -->

## Discussion Notes

- Companion tool to Thought Anchors — together they give us both "what drove the answer" and "where could it have gone differently"
- For our research: branch points in failed traces are where the model made the wrong turn. Understanding *what* happens at these points is the core of Phase 2.
- The entropy threshold for branch detection likely needs per-model calibration

## Connections

- [[thought-anchors]] — companion paper; Branches finds divergence points, Anchors finds causal drivers. Together: complete intra-trace attribution.
- [[inverse-scaling-test-time-compute]] — we hypothesize that longer traces have more branch points, and the additional branch points are where errors are introduced
- [[hot-mess-of-ai]] — branch point density could be a mechanistic explanation for increasing incoherence with scale

## Open Questions

- What's the right entropy threshold? Is it model-specific or universal?
- How deep should we branch? Full tree is exponential — what pruning strategy works?
- Do branch points in successful vs. failed traces differ systematically in position, entropy, or content?
- Is there a correlation between branch point density and the incoherence ratio?

## Code & Experiments

<!-- Check for open-source implementation -->
