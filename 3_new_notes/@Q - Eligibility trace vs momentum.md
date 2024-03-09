
- Date & time:*current date and time*
- Tag: #question
- Project:

---

## Question

Looking at the implementations for eligibility traces and momentum based gradient descent algorithms, it appears they are very similar, and in fact both essentially promise the same thing (improved sample efficiency, aka improved convergence speed).

This similarity was also spotted by the author of [this post]. So this begs the question, what is the difference between them?

[this post]: https://stats.stackexchange.com/questions/408046/difference-between-eligibility-traces-and-momentum

## Context

I had a random thought one day when considering eligibility traces and how they can be augmented into IDHP, and realised that the changes i would have to make resembles momentum approaches to gradient descent, so i began to wonder, what is their difference? 

This adds to the knowledge of eligibility traces, and understanding of how to improve sample efficiency of algorithms.

## Answer

This paper presents an algorithm which *combines* eligibility traces and momentum into one algorithm, without making any disambiguation on the similarities of the two:
[[@L - Momentum and mood in policy-gradient reinforcement learning]]

Which shows that the two can be used together, but does not really answer the big question. This second paper however, does:

[[@L - Applicability of Momentum in the Methods of Temporal Learning]]

The answer is essentially that eligibility traces only use the latest TD errors for parameter updates whilst keeping track of what parameters were *eligible* for update in the past. Whereas momentum effectively uses all previous TD errors (albeit at diminishing magnitude) since it keeps track of not only what parameters were *eligible*, but also by how much they changed. This answer is further corroborated by additional sources:

[[@L - A Comparison of Eligibility Trace and Momentum on SARSA in Continuous State- and Action-Space]]
[[@L - Learning with Eligibility Traces in Adaptive Critic Designs]]

All of these sources show results that eligibility traces produce better results than momentum in terms of e.g. sample efficiency, asymptotic performance, and success/no failure rates.

## Proof

See linked literature notes.