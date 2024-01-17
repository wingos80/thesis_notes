# Master Notes

File containing todo list and all miscellaneous/thinking-out-loud notes throughout my thesis.

## Table of Contents

1. [List of TODOs](#todo)
2. [Daily Notes](#daily)

---

## List of TODOs <a name="todo"></a>

Ranked, 1 = highest priority

1. Read up on Sutton & Barto
2. Read up on flying-V
3. Read up on Hierarchical RL (maybe HRL for continuous algos? For SAC?)
5. Read Ye Zhou's PhD dissertation which presents iADP, IDHP, and hierarchical RL.
4. Look into evolutionary reinforcement learning (what's the deal with using GA over ES?)
6. Look at Lucas/Caspers repos to study IDHP implementation

---

## Daily Notes <a name="daily"></a>


### 4/1/2024

- I am slowly developing a framework for the work i need to carry out to tackle this thesis, and it comes in 3 parts:

    part 1, the flying v. I need to become an expert in everything related to the flying v
    part 2, RL. I need to become an expert in rl
    part 3, the code. The code will be distinguished into 2 major parts, the simulation/training environment that i will need to create, with all the gym.methods() present; and then the actual RL algo to be used, which i will need to design from my knowledge of the field of RL and then implement in python, possibly following the outline of the algos in spinning up.

### 5/1/2024

- *auxiliary stuff:* Needed to send proof of funding and BSc diploma to OCW for the knowledge embargo today.
- Also wrote a couple definitions.

- Return at any particular time instant G_t is equal to the sum of rewards from all subsequent time instances R_{t+1}. A reasonable deduction from this definition is that to maximum the value of an agents' return, one must select a sequence of actions to maximize thie sum of rewards. This would not be a problem if the rewards and actions were deterministic, however in most cases they are stochastic as is the case in the classical formulation of reinforcement learning problems. Meaning it is not possible to find the exact return given a set of actions, and therefore this approach is flawed. Fortunately, this can be fixed with a small tweak. This tweak is: instead of maximizing the value of an agents' return, one should seek to maximize the **expected value** of an agent's rewards from subsequent time instances. The important distinction between these two notions is in using the **expectation** of reward instead of the value of reward itself. And to further develop this tweak, the probabilities of rewards and actions then becomes a necessary ingredient to optimizing an agent's action.

    The principle of maximizing an expected return is the principle of reinforcement learning in a nutshell, 

- **MY PLAN:** to start out by reading the theory from willem's report, from which i can make a more broad literature study (BLS). The BLS shall be divided into: the RL segment, where i continue off of RL sources from willem; the flying-V segment; and looking at implementation + results of willem + Lucas. What i am more concerned with now are: when will embargo get approved, will i be able to implement correct RL algo, how will i find a novel RL algo.
- For comparing algos, could either keep episode length fixed, or keep termination moving avg cost (MAC) fixed. For former, keep track of final MAC, for latter, keep track of how long to arrive at terminal MAC.
- There is a ``* (1 - done_list)`` factor in the target term in the lunar lander DQN file, what is that?

> [!Definition]+
> **Dynamics of a MDP**
>  Dynamics of a MDP is expressed by a conditional probability function, p(s',r|s,a), i.e. probability of state being s' and reward being r given was that the previous state was s and action a was taken. This is sometimes called the "*four-argument p*".

> [!Definition]+
> **State transition probability**
>  State transition probability is defined as p(s'|s,a), i.e. probability of state becoming s' given an agent was in state s and took action a.

> [!Definition]+
> **Value function**
>  The value function states the **expected return** of a state, recall the definition of return. It is a.k.a the *state value function*

> [!Definition]+
> **Bellman equation of the Value function**
> The Bellman equation of the Value function is simply **the** definition of the value function for a given state. It returns the same value as the value function, so in the sense of what it computes the Bellman equation of a value function is equal to the value function. But the equation itself allows you to compute the value function, unlike the equation for the value function which is simply an expectation operator taken on the return of a state. It is written as a function of the dynamics of the MDP, the entire policy of the agent, every possible the rewards, and every next possible state's value function. The equation's derivation is shown in figure 1 [from].

> [!Definition]+
> **Bellman equation**
> The Bellman equation refers to an equation which can be used to compute (typically) a given value function (but also other functions though i am not sure what other functions).



![[./pictures/bellman_derive.png]]
<figure>
<figcaption><b>Figure 1</b>, derivation of the bellman equation.</figcaption>
</figure>

[from]: https://stats.stackexchange.com/questions/243384/deriving-bellmans-equation-in-reinforcement-learning


### 6/1/2024

- [Deadly triad]: instability and divergent behaviour exhibited by algorithms which combine off-policy learning, bootstrapping, and function approximation. TD can be synonymous with bootstrapping.
- Small [online] discussion on on vs off policy RL.
- Made a list of most relevant RL MSc theses.

**List of RL MSc theses under Erik-Jan (that i could find):**

1. Joost Dorscheidt (N/A, Nov 2018): **flexible DDPG** (curriculum(ed) learning), novelty: the algo used
2. Jonathan Hoogvliet (20 months, Nov 2019): **HRL** (Hierarchical Reinforcement learning) , novelty: the algo used
3. Dave Kroezen (12 months, Apr 2019): **DHP**, novelty: the algo used
4. Stefan Heyer (12 months, Mar 2019):  *IDHP* i think?
5. Ramesh Konatale (21 months, Jan 2020): **IADP**, novelty: the algo used
6. Killian Dally (10 months, Feb 2021): **SAC**, novelty: the algo used (only used for non-terminal flight phase)
7. Zhou Xin Ge (N/A, Aug 2021): **PPOC** (Proximal Policy optimization with Option-Critic), novelty: comparing/using PPOC & PPO
8. Willem Völker (10 months, Jun 2022): **TD3** , novelty: the algo used (didnt seem like prev studs used TD3), used on flying-V
9. Casper Teirlinck (11 months, Sep 2022): **SAC + IDHP**, novelty: hybrid (2+ algos combined) online offline
10. Peter Seres (10 months, Oct 2022): **DSAC**, novelty: the algo used
11. Lucas Viera (11 months, Jul 2023): **DSAC + IDHP**, novelty: hybrid (2+ algos combined) online offline
12. Thomas vd Laar (13 months, Jun 2023): **SAC**, novelty: used in landing
13. Vlad Gavra (11 months, Jan 2023): **TD3**, novelty: EA based RL

[Deadly triad]: https://k4ntz.medium.com/why-is-there-a-deadly-triad-issue-and-how-to-handle-it-e1839b1a8b40
[online]: https://datascience.stackexchange.com/questions/13029/what-are-the-advantages-disadvantages-of-off-policy-rl-vs-on-policy-rl

> [!Definition]+
> **Actor Critic Design (ACD)**
>  In ACD an internal division of tasks(controlling and learning) and components (actor and critic)is made in an effort to reduce the complexity of the total problem.

> [!Definition]+
> **$\epsilon$ greedy**
> To be epsilon greedy is to apply a greedy approach but allowing in a small (epsilon small) probability the selection of a non greedy option.

### 7/1/2024

- New perspective on how "reward" is defined, reward should be a function of both action and state, i.e. what action was taken from what state. Not simply just the state...
- Curriculum learning, a new topic on RL i have never heard of before. Where you shape the learning process of your agent, introducing it to easier tasks before learning harder tasks, in aerospce terms you introduce the nominal parts of the flight envelope to the agent before the extreme parts.
- When it comes to applying reinforcement learning in the real world, all RL based controllers will neccessarily have to be pretrained offline. An airplane simply cannot be flown with a purely online trained controller. However, where online training could be useful is in situations when the controller parameters need to be adapted, for instance when entering unexpected regions of the flight envelope, or aircraft faults.
- What is "option"? 
- Done with reading chapter 2 and 4 of willems thesis, now, what to do next...
- **MY PLAN 2:** Eventually... make a list of algorithms and state their meta-data (ideally organize in table). I will make a short summary of all 13 RL MSc theses i have found so far, which should only take me 1.5 days. And then I will begin reading Barto & Sutton, which is probably going to take 2 days to 4. Finally, i will read about the flying V (literature t).

Algorithms:

DSAC, SAC, TD3, IDHP, IADP, PPO, flexible DDPG,

### 8/1/2024

- ERL uses the very old GA mutation and selection operators, but i am very confident that ES (specifically the CMA-ES variants) can provide much better real valued optimization performance than the traditional GA can. Look into this more later. Also, use this paper from [Cristian Bodnar] et al to read about PDERL.

[Cristian Bodnar]: https://arxiv.org/pdf/1906.09807.pdf

- iADP (I like to call it **I**ADP) is an online type of RL algo and supposedly results in "highly adaptive controllers" according to Thomas vd Laar. DRL in contrast, are offline algos.
- I found pseduo code for a bunch of DRL's in Thomas vd Laar's thesis appendix A, which were all taken from different sources. I will rewrite these pseudo code in my own terms and cite these sources.
- Lol Thomas refernced Clark's lecture slides??
- The ILS is made up of a LOC (localiser) and a GS antenna (glideslope)
- Use parallel coordinate plots for hyperparameter tuning.
- ADP is Approximate dynamic programming!!!
- Sample efficiency generally refers to achieving higher rewards in smaller number of samples
- I need to understand the developments that led to SAC & TD3, and then the significance/meaning of distributional RL.
- CAPS penalizes temporal and spatial variation in control policy.

### 9/1/2024


- Continue doing litearture review of the preview MSc theses. First read the abstract, then read the thesis conclusion, then look at the scientific article conclusion, then verify and clarify any claims and points that are made.
- The hybrid controller from Casper and Lucas needs to be excited online to set the IDHP controller, and then these trained parameters are used as the initial conditions for the IDHP controller in "actual" online flights.
- [REINFORCE] is the simplest and classical policy gradient RL method. The name is an acronym: (**RE**ward **I**ncrement = **N**onnegative **F**actor x **O**ffset **R**einforcement x **C**hracteristic **E**ligibility)
[REINFORCE]: https://link.springer.com/article/10.1007/BF00992696
- A critique on SAC algorithms from Willem was the oscillatory nature, yet in his research he found that his TD3 also exhibited oscillations in its actions and even states.
- Reinforcement learning in general is about finding the **optimal** policy or value functions that maximizes the expected discounted return.

> [!Definition]+
> **Estimator windup**
>  When the covariances of the estimator in an RLS increases exponentially due to e.g. lack of system excitation. This can be overcome by setting the forgetting factor as 1.

> [!Definition]+
> **Options**
>  The set of all primitive and temporally extended actions that an agent can take, often used in the context of semi-MDP and MDPs.

### 10/1/2024

- *Actor-Critic* methods learn both a value function and the policy function simultaneously, as opposed to policy-gradient methods which only learn policy, or with dynamic programming methods which only learn the value function.
- *Generalised policy iteration* (GPI) is a dynamic programming approach to finding an optimal policy and the true value function "simultaneously", which is achieved by alternating between performing *policy evaluation* and *policy improvement*. Policy evaluation is done to improve the approximation of the value function, which is done by simply computing the bellman equation for value function, Eq 2.10 from Willem. Policy improvement is done to improve the policy itself, which is done by setting the policy to be equal to the action (which remember is a probability distribution!) which has the highest expected return, Eq 2.13 from Willem. GPI is a useful theoretical foundation for more advanced techniques such as *heuristic dynamic programming* (HDP), but GPI in its pure analytical form is too computationally expensive to compute/reach convergence especially for as complex a MDP as flight control, and require complete models of transition probabilities (i.e. the complete dynamics of the MDP). 
- iADP was first proposed by Ye Zhou, Erik jan, and Qi Ping 
- *Persistent excitation ([PE])*: during GPI, ADP and iADP require using PE to enable environment exploration. PE is done by adding disturbance (e.g. white noise) to the action values to ensure that the system states are excited at all times, this comes at the cost that the converged policies are not optimal. Fortunately, PE is only performed during the training stages of the algorithms, e.g. an iADP controller is trained with PE included to arrive at a set of incremental models $F$ & $G$, where after these models can be used for actual flight with no PE included in the iADP controller.
  - **note**: the concept of PE originates in system identification, where the input signal with a PSD $S_u$ to a system needs to satisfy the *persistently exciting in order n* property to ensure that it is possible to converge to a set of system parameters, i.e. st $DG*S_u = 0$ can only imply DG=0, not $S_u$ = 0. A corollary of this definition is that a white noise input signal is persistently exciting of *[all orders]*, which means a white noise input signal is sufficient for identifying model parameters for models of any order.
  

> [!Definition]+
> **Sample complexity**
>  It describes how many examples are required to guarantee a *probably approximately correct* (PAC) solution, and is a function of accuracy and confidence. Note that this is the definition in the field of machine learning, in the field of reinforcement learning it has been used as: **number of time steps (during exploration) in which the algorithm does not select near-optimal actions** by Sutton and Barto in their famous reinforcement learning book.
>  
>  A [discussion] on stack exchange about sample complexity vs sample efficiency.

> [!Definition]+
> **Probably approximately correct (PAC)**
> A hypothesis can be called PAC learnable for a chosen accuracy and confidence parameter, with accuracy defining how far the classifier output can be from the true value, and confidence defining how likely the class is to meet this given accuracy.

[discussion]: https://ai.stackexchange.com/questions/38775/do-the-terms-sample-complexity-and-sample-efficiency-mean-the-same-thing-in
[all orders]: https://youtu.be/IDaCw3LjzC4?si=b9FxzgtakMqPjSfD&t=1668
[PE]: https://repository.tudelft.nl/islandora/object/uuid%3A5b875915-2518-4ec8-a1a0-07ad057edab4

### 11/1/2024

- According to Zhou, HRL is for high-level guidance and navigation, would be an intereesting angle to look at the flying v control system, and she even further devloped HRL into a new algorithm coined hybrid HRL. But Jonathan hoogvliet might have already done his thesis on this, so now i should review his thesis.
- Both [Zhou] and [Jonathon] introduce their own version of a "hybrid" hierarchical reinforcement learning algorithm. For Zhou, the hybrid part of the algorithm comes in how lower level policies are trained/executed (need to confirm) by continuous RL algorithms e.g. SAC, and higher level policies are trained/executed (need to confirm) by discrete RL algos e.g. Q-learning. For jonathon, the hybridness comes in the form of a discrete HRL agent with temporally-abstract actions (actions that execute for several timesteps) in addition to being able to invoke a PD controller.
- Meaning of *if and only if* or *neccessary and sufficient*
  - A statement "***p*** is sufficient for ***q***" means that if ***p*** is true, then ***q*** is true. But gives **no** indication on the truth of ***p*** if ***q*** is true. i.e. ***p*** => ***q***
  - A statement "***p*** is neccessary for ***q***" means that if ***q*** is true, then ***p*** is true. But if ***p*** is true, then the statement gives no indication on if ***q*** is true. i.e. ***q*** => ***p***
  - Therefore, a statement "***p*** is neccessary and sufficient for ***q***" means that if ***p*** is true, then ***q*** is also true, and if ***q*** is true, then so is ***p***. i.e. ***p*** <=> ***q***

[Jonathon]: https://repository.tudelft.nl/islandora/object/uuid%3Ad66efdb7-d7c7-4c44-9b50-64678ffdf60d
[Zhou]: https://repository.tudelft.nl/islandora/object/uuid%3A5b875915-2518-4ec8-a1a0-07ad057edab4

> [!Definition]+
> **Upper confidence bound (UCB)**
> A type of action selection policy (in short a policy) which allows for exploration instead of solely exploitation, similar to epsilon-greedy, it bases action selection on uncertainty of an action's value by increasing the value of actions which are least selected. It is a heuristic for introducing an upper bound on the value of an action. UCB is hard to extend to general RL problems. Maybe could develop ways to generalize UCB.

### 12/1/2024

- Added more notes to the Sutton and Barto item in 1st_read_notes.md

### 13/1/2024

- Reading ch 5 and added more notes to the Sutton and Barto item in 1st_read_notes.md
- There are quite a lot of coding exercises that are on this book, i would like to dedicate about 2 days to doing these exercises;
	- Example 6.5, windy gridworld
	- Example 6.6, cliff walking

### 14/1/2024

- Reading ch 6 and added more notes to the Sutton and Barto item in 1st_read_notes.md

### 15/1/2024

- Reading ch 5 and added more notes to the Sutton and Barto item in 1st_read_notes.md
- Implemented SARSA, Q-learning, and expected SARSA and solved the windy grid world problem from example 6.5
- TD and Monte Carlo methods are two basic frameworks for estimating value functions in an MDP, the only difference lies in what number is used to estimate the value function. In the TD case the value function is estimated using **rewards** that are sampled, while in MC methods **returns** are sampled to estimate the value functions. And in the former, bootstrapping operations are essentially the only operations used to update estimates, whilst MC methods can use either bootstrapping operations or a sample average to obtain the value function estimates. Both methods are free to work with either state-value functions, or action-value functions, or even *after state*-value functions which is a type of value function observed in some classes of MDP's.

## 16/1/2024

- Finished reading Part I of Sutton & Barto, skimmed through chapters 9 and 10 in Part II.
- In the case of using function approximators for the value functions. Suppose i am enacting a greedy policy and selecting whichever action has the highest action-value according to my function estimate, how am i to see which action has the highest action-value? Do i have to iterate through all actions, find the action-value for each, and then choose the action with the highest action-value as my policy action?