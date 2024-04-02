
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

2. Gradient divergence issue refers to difference in gradients computed using latest network weights and previous network weights. This issue is only noticeable in nonlinear functions such as neural networks, where gradients of certain parameters depend on some of the function parameters itself. This divergence issue makes neural networks incompatible with eligibility traces as they are originally formulated. This is because eligibility traces assumes gradients stored in the trace are all computed from the same parameters, which is true for linear functions but not for nonlinear functions (neural networks). In fact, the deeper the network, the more inadmissible the trace gradients become. A method to fix this can be found in the paper discussed in [[@L - Adaptive and multiple time-scale eligibility traces for online deep reinforcement learning]]. However, as mentioned, the deeper the network is the more sever this issue becomes, and for shallow networks used in IDHP this problem has thus far not appeared.

## Proof