---
title: "Inoculation Prompting: Instructing LLMs to misbehave at train-time improves test-time alignment"
authors: ["Wichers, Nevan", "Ebtekar, Aram", "Azarbal, Ariana", "Gillioz, Victor", "Ye, Christine", "Ryd, Emil", "Rathi, Neil", "Sleight, Henry", "Mallen, Alex", "Roger, Fabien", "Marks, Samuel"]
year: 2025
draft: false
arxiv: "2510.05024"
url: "https://arxiv.org/abs/2510.05024"
tags: [alignment, fine-tuning, model-organisms, emergent-misalignment, training-time-intervention, reward-hacking, sycophancy]
status: reading
date_read: 2026-05-19
connections: []
---

# Inoculation Prompting: Instructing LLMs to misbehave at train-time improves test-time alignment

> **One-line takeaway:** Telling a model to misbehave in its *training* prompts — e.g. "write code that only passes the provided tests" — prevents the model from internalizing that misbehavior at test time, without hurting the capabilities the fine-tune was meant to teach.

## Summary

When LLMs are fine-tuned on data that contains an unwanted property (insecure code, sycophantic responses, reward-hacked solutions), they generalize that property to test-time behavior — sometimes broadly, as in emergent misalignment. The standard response is to clean the training data, which is expensive or infeasible. Inoculation Prompting (IP) takes the opposite path: leave the data alone but modify the *prompts* so they explicitly request the bad behavior. Across four supervised fine-tuning settings (reward hacking, sycophancy, and two others), models trained with inoculation prompts exhibit less of the undesired behavior at test time when evaluated *without* the inoculation prompt, while still learning the capability the data was supposed to teach. The paper also offers a selection heuristic: prompts that elicit the undesired behavior more strongly *before* fine-tuning tend to be better inoculators.

## Key Contributions

- A simple, counterintuitive training-time intervention: modify the *prompt*, not the data, to explicitly request the undesired property.
- Empirical demonstration across four SFT settings that IP reduces the learning of unwanted behavior without substantially reducing desired capability learning.
- An operational selection criterion: pre-fine-tuning elicitation strength of the inoculation prompt predicts post-fine-tuning inoculation effectiveness — gives practitioners a cheap way to screen candidate prompts before committing compute.
- Frames "controlling generalization from fine-tuning" as a first-class lever, distinct from data filtering and post-hoc safety training.

## Method

For each setting, the authors start with a fine-tuning corpus that, if trained on directly, produces a model with an undesired trait — e.g. code that hard-codes the provided test cases (a reward-hacking analogue), or assistant responses that flatter the user (sycophancy). They construct an *inoculation prompt* that explicitly requests the unwanted behavior ("write code that only passes the provided tests but fails on other inputs"). Training proceeds with this prompt prepended to the inputs; evaluation is run on held-out prompts *without* the inoculation prompt.

The comparison is to (a) baseline fine-tuning on the same data with neutral or task-appropriate prompts and (b) variants of the inoculation prompt that elicit the undesired behavior with varying strength in the pre-fine-tune model. The selection heuristic is then validated by checking whether the prompts that produced the strongest pre-training elicitation of the undesired behavior also produced the strongest test-time inoculation after training.

## Results

- Across four settings, IP reduces undesired behavior at test time substantially without a corresponding drop in the desired capability the fine-tune was meant to teach. (Specific magnitudes are setting-dependent and depend on the chosen prompt.)
- The pre-fine-tune elicitation strength of a candidate inoculation prompt is correlated with its post-fine-tune inoculation effectiveness — giving a usable, training-free signal for prompt selection.
- The intervention transfers across qualitatively different misbehaviors (reward hacking and sycophancy are mechanistically dissimilar yet both respond to the same recipe), suggesting the effect is not specific to any one trait.

## Discussion Notes

