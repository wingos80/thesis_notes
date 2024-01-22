This literature review focuses on the field of RL, seeing what algorithms exists, how they have been applied, and what suggestions that present researchers have.

<figure>
    <figcaption><b>Table 1</b>, broader RL based search queries used on Scopus.</figcaption>
</figure>

| Subject Area|  Query |
| ------------   |  ------- |
|  | "reinforcement learning" AND "control systems"|
|  | "reinforcement learning" AND "flight control"|


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

## Item 7: Research on Intelligent Control Method of Launch Vehicle Landing Based on Deep Reinforcement Learning

**From Stefan Heyer's thesis, i found a paper that seemed very seminal in the field of ADP, which i am interested in because the incremental version of it can be used as an online controller. And i also checked the list of papers that cited this seminal paper, which seems to suggest novel RL algorithms, which are listed in the following**

1. J. Ye, Y. Bian, B. Luo, M. Hu, B. Xu and R. Ding, "Costate-Supplement ADP for Model-Free Optimal Control of Discrete-Time Nonlinear Systems," in _IEEE Transactions on Neural Networks and Learning Systems_, vol. 35, no. 1, pp. 45-59, Jan. 2024, doi: 10.1109/TNNLS.2022.3172126.
2. D. Wang, J. Wang, M. Zhao, P. Xin and J. Qiao, "Adaptive Multi-Step Evaluation Design With Stability Guarantee for Discrete-Time Optimal Learning Control," in IEEE/CAA Journal of Automatica Sinica, vol. 10, no. 9, pp. 1797-1809, September 2023, doi: 10.1109/JAS.2023.123684.
3. Y. Li and E. -J. v. Kampen, "Incremental Generalized Policy Iteration for Adaptive Attitude Tracking Control of a Spacecraft," 2023 European Control Conference (ECC), Bucharest, Romania, 2023, pp. 1-6, doi: 10.23919/ECC57647.2023.10178221.
4. L. Chen and Q. Quan, "Reinforcement Learning for Non-Affine Nonlinear Non-Minimum Phase System Tracking Under Additive-State-Decomposition-Based Control Framework," 2023 IEEE 12th Data Driven Control and Learning Systems Conference (DDCLS), Xiangtan, China, 2023, pp. 2043-2049, doi: 10.1109/DDCLS58216.2023.10166298.

## Item \*: Adaptive critic designs

link: https://ieeexplore-ieee-org.tudelft.idm.oclc.org/document/623201


