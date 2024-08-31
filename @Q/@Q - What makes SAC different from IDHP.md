
- Date & time: 08/03/2024
- Tag: #question
- Project:

---

## Context

Trying to understand exactly what distinct IDHP from SAC. On 31/08/2024 i revisited this note to fit it into the @Q zettelkasten template, and then i realised i don't understand what i said in the answer anymore, it was worded very confusingly. I think the key point is that SAC comes from Q learning which is model free, whereas IDHP came from ADP which is model based. 

## Question

Why does IDHP include using an identified dynamics model but not SAC?

IDHP needs this following equation to update its critic parameters:
![[Pasted image 20240229123803.png]]

Where the definition for $\frac{\partial \delta_t}{\partial x_t}$ requires a dynamics term $\frac{\partial x_{t+1}}{\partial x_t}$
![[Pasted image 20240229123953.png]]

But then what about the update rule for SAC, or other DRL's for that matter?

## Answer

SAC is an off-policy algorithm, since it uses experience replay for actor learning, meaning it takes actions performed by an earlier version of the actor to update the current actor, thus the behaviour and target policies are distinct. SAC is also a model-free algorithm as no model dynamics is used to help with updating. Whereas IDHP is an on-policy & model-based algorithm.


SAC is an algorithm that evolved from Q-learning, and Q-learning's update rules only require using sampled transitions. In contrast, IDHP evolved from ADP, which uses model dynamics (state transition dynamics) to update its value estimates.

IDHP's td error is defined using future estimates, and takes that derivative wrt current states. And since during update of function approximator parameters, the error function always analytically shows up, this means that model terms need to be used during function approximator updates. 
