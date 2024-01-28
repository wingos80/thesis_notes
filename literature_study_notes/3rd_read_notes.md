This literature review focuses on the field of RL, seeing what algorithms exists, how they have been applied, and what suggestions that present researchers have.

<figure>
    <figcaption><b>Table 1</b>, broader RL based search queries used on Scopus.</figcaption>
</figure>

| Subject Area|  Query |
| ------------   |  ------- |
|  | "reinforcement learning" AND "control systems"|
|  | "reinforcement learning" AND "flight control"||
|  | "reinforcement learning" AND "fault tolerant control"|


---

## Item 1: Adaptive Optimal Flight Control for a Fixed-wing Unmanned Aerial Vehicle using Incremental Value Iteration

doi: 10.1109/ICM54990.2023.10101984

### Notes- 

- An LQR cost (in this paper called a utility function) is defined for each time step, and then the cost function (eq. 12)is defined to be a sum of these costs. This cost function's form is very reminiscent of the value function, in fact it essentially is the value function. To perform the optimal control is to make sure that the control action is optimal for this cost function, however analytically deriving this solution using this form of the cost function is not easy. An easier method would be to first approximate the terms that makes up this cost function, which would mean approximating the error (making up the utility function), and approximating the cost function of the next time step. After doing so it is possible to arrive at a quadratic function, for which an optimal can be analytically found as the global optimum of a quadratic function is where the derivative is zero.  
- The solution here is a control increment, and thus the optimal control increment can be found for a given cost function approximation. Computing this control increment using the quadratic function is called "policy improvement"
- This optimal control increment is added to the previous control command to give a current control signal. And this allows for "policy evaluation" to take place, where the approximating matrix $P$ is updated until convergence.


---

## Item 2: Nonlinear adaptive flight control using incremental approximate dynamic programming and output feedback

doi: 10.2514/1.G001762

### Questions-

1. She seems to claim that to use function approximators to approximate value functions is approximate dynamic programming, should confirm 

### Notes-

- As opposed to taking jacobians of the system equations to find the incremental models, which is an analytical means and requires an engineer to perform prior to making a controller, a least squares method to identify the incremental model online is done which allows the algorithm to effectively compute for itself the system model.
- IADP also relies on high sampling frequencies just like INDI, it in fact needs at least $n+m$ number of samples to identify each incremental model.
- A cost-to-go function which eerily resembles the value function in the MDP frame work is created.
- Ye then explains that in fact the cost-to-go function created is exactly the LQR cost function that is tackled in optimal control when the system being controlled is a linear system!!! wowwww
- IADP and IDHP are two ways of tackling the ADP problem, in IADP the cost-to-go function is parameterized as a quadratic function $ePe$ parameterized y the square positive definite symmetric matrix $P$ and uses the regulation error $e$. While  in IDHP i suspect that they parametrize the cost-to-go function as a neural network instead? Not sure, will need to read about it to find out. I.e, IADP is heavily based on optimal control and dynamic programming.
- My definition of value and policy iteration:
	- Value iteration is to compute the true value function for a given policy, the significance of *iteration* is that an iterative update equation is used which moves the value function estimate closer and closer to the true value function
	- Policy iteration, or more generally evaluation, is to find the optimal policy for a given value function.

---

## Item 3: Incremental Model Based Online Dual Heuristic Programming for Nonlinear Adaptive Control

### Questions

1. She keeps referring to the need to learn a *global system model* in adaptive critic designs, but what is this global system model? I need to research and learn more about adaptive critic design.

### Notes-

- Adaptive critic can also be called actor critic methods, since policy evaluation (the critic) and policy improvement (the actor) are approximated by two function approximators.
- 3 main groups of ACDs can be identified:
	1. Heuristic dynamic programming, the most basic form and uses critics to estimate cost-to-go/rewards.
	2. Dual heuristic programming, critics approximate gradients of cost-to-go/rewards with respect to critic inputs
	3. Globalized dual heuristic programming, which approximates cost-to-go/rewards as well as its' derivative.