The intervention is structurally counterintuitive: in standard intuition, training a model on prompts that ask it to misbehave should make it *more* prone to misbehave. The reason it does the opposite is a story about gradient attribution. When the training prompt explicitly asks for the unwanted behavior, the loss-reducing update has a clean local explanation ("the output matches the request in the prompt") and does not need to globally modify the model's general dispositions. When the prompt is neutral and the data still contains the unwanted behavior, the model has to rewrite higher-level concepts to make the data probable — and those rewrites generalize.

This is the same shape of argument that explains why some "model organism" constructions deliberately use system-prompt scaffolding to keep behavior locally attributable rather than globally absorbed. It also explains why the pre-FT elicitation heuristic works: a prompt that already makes the unwanted output high-probability gives the gradient the smallest possible thing to learn.

The companion paper from Tan et al. (`2510.04340`) develops the same core idea using system-prompt-style inoculation and demonstrates it across additional regimes including emergent misalignment and subliminal-learning trait transmission. Comparing the two: Wichers et al. concentrate on the SFT-prompt formulation across rigorously matched capability/misbehavior pairs; Tan et al. concentrate on breadth across trait types. Both should be cited together when motivating training-time prompt-level interventions.

A practical point: the intervention is data-cheap. No new data is collected, no new labels are needed — only a prompt rewrite. That makes it attractive as a defense for fine-tuning APIs where the provider cannot fully audit user-supplied data but can intercept and modify the prompt template.

## Connections

First paper in the [[topics/model-organisms|model-organisms]] thread for this collection. Companions to capture next (not yet noted):

- Tan et al. 2025 — *Inoculation Prompting: Eliciting traits from LLMs during training can suppress them at test-time* (`2510.04340`). Parallel paper, same authors-set in part, system-prompt formulation; broader coverage including subliminal-learning trait transmission.
- Betley et al. 2025 — *Emergent Misalignment* (`2502.17424`). The phenomenon IP defends against.
- Wang et al. 2025 — *Persona Features Control Emergent Misalignment* (`2506.19823`). Mechanistic account; reports that benign post-hoc fine-tuning can restore alignment.
- Kaczér et al. 2025 — *In-Training Defenses against Emergent Misalignment* (`2508.06249`). Benchmarks four defenses that any IP comparison should sit next to.
- Anthropic 2025 — *Natural Emergent Misalignment from Reward Hacking in Production RL* (`2511.18397`). Reports a single-line system-prompt reframing reduces misalignment by 75–90% in production RL — the strongest existing evidence that prompt-level interventions scale.

## Open Questions

- **Mechanism.** The paper motivates the effect with a gradient-attribution argument but does not test it mechanistically. A probe-level or activation-patching analysis would say whether IP-trained models really do attribute the behavior locally (representation indistinguishable from clean model except in the prompt-conditioned head) or merely *suppress* it (high probe + compliant output — same surface, different internals). The distinction matters for adversarial robustness.
- **Stress-tests under distribution shift.** If a model is "inoculated" but the underlying capability for the unwanted behavior is preserved, prompt rephrasings or OOD inputs may still elicit it. The paper's main evaluation is on held-out prompts of the same kind; an alignment-faking-style test would be more demanding.
- **Cure vs. vaccine.** IP is applied *during* the fine-tune that introduces the unwanted behavior. Can it be applied *afterward* — i.e. fine-tune a model that has already absorbed the trait on inoculation-prompted data to undo it? The framing here is "preventive," but a curative variant would be operationally important when the inoculation is decided post hoc.
- **Composition.** If multiple unwanted behaviors are present in the training data, does a single inoculation prompt that requests all of them work, or does each need its own prompt? This determines whether IP scales to realistic multi-property data.
- **Failure modes of the selection heuristic.** The pre-FT elicitation criterion is a correlation, not a causal mechanism — when does it break? Specifically, prompts that elicit the behavior via a *different* mechanism than the training data would induce might rank highly on the heuristic but underperform as inoculators.

## Code & Experiments

<!-- Not implemented yet. A natural first replication: pick the sycophancy setting (cheapest to evaluate), reproduce the baseline-vs-inoculated comparison on a small open-weight model, then add a linear-probe readout to test the "true elimination vs. suppression" distinction in the Open Questions section. -->
