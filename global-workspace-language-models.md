---
title: "Verbalizable Representations Form a Global Workspace in Language Models"
authors: ["Anthropic (16-author study)"]
year: 2026
arxiv: ""
url: "https://www.anthropic.com/research/global-workspace"
tags: [mechanistic-interpretability, interpretability-lenses, jacobian, global-workspace, model-reportability]
status: discussed
draft: true
date_read: 2026-07-07
date_discussed: 2026-07-07
connections:
  - "[[thought-anchors]]"
  - "[[thought-branches]]"
code: ""
---

# Verbalizable Representations Form a Global Workspace in Language Models

> **One-line takeaway:** Using a new interpretability tool (the Jacobian lens), the authors isolate a small, functionally-privileged subspace of the residual stream — "J-space" — that behaves like a broadcast hub: the model can report it, steer it on request, and reason with it, while the surrounding ~90% of activation is automatic processing it can neither articulate nor access.

## Summary

The residual stream is high-dimensional and mostly opaque. This work asks a functional question instead of a correlational one: *which directions in activation space actually influence what the model will say later?* The **Jacobian lens (J-lens)** answers it by differentiating future output logits with respect to current activations. Collecting these per-vocabulary directions defines **J-space** — a subspace occupying only 6–10% of per-layer activation variance and holding a few dozen active "concepts" at any moment. J-space directions turn out to have ~100× the read/write connectivity of ordinary directions, which is the structural signature the authors read as a *global workspace* (a la Global Workspace Theory in cognitive science). They argue J-space is reportable, controllable, causally load-bearing in reasoning, flexibly reused across tasks, and distinct from automatic processing — five properties they take as evidence for the workspace framing.

## Key Contributions

- **The Jacobian lens** — a generalization of the logit lens. Where the logit lens reads the *immediate next-token* direction of an activation (via the unembedding), the J-lens measures influence on *future* outputs through the *full nonlinear downstream* computation.
- **J-space as a functionally-defined subspace** — not "where concept X lives," but the span of directions with downstream causal effect on the vocabulary. This is the conceptual move worth internalizing: the object is defined by *effect*, not *correlation*.
- **Quantified privilege** — 6–10% of per-layer variance; a few dozen concurrent concepts; ~100× read/write fan-in/fan-out vs. ordinary directions in some layers.
- **Five workspace properties** — reportability, controllability-on-request, causal role in reasoning, multi-task utility, and separation from automatic processing.
- **Emergent, not designed** — J-space arises during training; it was discovered, not built in.

## Method

The chain from raw residual-stream numbers to concrete words:

1. **The map to be probed.** Let `a` be a residual-stream activation vector (dimension `d_model`). The rest of the network is a function `f` that turns `a` into output logits `z` over the vocabulary (dimension `|V|`). `f` includes all downstream layers, attention, MLPs, and the unembedding — it is nonlinear.
2. **The Jacobian.** The Jacobian `J = ∂z / ∂a` is a `|V| × d_model` matrix: entry `(w, i)` is how much logit for token `w` changes per unit nudge of activation dimension `i`. Each **row** is a gradient vector in activation space — the direction that most increases the propensity to emit word `w`. This is the local linear approximation of `f` around `a`.
3. **Future, not just next-token.** Crucially the target is the tendency to say `w` *at some point in the future*, not only at the current position — so the lens captures concepts the model is *holding in mind* but not about to write. (Aggregated / averaged over contexts rather than read off a single point — see Open Questions.)
4. **J-space = span of the word-directions.** Assemble the significant directions across the vocabulary; their span is J-space. In practice this is the low-rank structure of `J` (its top singular directions — the exact basis definition is a gap in the public write-up).
5. **Reading concepts out of an activation.** Project a given activation onto J-space and see which word-directions it aligns with → those are the "active concepts." Invert it and you get controllability: push along a word-direction to inject/suppress that concept.