- The 3 versions of ACD can all be extended to *action depended* versions of themselves, where the output (action) of the actor is fed as input to the critic functions.
-  Idea: current online-offline hybrids which lucas and casper made only deals with the actors in both halves of the algorithm, but if globalized DHP is used, then it would be possible to use the offline critic for the online algorithm too? (***with this idea i found a paper published also by zhou on an efficient online implementation of GDHP, see item 4, the next literature reviewed***)
- "conventional DHP controllers use 3 nonlinear function approximators to approximate 3 things: the actor, the critic, and the global system", this is in contrast with the internal structure of IDHP, where only 2 nonlinear function approximators are kept as only the actor and critics are approximated nonlinearly, while the global system is approximated with an RLS regressor instead.
- The critic in a DHP and the IDHP approximates the derivatives of the value function with respect to the state vector of the system being controlled, i.e. the state variables of the environment if we were to speak in terms of the gymnasium language.
- The actor in DHP and IDHP is once again our favourite operator: the argmin operator (actually does not look like that this function is used at all?)
- Eq 17 is an impressive substitution wow!


---

## Item 4: Efficient Online Globalized Dual Heuristic Programming With an Associated Dual Network

### Notes-

- GDHP requires calculating second order gradients, supposedly these sources should provide some information on that [source 1], [source 2]. Source 2 should also talk about why theoretically GDHP should be more powerful than DHP or HP.
- Previous attempts to find more practical methods of implementing GDHP faces the dichotomy of either high computational load for good accuracies or low computational load for bad accuracies.
- The proposed GDHP algorithm is called IGDHPa (incremental GDHP association), incremental because the algorithm's model of the global system a.k.a. plant is identified incrementally taking the form of a time varying LTI, association because the algorithm has a DHP style critic network which is *associated* to the HDP style critic network that the algorithm also has.
[source 1]: https://repository.essex.ac.uk/21300/1/IEEE%20SecondOrderBackpropForVGL.pdf
[source 2]: https://ieeexplore-ieee-org.tudelft.idm.oclc.org/document/623201

---

## Item 5: Optimal and Autonomous Control Using Reinforcement Learning: A Survey

### Follow up papers:

1. *A novel actor–critic–identifier architecture for approximate optimal control of uncertain nonlinear systems*. A paper on synchronous actor-critic update which should be "online".
	1. They use an integral bellman equation.
	2. It seems that the algorithm is indeed online, they only train the agent on one episode which is 10 seconds long, and within the first 3 seconds the estimates have all converged.
	3. The proposed algorithm solves the HJB equations, and uses a *Bellman residual error* to update the critic and actor networks		
		> The **HJB equation** is a necessary and sufficient condition for optimality of a given control action w.r.t. a given loss function. Iteratively solving this equation can provide the optimal value function for a given MDP.
		> The **Bellman residual** expresses the residual (error) between a value function estimate and a value function evaluated from a policy?

---


## Item 6: Adaptive Dynamic Programming for Control: A Survey and Recent Advances

### Notes-
- Very good introduction and litearture study on origins of adaptive dynamic programming.
- Origins of the Name of ADP:
	- ADP can be an acronym standing for either approximate or adaptive dynamic programming.
	- [First use] of the acronym ADP, and then explored formally by [Papachristos]
	- [Werbos] introduced the class of adaptive critic designs 
- kernel ADP's combine kernel machines with neural networks?
- *[Global ADP]*, a control method which solves a relaxed version of the HJB, providing a control policy which has **global asymptotic stable**. Should look into papers which cite this one and see how they use this.
- Generalized PI has been developed into a formal [algorithm], but can be used to refer to the general algorithm of iterative policy evaluation and policy improvement. 
- There also exists formalized Generalized VI [algorithms].
- Integral reinforcement learning allows for policy evaluation even when system dynamics are unknown.
- A mathematical definition for persistent excitation condition is given.

[First use]: https://scholar.harvard.edu/files/rzeckhauser/files/depletable_natural_resources.pdf
[Papachristos]: https://www.proquest.com/openview/04b160da9282e5ad0815d5a55424a6ee/1?pq-origsite=gscholar&cbl=18750&diss=y
[Global ADP]: https://doi.org/10.3182/20140824-6-ZA-1003.00523
[algorithm]: https://doi.org/10.1109/tnnls.2017.2661865
[algorithms]: https://doi.org/10.1049/iet-cta.2011.0783

