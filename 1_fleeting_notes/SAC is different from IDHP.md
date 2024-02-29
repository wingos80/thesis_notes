parent: [[RL]]

Why does IDHP include using an identified dynamics model but not SAC?

IDHP needs this following equation to update its critic parameters:
![[Pasted image 20240229123803.png]]

Where the definition for $\frac{\partial \delta_t}{\partial x_t}$ requires a dynamics term $\frac{\partial x_{t+1}}{\partial x_t}$
![[Pasted image 20240229123953.png]]

But then what about the update rule for SAC, or other DRL's for that matter?

Ans: Because SAC is an algorithm that evolved from Q-learning, and Q-learning's update rules only require using sampled transitions. In contrast, IDHP evolved from ADP, which uses model dynamics to update its value estimates.

More specifically, because IDHP's critic estimates value function gradients, this causes state dynamic terms to pop up when expanding the error function for a critic. And since during update of function approximator parameters, the error function always analytically shows up, this means that model terms need to be used during function approximator updates. Hence. the reason why IDHP uses model information but SAC does not, can be attributed to the fact that the critic of IDHP models gradient information.
