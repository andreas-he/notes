---
title: "Open-weight safety as a threat model"
date: 2026-04-23
draft: true
tags: [ai-safety, open-weight, threat-model, deployment]
type: post
description: "Safety properties that hold for a model in the lab rarely survive fine-tuning, quantization, or composition into real deployments. Open-weight release is where that gap becomes concrete and unrecoverable."
connections: []
crossposted: []
---

# Open-weight safety as a threat model

> **One-line thesis:** Safety properties that hold for a model in the lab rarely survive what the world does to it after release — and once weights are open, that gap is concrete and unrecoverable.

<!-- DRAFT. Migrated from memory/career-mission-framing.md (Apr 18 BlueDot workshop).
     Needs a second pass: sharpen the argument, trim the "why I care" section, add
     at least one concrete example of a post-release safety failure, connect to
     the compositional-misalignment thread. Not ready to publish. -->

## Why this matters

Most alignment research is evaluated on the model the lab trained. The world deploys something else: a fine-tune of that model, a quantized copy of that model, a chain of agents built around that model, or a retrieval pipeline that feeds it context the safety evals never imagined. Each of these transformations can erode safety properties that were genuine in the checkpoint the lab released.

Open-weight release is where this becomes acute. Once weights are public, a safety property either survives every transformation the community will apply, or it doesn't hold at all. There is no recall. The safety story you could tell at release is the strongest one you will ever get to tell.

This is a near-term, concrete harm surface — not a 10-year AGI scenario. Fine-tunes that strip refusal training exist today. Poisoning attacks on released weights exist today. Open-weight models get composed into agent systems nobody audits. The failure modes aren't hypothetical; the methodology to detect them at scale mostly is.

## Argument

**Safety is a property of a deployment, not of a checkpoint.** The thing that can harm a user is the system as deployed — model + scaffolding + tools + context + downstream fine-tuning. A lab can only sign off on the checkpoint. Everything after release is out of scope for the release eval, and in scope for whoever runs the system.

**Open-weight release collapses the responsibility gap.** Closed-weight release lets the lab maintain some safety by controlling the inference stack. Open-weight release transfers that responsibility to an ecosystem that has no aligned counterpart to the lab's alignment team. If the safety story depends on the inference stack, open-weighting removes the stack.

**The research agenda writes itself once you take this seriously.** Concrete questions:

- Which safety interventions (RLHF, constitutional AI, representation engineering, steering vectors) survive a few thousand steps of fine-tuning on innocuous data? Which don't?
- Do refusal behaviors degrade monotonically under quantization, or are there sharp thresholds?
- Can we build probes that run against any open-weight model to detect whether post-hoc fine-tuning has stripped specific safety training?
- What properties *do* survive open-weighting, and can we design training methods that reinforce them specifically?

**This complements compositional misalignment, it doesn't replace it.** Open-weight + composition is strictly worse than either alone: a composed system built on open-weight components inherits both "weights were tampered with" and "evals were for single models" failure modes. The research agenda covers both because the same measurement tooling generalizes.

## What I'm uncertain about

- Whether "open-weight safety" is a durable research frame or whether the field folds it into robustness / evaluation within a year. If it's absorbed, the niche thins.
- Whether the most interesting work is defensive (detecting tampering, building probes) or offensive (characterizing which safety properties can be broken, at what cost). The offensive path is higher-signal but harder to publish without attracting concern about dual use.
- How much of this is already being done at scale inside frontier labs and simply unpublished. Distance-to-frontier matters for whether independent work contributes or duplicates.

## Connections

<!-- To be wired up as more material migrates:
     - [[research-inverse-scaling-incoherence]] — related methodology for post-hoc probes
     - [[hot-mess-of-ai]] — compositional misalignment strand of the same threat model
-->

## References

- BlueDot AI Safety Strategy workshop, Apr 2026 — source discussion for this framing