---

## Item 7: DDPG-based active disturbance rejection 3D path-following control for powered parafoil under wind disturbances

### Notes-

- They posed DDPG as a more widely adopted algorithm for continuous state-action spaces alternate to DQN which is a little less modern, which in itself is more preferable over Q-learning due to the former being able to be effective under continuous spaces while the latter suffers from the curse of dimensionality as a result of its' tabular memory.
- The application of DDPG was not on controlling the system directly, but on deciding for parameters of a controller which is in turn used to control the system.
- The state consists of 2 variable (it's a 2d state space), first dimension is altitude error $s_1 = z - z_d$, second dimension is lateral position error $s_2 = \Delta d$. Reward is defined as the negative of the sum of the two states $r = -(s_1 + s_2)$, and return is defined as the integral of reward over time.

---

## Item 8: Learning Extreme Hummingbird Maneuvers on Flapping Wing Robots


### Notes-

- The goal is to make an at-scale hummingbird robot perform the same evasive flight maneuver that hummingbirds are able to make.
- The proposed approach is to adopt a hybrid controller composed of two controllers, a model-based nonlinear controller for nominal flight control, and a model-free reinforcement learning controller which activates when performing the maneuver and adds its' own control commands to the nonlinear controllers' commands. This hybrid method was deemed necessary as the nonlinear adaptive controller is not accurate enough to provide stabilizing control to the system.
	- The model-based controller is an adaptive sliding mode controller, with additional terms in the control law that ensures robustness to model uncertainty.
	- The model-free controller is based on DDPG with MLPs approximating the functions of the critic and actor.
- The RL problem was posed as follows. The robot is initialized flying upright, and the goal is for it to translate backwards by some distance $x_d$ while simultaneously ending up with the body frame being rotated 180 degrees about the $z$ (yaw) axis.
- The implementation of DDPG was based on [Deepmind's implementation]

[Deepmind's implementation]: https://arxiv.org/abs/1509.02971

---

## Item 9: The Option Keyboard: Combining Skills in Reinforcement Learning

a publication from deepmind

### Notes-

- The authors introduce a new hierarchical reinforcement learning framework, with the central notion of an *option keyboard*, which is a generalization of the idea of actions and options. This keyboard provides RL agents with a hierarchical interface to various levels of actions, where various options can be combined to execute complex tasks, leading to an even more emergent behaviour.
- Mentioned the ideas of generalized policy evaluation and generalized policy improvement to build up the framework needed to introduce the option keyboard, did not really understand the maths behind the reasoning.

---

## Item 10: Fast reinforcement learning with generalized policy updates

another publication from deepmind

### Notes-

- The idea of Fast reinforcement learning stems from seeking a way to reconcile the discrepancy between learning times of human and RL agents in simple tasks, the example given is that of learning how to play an Atari game. Whereas a human can learn roughly how to play a game in 15 minutes, for an RL agent to achieve the same level of play would require weeks upon weeks of uninterrupted play time. It is hypothesized by the authors here that this discrepancy arises from how RL agents have to learn essentially from scratch, but that if they were endowed with mechanisms to leverage prior knowledge, they would be much smarter.
- This mechanism is very useful to an agent that has successfully learnt an advantageous behaviour in one task, and needs to learn a behaviour in another task.
- They provide a formal definition of GPI.

 
---

## Item 11: Discrete Globalized Dual Heuristic Dynamic Programming in Control of the Two-Wheeled Mobile Robot


### Notes-

- [this paper] says DHP is better than HDP
- Their definition of the HDP, DHP, and GDHP architectures are completely different from what i am aware of.
- Their architecture of the GDHP involves the following, the actor is made up of $n$ neural networks where $n$ is the number of actions, and the critic is made up of one neural network which predicts the value function *but is updated according to the DHP and DHP rules???*

[this paper]: https://www.scientific.net/SSP.164.419

---

## Item 12: Online Learning Control Using Adaptive Critic Designs With Sparse Kernel Machines

### Notes-

- First to integrate Kernel Method into critics of an ACD method, what is a kernel method???
- They suggest sparse kernel machines have better generalization capability than MLPs.
- They require a model to predict the state transitions.
- So instead of using neural networks/MLPs to approximate the actor policy and critic value function, they used kernel machines.

--- 

## Item 13: Reinforcement Q-learning for optimal tracking control of linear discrete-time systems with unknown dynamics

### Notes-

- Remark 6 notes several sources stating the need for persistent excitation
- They use a LS method to solve the policy evaluation equation in order to update the P matrix, need to find how LS methods work.
- The presented Q-learning method does not use action-value functions, and hence no function approximators either. Instead they straight forwardly solve the algebraic riccati equations for optimal performance.


---

**From Stefan Heyer's thesis, i found a paper that seemed very seminal in the field of ADP, which i am interested in because the incremental version of it is very adaptive and thus can be used as an online controller. And i also checked the list of papers that cited this seminal paper, which seems to suggest novel RL algorithms, which are listed in the following**

1. J. Ye, Y. Bian, B. Luo, M. Hu, B. Xu and R. Ding, "Costate-Supplement ADP for Model-Free Optimal Control of Discrete-Time Nonlinear Systems," in _IEEE Transactions on Neural Networks and Learning Systems_, vol. 35, no. 1, pp. 45-59, Jan. 2024, doi: 10.1109/TNNLS.2022.3172126.
2. D. Wang, J. Wang, M. Zhao, P. Xin and J. Qiao, "Adaptive Multi-Step Evaluation Design With Stability Guarantee for Discrete-Time Optimal Learning Control," in IEEE/CAA Journal of Automatica Sinica, vol. 10, no. 9, pp. 1797-1809, September 2023, doi: 10.1109/JAS.2023.123684.
3. Y. Li and E. -J. v. Kampen, "Incremental Generalized Policy Iteration for Adaptive Attitude Tracking Control of a Spacecraft," 2023 European Control Conference (ECC), Bucharest, Romania, 2023, pp. 1-6, doi: 10.23919/ECC57647.2023.10178221.
4. L. Chen and Q. Quan, "Reinforcement Learning for Non-Affine Nonlinear Non-Minimum Phase System Tracking Under Additive-State-Decomposition-Based Control Framework," 2023 IEEE 12th Data Driven Control and Learning Systems Conference (DDCLS), Xiangtan, China, 2023, pp. 2043-2049, doi: 10.1109/DDCLS58216.2023.10166298.

## Item 14: Adaptive critic designs

link: https://ieeexplore-ieee-org.tudelft.idm.oclc.org/document/623201


---

## Item 15: Incremental Generalized Policy Iteration for Adaptive Attitude Tracking Control of a Spacecraft

### Notes-

- Using the original formulation of a GPI algorithm, an incremental form is developed by inserting an RLS identified system model into the Bellman equation, which allows for the equation to be computed to build the policy iteration algorithm.

---

## Item 16: Adaptive Multi-Step Evaluation Design With Stability Guarantee for Discrete-Time Optimal Learning Control

### Notes-

- [this source] implemented a multi-step ADP algorithm (might be the first?), creating the class of algorithms called Multi-step ADP (MsADP), the variant of  ADP used is HDP, thus the paper presented a MsHDP alorithm.
- This paper then extends that idea further to create an *integrated* MsADP, specifically the MsHDP. The core idea of *integrate* MsHDP is to use small $N$'s in the initial iterations, with $N$ being the number of steps taken in the Multi-**step** of the MsHDP, then after the onset of theoretical stability of the algorithm $N$ can be increased to massively speed up policy convergence.
- There also exists an [integrated VI algorithm]?
- The idea behind an *integrated* algorithm is that PI only converges when using an admissible control policy, i.e. a stable one. However, VI can work even with an inadmissible policy. Therefore, the strategy is to initially use VI, i.e. use less steps in the beginning for the Multi-step part of the algorithm, and then once a stability criterion is reached, to switch over to using large number of steps to emulate PI, which converges faster at the expense of extra compute.

Couple questions:
1. Is it worth investigating a multistep version of DHP vs HDP?
2. If so, how can a Ms version of DHP be developed using the framework proposed?

For q1, according to Zhou 
> "DHP methods are most favored within the ACD category because they have higher success rate and accuracy than the HDP and lower computational complexity than the GDHP"
which is indeed confirmed in several papers, such as:
1. G. K. Venayagamoorthy, R. G. Harley, D. C. Wunsch, Comparison of heuristic dynamic programming and dual heuristic programming adaptive critics for neurocontrol of a turbogenerator, IEEE Transactions on Neural Networks 13 (3) (2002) 764–773.
2. J. Si, Y.-T. Wang, Online learning control by association and reinforcement, IEEE Transactions on Neural Networks 12 (2) (2001) 264–276.
3. D. V. Prokhorov, D. C. Wunsch, Adaptive critic designs, IEEE transactions on Neural Networks 8 (5) (1997) 997–1007.

Therefore, it is possible that an MsDHP would out perform MsHDP when it comes to performance and robustness. However, it is necessary to also consider if IDHP is already fulfilling the same purpose as MsDHP, since the primary benefit of IDHP is the rapid convergence to a stable policy, and the benefit of MsDHP would also be a rapid convergence to stable policy.

The incremental part of IDHP or IHDP is there to eliminate the need to model the underlying system being controlled. In traditional ADP's, there exists some model of the system that the algorithm keeps track of. Usually, this either means that you need to have a degree of a-priori knowledge of the system which can allow for sufficient modelling of the state transition dynamics, or alternatively you need to build a system which is usually done slowly through function approximators such as neural networks. Incremental versions of ADP algorithms use an online model identification approach and the assumption of high sampling rate to utilize an RLS identified time varying linear system as the system model, instead of the usual nonlinear models or expensive-to-create yet poorly-generalized time independent linear models. This means that now a big portion of the computational complexity of ADP algorithms is eliminated, as RLS is a very efficient operation, and under high sampling rates of typically around 100 Hz can offer sufficient approximation of state transition dynamics. And thus, this allows ADP algorithms to create online adaptive controllers.

Comparing this solution to the MsHDP solution, it was found that MsHDP does not use a system model, while IHDP does (the recursively identified one), this discrepancy needs to be studied further by reading the original papers describing HDP/ADP. The paper [Online Adaptive Critic Flight Control] is read to learn more about this (did not read in detail).

On further reflection, instead of directly adopting the Algorithm of MsHDP, it may be better to adopt the more general idea of multi-step bootstrapping, such as n-step TD learning, and apply it to IDHP. The questions to be asked in this case are:

1. How would IDHP be made multistep?
	1. Could be done by expanding equation 15 in Zhou's IDHP paper:
		$e_c(t) = \frac{\delta [\mathcal{J}(x_{t-1}) - c_{t-1} - \gamma \mathcal{J}(x_t)]}{\delta x_{t-1}} = \lambda(x_{t-1}) - \frac{\delta c_{t-1}}{\delta x_{t-1}} - \gamma \lambda(x_t)\frac{\delta x_t}{\delta x_{t-1}}$
		$e_{Ms\ c}(t) = \frac{\delta [\mathcal{J}(x_{t-2}) - c_{t-2} - \gamma \mathcal{J}(x_{t-1}) - \gamma^2 \mathcal{J}(x_{t})]}{\delta x_{t-2}} = \lambda(x_{t-2}) - \frac{\delta c_{t-2}}{\delta x_{t-2}} - \gamma \lambda(x_{t-1})\frac{\delta x_{t-1}}{\delta x_{t-2}}- \gamma^2 \lambda(x_t)\frac{\delta x_t}{\delta x_{t-1}}\frac{\delta x_{t-1}}{\delta x_{t-2}}$
1. At first glance or surface level inspection, is there any additional benefit to using a multi-step IDHP?


[this source]: https://www.sciencedirect.com/science/article/pii/S0020025516312853
[integrated VI algorithm]: https://ieeexplore.ieee.org/document/9536025?denied=
[Online Adaptive Critic Flight Control]: http://lisc.mae.cornell.edu/LISCpapers/Online%20Adaptive%20Critic%20Flight%20Control.pdf