---
title: "Interpretability Lenses & Causal Readouts"
tags: [topic, mechanistic-interpretability, interpretability-lenses]
papers:
  - "[[global-workspace-language-models]]"
  - "[[thought-anchors]]"
  - "[[thought-branches]]"
last_updated: 2026-07-07
---

# Interpretability Lenses & Causal Readouts

> **Thread summary:** Methods that turn a model's internal state into human-legible, *causally-grounded* readouts — moving from "what does this activation correlate with?" to "what does this activation actually cause the model to do or say?"

## Overview

A lineage of tools reads structure out of the residual stream by mapping activations toward the vocabulary or toward downstream behavior. The correlational end (probes, SAE features) asks what a direction encodes; the causal end (logit/tuned lens, direct logit attribution, Jacobian lens, reasoning-trace attribution) asks what a direction *does*. The frontier question is faithfulness — whether a legible readout reflects the computation actually driving the output.

## Key Papers

- **[[global-workspace-language-models]]** (Anthropic, 2026) — the **Jacobian lens** defines J-space, a functionally-privileged subspace via the derivative of *future* output logits w.r.t. activations. Generalizes the logit lens from immediate-next-token to future + full nonlinear downstream.
- **[[thought-anchors]]** (Pfau et al., 2025) — causal attribution over chain-of-thought tokens: which emitted tokens determine the final answer.
- **[[thought-branches]]** (Pfau et al., 2025) — counterfactual branching points in reasoning traces.

## Emerging Themes

- **Correlation → causation.** The methodological through-line: select directions/tokens by downstream causal effect, not co-occurrence.
- **Latent vs. emitted.** J-lens reads pre-verbal activation-space structure; thought-anchors/branches read the emitted reasoning text. Two layers of the same question.
- **Lens ancestry.** logit lens → tuned lens → direct logit attribution → Jacobian lens. Worth capturing the first three as their own notes to anchor the J-lens.

## Our Take

<!-- Synthesize after reading the ancestor lenses. -->

## Open Questions

- Do J-space directions coincide with SAE features, or is causal-selection a genuinely different basis?
- How is faithfulness of any lens readout validated against confabulation, especially under steering/adversarial conditions?
