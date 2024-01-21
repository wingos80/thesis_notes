This literature review focuses on the field of RL, seeing what algorithms exists, how they have been applied, and what suggestions that present researchers have.

<figure>
    <figcaption><b>Table 1</b>, broader RL based search queries used on Scopus.</figcaption>
</figure>

| Subject Area|  Query |
| ------------   |  ------- |
| Engineering | "reinforcement learning" AND "control systems"|


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

**From Stefan Heyer's thesis, i found a paper that seemed very seminal in the field of ADP, which i am interested in becase the incremental version of it can be used as an online controller. And i also checked the list of papers that cited this seminal paper, which seems to suggest novel RL algorithms, which are listed in the following**

1. J. Ye, Y. Bian, B. Luo, M. Hu, B. Xu and R. Ding, "Costate-Supplement ADP for Model-Free Optimal Control of Discrete-Time Nonlinear Systems," in _IEEE Transactions on Neural Networks and Learning Systems_, vol. 35, no. 1, pp. 45-59, Jan. 2024, doi: 10.1109/TNNLS.2022.3172126.
2. D. Wang, J. Wang, M. Zhao, P. Xin and J. Qiao, "Adaptive Multi-Step Evaluation Design With Stability Guarantee for Discrete-Time Optimal Learning Control," in IEEE/CAA Journal of Automatica Sinica, vol. 10, no. 9, pp. 1797-1809, September 2023, doi: 10.1109/JAS.2023.123684.
3. Y. Li and E. -J. v. Kampen, "Incremental Generalized Policy Iteration for Adaptive Attitude Tracking Control of a Spacecraft," 2023 European Control Conference (ECC), Bucharest, Romania, 2023, pp. 1-6, doi: 10.23919/ECC57647.2023.10178221.
4. L. Chen and Q. Quan, "Reinforcement Learning for Non-Affine Nonlinear Non-Minimum Phase System Tracking Under Additive-State-Decomposition-Based Control Framework," 2023 IEEE 12th Data Driven Control and Learning Systems Conference (DDCLS), Xiangtan, China, 2023, pp. 2043-2049, doi: 10.1109/DDCLS58216.2023.10166298.

## Item 3: Adaptive critic designs

link: https://ieeexplore-ieee-org.tudelft.idm.oclc.org/document/623201


