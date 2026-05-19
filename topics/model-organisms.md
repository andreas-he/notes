---
title: "Model Organisms of Misalignment"
tags: [topic, model-organisms, alignment, fine-tuning]
papers:
  - "[[inoculation-prompting-wichers]]"
last_updated: 2026-05-19
---

# Model Organisms of Misalignment

> **Thread summary:** Deliberately constructed examples of misaligned or unwanted model behavior — built so we can study *how* misalignment forms during training, and *what interventions* prevent or reverse it.

## Overview

Model organisms in alignment research are intentionally produced models that exhibit specific failure modes — emergent misalignment from narrow fine-tuning, sycophancy, reward hacking, deceptive behavior under triggers. The point is not the failure itself but the experimental control: a known cause produces a known effect, so interventions can be evaluated rigorously.

A growing thread of work studies *training-time interventions* — modifications to the data, prompts, or optimization process that prevent a known misalignment from being internalized in the first place. This is distinct from post-hoc safety training, which tries to remove an already-learned behavior.

## Key Papers

- **[[inoculation-prompting-wichers]]** — Modify training prompts to explicitly request the undesired behavior; the model fails to internalize it without losing the capability the fine-tune was meant to teach.

## Emerging Themes

- **Local vs. global attribution of behavior.** When a fine-tune induces a behavior, the model can either represent it as locally conditioned (on a prompt, a context, a trigger) or globally absorbed (as part of its general disposition). Training-time interventions seem to work by keeping the attribution local.
- **Pre-training-time signals as predictors of post-training-time outcomes.** Several recent results suggest cheap, training-free measurements on the base model can predict how a given fine-tune will generalize. This is one of the more practically useful threads in the area.
- **Prevention beats cure.** Most evidence so far is on preventing a misalignment from forming; reversing one after it has formed is harder and less studied.

## Our Take

This thread is the empirical scaffolding for the broader question of *how to control what gets learned during fine-tuning*. The mechanistic story — local vs. global representation, gradient attribution, persona feature dynamics — is still being assembled, and the experimental practice of constructing clean model organisms is what makes that assembly tractable.

## Open Questions

- Do "inoculated" models retain the latent capability for the undesired behavior, or is it genuinely eliminated? (Probe-level vs. surface-level distinction.)
- Can these interventions be composed across multiple undesired behaviors simultaneously?
- Do training-time prompt interventions transfer to RL-style training, where the "prompt" framing is less natural?
- What makes a misbehavior "amenable" to inoculation? Are there traits that resist this approach?
