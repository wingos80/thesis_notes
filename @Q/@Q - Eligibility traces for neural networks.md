
- Date & time: 24/03/2024
- Tag: #question
- Project:

---

## Context

When trying to code the rules for eligibility updates, i realised there was a big problem to incorporating eligibility in neural networks. The gradient of function approximator output wrt it's parameters in the case of neural networks are dependent on existing parameters, and additionally their formulation strictly depends on matrix operations where order of operations highly matters. All this leads to the gradient of neural network output wrt hidden layer weights being incompatible with the idea of eligibility, however there seems to be ways around this, so the question is...:

## Question

How do we make the idea of eligibility traces work for neural network parameter updates?

## Answer

hmm, need to read and understand several sources first.

## Proof