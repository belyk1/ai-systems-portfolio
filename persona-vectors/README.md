# Persona vectors for style traits, and monitoring a closed model through an open one

A study built on Anthropic's *Persona Vectors* method (Chen et al., 2025; code at [safety-research/persona_vectors](https://github.com/safety-research/persona_vectors)). Two questions:

1. Do fine-grained style traits, not just the alignment traits in the paper, show up as directions in a model's activations?
2. Can an open-weight model's activations monitor a closed model's outputs, where the closed model's own activations are out of reach?

Run in May 2026. Every number below was recomputed from the saved data in October 2026. The answer to the first question is yes, with qualifications. The answer to the second is negative: the proxy result the experiment first produced did not survive the recheck.

## Setup

- Model whose activations are read: Qwen2.5-7B-Instruct, layer 20, response-averaged mean-difference vectors (the paper's main variant), filter threshold 50.
- Closed model being monitored: Claude Haiku 4.5.
- Judge: Claude Haiku 4.5, scoring each response 0–100 for the trait and for coherence.
- Port of the official pipeline to Windows and a single consumer GPU: Hugging Face transformers in place of vLLM, and a Claude judge that parses a text score in place of the original GPT-4.1-mini log-probability judge.

## 1. Replication on an alignment trait

On the paper's "evil" trait, responses to the positive system prompt averaged 73.9 on the judge's scale and responses to the negative prompt averaged 0.1. The correlation between a response's projection onto the vector and its judged trait score was **r = 0.91** (n = 970). The pipeline reproduces the paper's basic effect on this setup.

## 2. Style traits as directions

Five style traits, each defined by contrasting system prompts: sentence-fragment cadence, use of contractions, soft imperatives, warm-but-direct tone, and a short-word register.

| Trait | r, both prompt groups pooled | r within positive group | r within negative group |
|---|---|---|---|
| Fragment cadence | 0.97 | 0.20 | 0.66 |
| Contraction usage | 0.91 | 0.41 | 0.51 |
| Soft imperative | 0.91 | 0.71 | −0.26 |
| Warm-but-direct | 0.80 | 0.18 | 0.71 |
| Short-word register | 0.79 | 0.44 | 0.60 |

About 1,200 of 2,000 generated responses per trait have a usable judge score. The rest failed to score, from an API error or a reply the parser could not read, and may not be missing at random.

The pooled correlations are high, but most of that comes from separating the two prompt groups. Within a group, where the variation is the model's own and not the prompt's, the projection tracks the judged trait more weakly and less consistently (r from −0.26 to 0.71). That within-group number is the one that matters for monitoring, and it is the honest headline: the directions exist, and they track graded variation partially.

**Separability.** Cosine similarity between the five trait vectors ranged from 0.26 to 0.79. These traits overlap substantially. The most distinct pairs sit near 0.26–0.31, but several are above 0.6, and contraction usage and warm-but-direct (0.79) are close to the same direction. A monitor reading one would partly be reading another.

## 3. Monitoring a closed model by proxy: the result did not hold

The plan: have Claude write responses under the same contrasting prompts, judge them, run them through Qwen, and see whether Qwen's projections predict the judge's scores. The original analysis reported a strong result (R² up to 0.94). The recheck found two faults that void it.

**An ordering bug.** The calibration script gathered Claude's answers, and then the judge's scores, in the order the API calls finished, not the order they were sent. Answers ended up paired with the wrong questions, and scores with the wrong answers. Within each prompt group the pairing is effectively random, which is why the within-group correlation is about zero (−0.14 to 0.08). It probably also explains why the judge rated most answers as incoherent: it was shown each answer beside the wrong question. The pipeline's Qwen-side code stores results by index, so sections 1 and 2 are free of this bug. That clears one fault; it is not an independent validation of them.

**The instruction was in view.** When Qwen read each Claude answer, the text in front of it was the bare system prompt that told Claude which style to use, with no question. Qwen separates the two groups almost perfectly (AUC 0.975–1.00), but the separation may come from reading the instruction rather than from Claude's style.

So this experiment does not show that an open model can monitor a closed one this way. A valid test of the idea keeps results in request order, projects each answer with a neutral prompt that does not name the style, and measures correlation within each prompt group.

## Limits

- One model read, one layer, one vector variant.
- The judge and the monitored model are the same model (Haiku), so the judge may share the monitored model's habits.
- Style traits were induced by system prompts, not observed in natural use.
- Small samples: 300 Claude responses per group per trait.
- In section 2, as in the paper's design, Qwen reads each response with its system prompt in view. Each group mixes five instruction variants, so part of the within-group signal may track which variant was used rather than the response's own style.
- The judge reads the first number in a short reply, which can occasionally pick up a stray number.

## What I took from it

The first analysis looked like a clean positive result, and it was wrong for two independent reasons, both found only by recomputing from the raw data. Section 3 stays on this page for that reason. A monitoring result needs a within-condition check, and the monitor must not see the label it is supposed to infer.