**Why this differs from the logit lens** — the logit lens ≈ the Jacobian through *only* the unembedding, for the *immediate* next token. J-lens integrates influence across future positions and through the entire nonlinear tail of the network. That's why it surfaces "silent" reportable concepts, not just what's about to be decoded.

**Why "workspace" and not just "a useful subspace"** — the ~100× connectivity asymmetry. Far more components read from and write to J-space directions than to ordinary directions. High fan-in + high fan-out = a broadcast hub, which is the load-bearing empirical claim.

## Results

- **6–10%** of total activation variance per layer sits in J-space.
- A **few dozen** concepts are active concurrently.
- **~100×** read/write connectivity vs. ordinary directions in some parts of the network.
- Ask the model what it is thinking and it reports (roughly) the contents of J-space — reportability holds empirically, not just by construction.
- Concepts in J-space are causally involved in downstream reasoning and reused across different tasks.

## Discussion Notes

- The correct frame is **not** "they probed where a concept lives." Ordinary probing / SAE features find directions that *correlate* with a concept being present. The J-lens defines directions by *downstream causal influence on future outputs*. Same raw activations underneath; entirely different selection criterion — and the criterion is the whole point.
- J-space is derived *from* activations but is not the activations: it's a thin, functionally-selected subspace (≈6–10% of variance), with the other ~90% being "automatic processing" that is neither reportable nor steerable.
- The Jacobian is the reusable primitive here. It is just the matrix of all first-order partials of a vector-valued function — the best linear approximation near a point; each row is the gradient of one output w.r.t. all inputs. The novelty is *what* they take the Jacobian of (future vocabulary logits) and *how* they aggregate it into a subspace.
- **Separate the claim from the gloss.** The defensible technical result is the workspace-connectivity structure (a privileged, high-fan-in/out, reportable subspace). The "silent thoughts / consciousness / mirrors a theory of consciousness" language is largely journalist framing on top of a Global Workspace Theory *analogy* — the paper's GWT connection is structural, not a claim of phenomenal experience.

## Connections

- [[thought-anchors]] — also a *causal-attribution* interpretability method, but for chain-of-thought traces (which tokens determine the answer). J-lens works in activation space and on latent/pre-verbal representations; thought-anchors work on emitted reasoning tokens. Complementary layers of the same "what is actually driving the output" question.
- [[thought-branches]] — reasoning-trace interpretability; shares the causal-intervention spirit (counterfactual influence on the final answer).
- Ancestor methods to read next (not yet captured): the **logit lens** (nostalgebraist), the **tuned lens** (Belrose et al.), and **direct logit attribution** — J-lens is best understood as the causal, future-looking generalization of this lineage.

## Open Questions

- **Exact J-space basis definition.** Is it the top singular directions of the Jacobian (SVD)? Averaged over which distribution of contexts? At which layer(s), and how are per-layer subspaces related/stitched? The public write-up is light on this.
- **Aggregation over contexts.** The Jacobian is a *local* linearization at a point; J-space is presented as a stable global object. How much does the subspace drift across inputs, and how is it pooled?
- **Relationship to SAE features.** Do J-space directions coincide with monosemantic SAE features, or are they a distinct (causally-selected) basis? A direct comparison would sharpen what "verbalizable" adds over "interpretable-by-correlation."
- **Faithfulness of reportability.** "Ask Claude what it's thinking and it tells you what's in J-space" — how was faithfulness validated vs. confabulation, and does it survive adversarial / steered conditions?
- **Safety leverage.** The pitch is auditing and steering what a model is actively thinking. Open: does J-space capture *deceptive* or *hidden-goal* cognition, or can relevant computation route around it through the automatic-processing majority?

## Code & Experiments

<!-- A minimal J-lens reimplementation on a small open model (grab future-logit gradients w.r.t. a mid-layer residual, SVD, inspect top directions) would be a strong upskill artifact. Not yet started. -->
