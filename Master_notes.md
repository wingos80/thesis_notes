# Master Notes

File containing all miscellaneous/thinking-out-loud thoughts throughout my thesis.

## Table of Contents

1. [List of TODOs](#todo)
2. [Research Objective](#obj)
3. [Daily Notes](#daily)

---

## List of TODOs <a name="todo"></a>

Ranked, 1 = highest priority

1. Read up on flying-V
2. See what curriculum RL is about
3. Read Ye Zhou's publications on iADP, IDHP
4. Learn how online trained rl agents are trained, do they just have the agent fly the airplane once and then declare it is finished training? In other words, what is the difference between an online and an offline RL algorithm?

---

## Research Objective

> ~~*To advance the state-of-the-art RL based fault tolerant flight controllers and contribute to the safety of autonomous flight, by studying and developing novel methods of hybrid RL algorithms.*~~

### **Research Objective**

> *To advance the state-of-the-art RL based fault tolerant flight controllers and further the technological readiness level of the Flying-V, by developing a reinforcement learning based intelligent flight control system for the Flying-V.*


### **Research Questions**

1. What reinforcement learning algorithm has most promise to the problem of fault tolerant flight control?
	1. What RL algorithms are used in state-of-the-art flight control? 
	2. What desirable qualities should an algorithm possess for the best fault tolerant controller?
	3. What preliminary comparison should be done to select the most promising algorithm?
2. What are the handling qualities or possible faults scenarios of the Flying-V that a controller should take into account?
3. How can the identified algorithm be applied to control the Flying-V?
	1. How are RL based flight controllers implemented?
	2. How will the algorithm interface with existing simulations of the Flying-V?
4. How does the implemented flight controller perform both in the presence of faults and in nominal flight?
	1. What criteria should be kept track of to characterize a controllers fault tolerance?
	2. Noting the possible fault scenarios and handling qualities of the Flying-V, what flight scenarios should be designed to test the performance of the controller?
	3. How well does the nominal flight performance of the proposed flight controller compare to other intelligent and traditional controllers?
	4. How are the fault tolerance characteristics of the proposed flight controller compared to other intelligent and traditional controllers?

---

## Daily Notes <a name="daily"></a>


### 4/1/2024

- I am slowly developing a framework for the work i need to carry out to tackle this thesis, and it comes in 3 parts:

    part 1, the flying v. I need to become an expert in relevant topics on the flying v
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

### 16/1/2024

- Finished reading Part I of Sutton & Barto, skimmed through chapters 9 and 10 in Part II.
- In the case of using function approximators for the value functions. Suppose i am enacting a greedy policy and selecting whichever action has the highest action-value according to my function estimate, how am i to see which action has the highest action-value? Do i have to iterate through all actions, find the action-value for each, and then choose the action with the highest action-value as my policy action? 
	- For discrete action spaces with small number of actions possible, this is what is done. For larger ones, i will have to find out.

### 17/1/2024

- Gathered a list of interesting literature related to the flying-v and flying wings which will catch me up to date with the development of the flying v, and the flight control challenges currently facing the aircraft as well as flying wings in general.

### 18/1/2024

- Potential paragraph that i can directly use in thesis:
	- Flying wing designs are inherently attractive design options due to the fact that it merges the key functions of lift generation and payload carrying into one single structure, the opposite case of the traditional tube-and-wing design that all modern passenger airlines adopt. This merger means that less material needs to be used to create the aircraft, since one structure serves two simultaneous purposes, thus the airplane can weigh less overall. Further weight savings come as a result of much more evenly matched weight and lift distribution across the aircraft, as the payload is no longer centered at mid-span but rather spread out across the wing span, thus the critical load junction along with structural reinforcements at wing root disappears. Additionally, the aerodynamics of the traditional design is slightly less efficient, since the tube fuselage does not generate any lift, and the joint section between fuselage and wing causes additional interference drag.  The Flying-V takes up this design philosophy in the form of two passenger tubes joint in a v configuration, embedded in a thick and heavily swept back set of wings.
	- However, the flight dynamics of flying wings are less favourable than the tube-and-wing design. Certain flight dynamic modes of the Flying-V are unstable, and piloting can feel sluggish as signified by the low damping ratios of some modes. One logical solution to this problem is the addition of automatic flight control systems, which can help with stability correction and control enhancements.
- The Flying-V *does not need high lift devices* due to a much larger wing surface area than traditional aircrafts, which reduces the complexity of the design.
- INDI requires a control input model of the aircraft, which is a potential disadvantage for INDI compared to using RL based controllers as INDI would be sensitive to changes between modelled and actual control effectiveness.
- Flying wings inherently have poor lateral handling qualities, due to lack of a vertical stabilizer. This means that they can be prone to sideslipping, or have poor yaw damping, and in the case of the Flying-V even an unstable Dutch roll mode.
- **Curriculum learning** when applied in the context of reinforcement learning is to train an agent for a simpler task, before training it on more and more difficult tasks, ultimately ending in training for the full task. Origins of curriculum learning as a formal methodology sprouted in the field of supervised learning with the works of [Bengio et al] in 2009. But has its roots in the informal idea that learning should follow a *curriculum*, which is to say learning should have a logical sequence of learning goals that build towards learning more abstract ideas; this is an idea that humanity in many forms adopt throughout its' long history, examples would be the primary school to high school path, or training of domestic animals to obey simple commands before higher level tasks.
- The idea of curriculum learning for online training of an adaptive controller seems very interesting. I think that the merits of adaptable controllers are one of the most attractive merits that RL possesses, and which makes it a standout option when compared to gain scheduled PID's. Where by using adaptable RL controllers, the engineering effort dedicated towards creating an entire array of controllers for each point in the flight envelop can be allocated towards other design efforts.

[Bengio et al]: https://dl-acm-org.tudelft.idm.oclc.org/doi/10.1145/1553374.1553380

### 19/1/2024

- It is very useful to compare performance of an RL based controller to a PID/traditional controller, make sure to do this in the thesis!
- I think reinforcement learning makes the most sense when the online version is used. With offline trained RL agents, the main advantages are:
	- the trained controller does not need to be obtained manually, it instead involves running an algorithm over many hours which means that controllers are essentially obtained automatically.
	- obtaining controllers also becomes a matter of having an effective enough RL algorithm, and having a representative model of the aircraft which you want to control
	- it is possible to train the controller in different flight conditions, and training could extend indefinitely until the controller can perform satisfactorily in *all* conditions (past research indicates satisfactory performance can be reached in tractable times)
	- performance and robustness can surpass traditional controllers
But an extra advantage that RL based controllers can have over traditional controllers is that it is possible for the controller parameters to change during flight, i.e. instead of having a one-size-fits-all controller, with one single controller being trained to handle all circumstances (or similarly training several controllers to handle different events just like gain scheduling). It would be in a control systems' best interest to have a controller that could adapt to ce equivalent would be to add to the fault-tolerance of an aircraft in addition to reducing pilot workload.
- An idea could be to use the hybrid format of Casper and Lucas, and then for the online training make use of SHERPA to make exploration online safer, or use curriculum learning just like Tijmen (could also only use SHERPA to action filter)
- I also have the idea to see if there exist distributional methods of online reinforcement learning, seems to have a [textbook] dedicated to distributional RL which has a chapter on incremental algorithms.

[textbook]: https://www.distributional-rl.org/


### 20/1/2024

- Should validate RL controller by comparing it with a PID controller tuned to ensure satisfaction of lateral handling qualities.
- [SAC gSDE] claims that it can reduce the jerky action signals that can result from deep reinforcement learning.
[SAC gSDE]: https://arxiv.org/pdf/2005.05719v2.pdf
%% - "By taking into account the Bellman optimality equation 12, it is possible to train a DQN in a reinforcement learning manner through the minimization of the mean squared error. The optimal expected Q value can be estimated within a training iteration $i$ based on a set of reference parameters $\theta$ calculated in a previous iteration$i'$ " from *a survey of deep learning techniques for autonomous driving* %%
- I am trying approach the literature review from the "supply side" like what Erik said, but I feel lost with this approach, i am not really sure what i am looking for, while i know that i want to research online methods of reinforcement learning, i cannot seem to find any useful articles.
- Journals which are reputable: IEEE transactions on intelligent transport systems, JMLR (journal of machine learning research)
- These are the state of the art RL algorithms i could find: SAC, TD3, PPO, IADP, IDHP, RAINBOW DQN,

### 21/1/2024

- plan for today:
	1. read up on zhou's papers about idhp and iadp
	2. do broad literature search to see what kind of online/adaptive reinforcement learning controller research has been taken.

### 22/1/2024

- "hedonistic machines"
- i should use the word "self play" and "tabular rasa" in my thesis heheh.

### 23/1/2024

- Value iteration is identical to Policy iteration with only a single change. Whereas in PI the policy evaluation iteration loop is continued for k number of iterations with k potentially being infinity, VI simply performs only one iteration of policy evaluation.
- Dynamic programming approaches seeks to find the optimal policy and its' corresponding value function through evaluating the bellman equations to iteratively approach the optimal value; this is an example of a model based approach. A model free approach instead uses experienced trajectories in an environment to iteratively approach the optimal policy and value function.
- A key to TD learning converging is that every step taken in an environment inevitably involves the dynamics and rewards which the environment has to offer, and then more steps we take, the more grounded in reality are the rewards and transitions that our agent observes.
- **interesting note**, greedy policy improvement using a state-value function estimate **is not model free**, as the updated policy in this case would be:
  $\pi'(s) = \underset{x}{\mathrm{argmax}}\,[R_a + p(s'|s, a)V(s')]$
  Which notably requires the dynamics of the MDP $p(s'|s,a)$ to be known.
  Whereas if one were to use the action-value functions to perform policy improvement, the policy update rule would instead simply be:
  $pi'(s) = \underset{x}{\mathrm{argmax}}\,q(s,a)$
  I.e. simply the action for which the corresponding q value is maximum. Thus by keeping track of an action-value estimate instead of a state-value estimate, it is possible to omit the need for any MDP model in policy improvement.
  The takeaway from this note is that to apply the idea of policy or value iteration to obtain model-free reinforcement learning algorithms, an immediate choice to make is to keep track of action-value estimates of an MDP instead of state-value estimates, as it would true model-free policy update rules.
  - **value vs policy based methods**, basically all value-based methods commands the agent to take the action which maximizes the estimated value functions, i.e. action = argmax of value estimate. This can be complicated to do when number of actions are high. Policy based methods can avod needing to evaluate an argmax operator. And for partially observed or uncertain MDPs, it is often better to use policy-based methods, which have a probability distribution over all actions. And stochastic policies can perform better than deterministic policies when *state aliasing* occurs.

### 24/1/2024

- learning from simulated experience instead of rea-world experience can result in very effective reinforcement learning algorithms.