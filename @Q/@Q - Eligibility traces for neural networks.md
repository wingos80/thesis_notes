
- Date & time: 24/03/2024
- Tag: #question
- Project:

---

## Context

When trying to code the rules for eligibility updates, i realised there was a big problem to incorporating eligibility in neural networks. The gradient of function approximator output wrt it's parameters in the case of neural networks are dependent on existing parameters, and additionally their formulation strictly depends on matrix operations where order of operations highly matters. All this leads to the gradient of neural network output wrt hidden layer weights being incompatible with the idea of eligibility, however there seems to be ways around this, so the question is...:

## Question

How do we make the idea of eligibility traces work for neural network parameter updates? Specifically:

1. How to fix the shape mismatch issue between accumulated traces, and td error.
2. How to fix the supposed gradient divergence issue noted by some in literature.

## Answer

1. Shape mismatch can luckily be solved rather easily. Take a neural network which has $n$ weights/trainable parameters, and $m$ outputs. This network will have a TD error that is an $m$ vector, since for each network output, there is a corresponding target, thus a corresponding TD error. By expressing the network's outputs as a vector of equations, i.e. an analytical expression for each of the $m$ network output, it is possible to obtain the network's weight Jacobian by taking the derivative of this vector w.r.t each of the weight $n$; which would result in a $m x n$ matrix, which can be stored to create the network's eligibility traces.
   
   To obtain the weight updates for each time step, this eligibility trace can be multiplied by that time step's TD error vector, which is an operation that can be commuted as the error vector has $m$ elements and the traces are in the shape $n x m$, thus the matrix product between the error vector and the traces produces a vector of $n$ weight updates.

2. Gradient divergence issue i think does not refer to the magnitude of the gradients, but rather the difference between the gradients computed by latest parameter and gradient presented by eligibility traces, need to read .

## Proof