# Master Notes


File containing all miscellaneous/thinking-out-loud thoughts throughout my thesis.

## Table of Contents

1. [List of TODOs](#todo)
2. [Research Objective](#obj)
3. [Daily Notes](#daily)

---

## List of TODOs <a name="todo"></a>

1. PDF changes to make:
	1. Make sure that variable + unit convention is followed:
		1. For units, write in normal text not math mode
		2. Use abbreviated 
	2. use s to represent states of the MDP/environment, x to represent the states of the model which is inside the environment.
	3. Change order of equation and variable explanation, explanation after equation!!!
	4. remove all mention of flying v
	5. Add SGD or nn weight optimizer section in ls?
	7. Cite elias for cover picture
2. Change logs for future idhp:
	1. Change the adaptivity rule to be triggered based on mse of past 50 time steps
	2. Change critic output to be derivative of value function wrt mdp states, instead of wrt model states.
3. Decisions:
	1. Nonlinear or linear?
		1.  probably just linear? Huge asym + sym ss model to get p, q, r models? And then tracking p, q, r?
	2. What reference signals? 
		1. Sine, 321, saw tooth, random step?
	3. What fault scenarios?
		1. Shifted cg, damaged elevator, wing icing, stuck aileron/rudder/elevator?
	4. How to make *traditional* robust controller??
		1. LQR state feedback, mixed sensitivity H_inf?
	5. Controller structure
		1. Cascaded (q control, and then alpha control)

---

## Research Objective <a name="obj"></a>

> ~~*To advance the state-of-the-art RL based fault tolerant flight controllers and contribute to the safety of autonomous flight, by studying and developing novel methods of hybrid RL algorithms.*~~

### **Research Objective**

>~~*To advance the state-of-the-art RL based fault tolerant flight controllers and further the technological readiness level of the Flying-V, by developing a reinforcement learning based intelligent flight control system for the Flying-V.*~~

> ~~*To advance the state-of-the-art RL based fault tolerant flight controllers and further the technological readiness level of the Flying-V, by developing a reinforcement learning based intelligent flight control system that can stabilize and improve the handling qualities of the Flying-V.*~~

> ~~*To advance the state-of-the-art reinforcement learning based flight controllers and further the technological readiness level of the Flying-V, by developing an intelligent and fault tolerant flight control system for the Flying-V.*~~

> ~~To advance the state-of-the-art reinforcement learning based flight controllers and improve their fault tolerance, by researching sample efficiency improving augmentations to reinforcement learning algorithms.~~

> To advance the state-of-the-art reinforcement learning based flight controllers and improve their fault tolerance, by researching novel augmentations to reinforcement learning algorithms.

The online-offline hybrid approach to applying RL to flight controllers seems the most promising solution to tackle the challenge of fault tolerance. The "offline" algorithms come in the form of model free actor critic methods, which while being relatively sample inefficient to the "online" algorithms, are firstly able to generalize to more control tasks as they through learn policies and value function estimates by repeated traversals across the state-action space, and secondly can provide an initialization condition for the "online" algorithms which have been pre-trained and tested rather than from tabular rasa, otherwise requiring online system identification and inconvenient persistent excitations.

The research plan will set a goal to implement and evaluate such an online-offline hybrid controller for attitude control of the Flying-V. At this stage it is not possible to tell whether this goal is too achievable or overly ambitious, but to be conservative keeping in mind time constraints it will be assumed that this goal is ambitious. Therefore, contingencies will be planned in the form of placing intermediate sub-goals between commencing the first research phase and achieving the final goal. The first of the sub-goals would be to implement an augmented IDHP algorithm, this implementation is prioritized as this incremental model-based algorithm is considered to be highly adaptive and can thus provide truly online control, which will have better odds of controlling the aircraft in face of sudden changes in system dynamics as a result of faults; achieving this sub-goal would yield the academic contribution of this MSc research, by contributing towards the development of the IDHP algorithm. The second sub-goal would be to implement the "offline" learning algorithms, which can take the form of as basic as SAC, to DSAC, or even the state of the art RUN-DSAC algorithm; achieving this sub-goal will yield the societal contribution of this research, as an offline-online hybrid algorithm has much improved flight performance and fault tolerance characteristics than a purely online trained controller.

To achieve this goal, the following research questions are posed.

%% 
- Add sub-question to define fault tolerance in q1
- 1c change "demonstrate" to "what has shown", because then you will be implementing and testing a lot of algorithms.
- 1d also change the same basically
- 2, add nominal challenges as well not only faulty challenge
- remove 4a handling qualities
- non-minimum phase sounds challenging for rls
%%

### **Research Questions**

1. What reinforcement learning algorithm should be studied in the present research?
	1. What RL algorithms are considered to be state-of-the-art? 
	2. How is fault tolerance defined and tested in past research?
	3. Which algorithms have been shown to provide the best fault tolerance?
	4. What reference tracking/control performance have these algorithms shown in past research?
	5. What promising augmentations to reinforcement learning algorithms can be made and experimented with?
2. How can the proposed algorithm be applied to control the PH-LAB research aircraft?
	1. How can the identified augmentations be integrated to the reinforcement learning algorithm studied?
	2. How should the flight control system be structured (cascaded control structure, what signals to feedback)?
	3. What are the variables defining the MDP in the case of controlling the PH-LAB (the state variables, action variables, environment to be controlled)?
3. How does the implemented flight controller perform during nominal flight and in the presence of faults?
	1. What flight scenarios should be designed to test the performance and fault tolerance of the controller (what faults to use, what tracking signals to use, superimposed faults...)?
	2. What are the nominal performance and fault tolerance behaviour of the implemented controller?
	3. How do the proposed augmentations affect the flight control characteristics of the reinforcement learning algorithm?
	4. How well does the nominal performance and fault tolerance of the proposed flight controller compare to a traditional PID controller?

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
- rough meeting notes from yesterday:
	- liguo sun @BUAA - working on incremental ADP methods outside delft
	- ferrari and stengel adp controller for business jet
	- enns + si helicopter apache adp type methods
	- contact dirench for indi flying v controller for potential comparison

### 25/1/2024

#### Model based algorithms notes:
- Model based algorithms can have several pros and cons, 
  **Pros**:
  1. A model of an MDP is easy to learn in discrete state-action cases.
  2. Improves sample efficiency, as the agent is reconstructing a picture of the world (the model of the MDP) with the experiences it is learning (state transitions) which it then uses to improve its' understanding of the environment (improved value estimates).
**Cons**:
1. Wastage of compute on irrelevant details, not all state transitions are necessarily meaningful to learn or keep track of, in addition it could introduce information that provides no help in moving the agent towards a target.
2. Continuous state-action spaces do not lend for easy model estimation
3. Computing a policy from the model, especially in continuous state-action spaces, is non-trivial and in tractable cases are often expensive to compute.
- The Dyna algorithm is a model-based reinforcement learning framework, when you design an algorithm which uses a learnt model to gain more experience from the MDP, which is done by sampling state transitions from your learnt model. This framework can be tweaked in several ways:
	1. First way is in what kind of model you choose to learn, this model can be as simple as keeping track of all trajectories, wherein an experience sample would be done by taking a state and action tuple as input and then choosing an experience from memory which also had these state and action, and then returning as output the subsequent state and reward that resulted from this experience.  **This kind of a model is a non-parametric model**, where you can only query state and actions the agent has experienced and the retrieve the state and rewards that resulted from that combination, sampling from such models is known as **experience replay**.
	   
	   And then a more complicated version of the model could be a Bayesian model, where probability distributions that state the likelihood of observing a subsequent state and reward given a state and action tuple. Here the complexity of the model can be further refined by the method used to identify this probability distribution, which could be done by minimizing the KL divergence between observed data and estimated distribution, or by sample counting which would give a maximum likelihood estimate of the distribution.
	   
	   Semantically ways to learn models are to learn the full MDP's dynamics, or to learn expected values, or to learn probability distributions.
	
   2. Second way is in the way that the model is used to generate simulated or synthetic experience. Here one way of generating experience is to sample the model by taking a previously experienced state and executed action in that state, and seeing what the model predicted to have been the state transitioned to and the resulting reward. Another way could be sampling a trajectory of experience, where instead of only retrieving the next immediate state and reward combination, further experience can be sampled from trajectory by seeing what actions have been taken in that subsequent state, and then using this state action tuple to again sample another state and reward transition.
      
      Alternatively, any sampling methods used can be repeated for any number of times, or more succinctly called planning steps. Where taking 1 sampled experience or trajectory would be taking 1 planning step, and taking 4 sampled experiences or trajectories would be called taking 4 planning steps.


### 26/1/2024

- Deepmind has published researches that i would classify as pushes towards generalized artificial intelligence. Taking the example of FastRL, this framework seeks to come up with ways that will allow for learned policies on several tasks to be combined together into a *generalized* policy, which can direct the agent to performing combinations of tasks or to specifically avoid performing tasks.

### 27/1/2024

- Deeper study into using MsHDP in thesis, how can this extend to Ms***D***HP? Does it even make sense to do so?
  
### 28/1/2024

- Continuing study from yesterday, it could make sense to extend to MsDHP, sources confirm that DHP generally has better control performance (e.g. tracking error) and learning performance (e.g. sample efficiency or iterations to converge) than HDP.
- Idea of MsDHP can even be extended to IDHP, initially there was some headache with seeing how i could port the algorithms from MsHDP directly to IDHP. But on further reflection, it is easier and more sensible to derive from the IDHP algorithm a multi-step extension of IDHP, which I think I have accomplished.

### 29/1/2024

- Encoding the states of an environment into a different representation through coarse encoding, then it is possible to give more authority to a function approximator to provide better value estimates. This is done in the hopes that blowing up the state space representation of any state values into a *feature based* representation might be rich enough that even a linear function approximator suffices.
- Using function approximators in general removes the Markovian property of any MDP, and the a degree of partial unobservability is introduced.
- Action-value approximation can use a state and action dependent feature vector, or it can use a state dependent feature vector and an action dependent parameter vector. The former is called an *action-in* approach while the latter is *action-out*.
- How to avoid deadly triad? Can try to incorporate eligibility traces and tune the $\lambda$ parameter, as it is sometimes the case that there exists a set of values of $\lambda$ for which the triad is not deadly. Or we can consider using a different type of loss and change the parameter update rules, such as using:
	- Residual Bellman, loss = $\mathbb{E}[\delta_t^2]$, update = $\Delta w_t  = \alpha \delta_t\Delta [v_w(S_t) - \gamma v_w(S_{t+1})]$ (generally this works poorly apparently)
	- Bellman error, loss = $\mathbb{E}[\delta_t]^2$, update = $\Delta w_t  = \alpha \delta_t\Delta [v_w(S_t) - \gamma v_w(S'_{t+1})]$ 

### 30/1/2024

- I've come to realise i do not need to put in explicit references to the specific algorithm or frame-work i end up using during my thesis, or which i've come to the conclusion seems most appropriate for my current set of research questions. Because doing so would be **a)** putting the answers of the questions inside the question, and **b)** would probably involve re-drafting some of the research questions to flesh out the algorithm better, which would in some sense make all the literature review done thus far meaningless as they have been devoted to arriving at the framework building these new questions in the first place.
- What is the eligibility trace?
- found really nice [summary] of sutton & barto.
- The supposed dichotomy of offline and online algorithms posed by Casper and Lucas is a pseudo dichotomy, as the difference in their algorithm is falsely attributed to the nature of how each one adapts its' parameters. Both algorithms work online, where parameters of both algorithms change as an agent steps through an environment. The real difference is firstly in how sample efficient they are, as "online" algorithms simply have much better sample efficiency than "offline" algorithms. Benefits of "online" algorithms are that they can adapt very quickly to model changes, a disadvantage is estimator windup when the system is not excited persistently, and can suffer in performance from biased and noisy sensor measurements. Also, cite [killian's paper] for suggestions to use an online adaptive controller to augment SAC.

[summary]: https://lcalem.github.io/blog/2019/02/25/sutton-chap12#121-the-lambda-return
[killian's paper]: https://doi-org.tudelft.idm.oclc.org/10.2514/6.2022-2078



### 31/1/2024

- What is TD learning? In layman terms, it is to learning a guess from a guess. It directly uses previous estimates of any variable to edge the estimation closer to the true value.
- **Target** is to be thought of as the value that an estimate should converge towards. In MC methods, an estimated value function is updated towards its' target which is the sampled return. In TD methods, the target is still a return but in this case it is made up of the reward observed over one step plus the estimated return thereafter, thus the TD target is composed of observed and estimated environmental variables. This split of the target into an observed and estimated half is what is known as bootstrapping, it is using the algorithms' previous estimate of a value to improve on the estimation.
- To be online is to update estimates throughout an episode, instead of only at the end of one. 
- **Gradient methods** describes the subset of algorithms using function approximators for any of its' estimates which uses gradient methods to update the function approximators.
- **Projected Bellman Error** $\overline{PBE}$,  

### 1/2/2024


- IDHP gains much higher adaptability over its' non incremental counterpart through the use of online RLS identified system models.
- Adaptive dynamic programming methods have been the method of choice for RL-based adaptive controllers, for example [Zhang C et al] applied DHP with an online identified system model using just-in-time learning (JITL) to create an adaptive controller capable of adapting parameters in fractions of one episode. JITL is interesting to note since the end result is also a time varying LTI system, except to identify this LTI an ARX model is used. ADP in the form of Policy iteration is applied by [Zhang K et al] to tackle the challenge of fault tolerant control in the face of actuator failure for a purely mathematical nonlinear system. The spectrum between PI and Value iteration was also explored through the works of [Luo B et al] who combined the two ideas into a multi-step HDP algorithm that allows for switching between PI and VI; this method is subsequently extended to an algorithm which adaptively changes between PI and VI to balance the fruits of faster convergence under PI and larger pool of admissible policies under VI, and is applied to a power generator system to demonstrate its' online learning ability through adaptation of parameters within fractions of one episode.
- These examples are contrasted against DRL and model-free algorithms, which produces lower sample efficiency and hence online adaptability due to the larger number of parameters in a DRL and the lack of information on system dynamics from being model-free. For example, the works of [Killian Dally] shows that in the order of $10^5$ time steps each with a duration of $0.01s$ are required before SAC can converge to a usable policy, and [van Hasselt] published results of the Rainbow DQN algorithm which required number of time steps in the order of $10^7$ for convergence to a stable and performant policy. 

[Zhang C et al]: https://doi-org.tudelft.idm.oclc.org/10.1002/oca.2791
[Zhang K et al]: https://doi-org.tudelft.idm.oclc.org/10.1016/j.jfranklin.2018.07.009
[Luo B et al]: http://dx.doi.org/10.1016/j.ins.2017.05.005
[Killian Dally]: https://arxiv.org/abs/2202.09262
[van Hasselt]: https://arxiv.org/abs/1710.02298

### 2/2/2024

- cite werbos for beginning of adp.
- drafting the literature study
- deriving the equations for multi-step idhp

### 3/2/2024

- Estimating state-value functions is more associated with the task of *prediction*, these estimates can be used to predict how useful it is to be at a certain state. While estimating action-value function is more associated with the task of *control*, where the estimates can be used to decide what actions have the most expected return, and thus which action should the agent be controlled to execute.


### 9/2/2024

- Question, in the IDHP critic update equation, why is the error term $e_c(t)$ defined as 
- Interesting research direction could also be to compare IDGP and IGDHP, [Prokhorov and Wunsch] claim that DHP is usually sufficient enough of an improvement over HDP and thus use of GDHP is usually not warranted. Also would be interesting to investigate augmenting IDHP with multi-step or eligibility trace. And frankly these would make for very interesting research objectives and i am slightly inclined to make my MSc research objective be focused on these. However, i am also interested in trying to come up with fault tolerant controllers and see an autopilot be able to fly a plane no matter what faults i impose on the aircraft, i have put it in my research objectives after all... If i proceed with the current fault tolerant line of research objective.  So yeah this is an interesting dilemma.
- [Trust region policy] methods try to improve the policy by maximizing a certain objective function, while subjecting the policy updates to a limit in their magnitude.


[Prokhorov and Wunsch]: https://ieeexplore.ieee.org/document/623201
[Trust region policy]: https://arxiv.org/pdf/1502.05477.pdf

### 11/2/2024

- The reward signal is sometimes called a cost-to-go function in ADP field such as by Zhou, or a utility function by jun hyeon.

### 12/2/2024

- Find out if AD versions of ADP are commonly used. So far it seems they are not so maybe there is a reason they aren't popular.

### 13/2/2024

- [this paper] describes policy iteration and value iteration as follows,
	- Policy iteration, an algorithm for solving the optimal control problem where an initial admissible control policy is chosen, the policy is then evaluated by computing its value function, then the policy is improved using this function, policy evaluation and improvement is done iteratively in that order until value function convergence.
	- Value iteration, an algorithm for solving the optimal control problem, where an initial value function estimate is chosen, a policy improvement (or rather definition) update is performed with this value function, then the value function is updated using this value function, policy improvement and evaluation is done iteratively in that order until value function convergence.
- In fact, reading the paper more carefully, i notice that their definition of value iteration and policy iteration and Sutton & Barto's definition of the same algorithms are identical. PI is when the value function estimate of a given policy is iterated ad nauseum until the estimate stops changing, and a policy improvement is performed on this estimate thereafter; while VI is when the value function estimate is only iterated once, and a policy improvement is done using this estimate thereafter.


### 15/2/2024

**Erik meeting notes:**
- idea of detecting faults and switching off multi step
- make what time step the terms are calculated more clear
- Add sub-question to define fault tolerance in q1
- 1c change "demonstrate" to "what has shown", because then you will be implementing and testing a lot of algorithms.
- 1d also change the same basically
- 2, add nominal challenges as well not only faulty challenge
- remove 4a handling qualities
- non-minimum phase sounds challenging for rls

- Understand this 2022 paper, and then highlight the cool stuff and send to erik, **done**
- Highlight the 2008 paper, **done**

### 16/2/2024

- working on literature study, almost finished writing on ADP/ACD section, only have GDHP section to write.

### 17/2/2024

- continue writing ls, 
- cite [ferrari] and [enns] for old studies into adp for flight control.
- don't forget to unify the drawing styles of figures.

[ferrari]: https://arc-aiaa-org.tudelft.idm.oclc.org/doi/10.2514/1.12597
[enns]: https://ieeexplore-ieee-org.tudelft.idm.oclc.org/stamp/stamp.jsp?tp=&arnumber=1215408

### 22/2/2024

- One version of research phase 1 plan is the provided one, second version is i do eligibility trace augmentation first, and then have multi-step IDHP/offline-online hybrid as the nice-to-have augmentation.
- What did Erik mean exactly with calculating the previous partial with the new information?

### 24/2/2024

- This paper: The evolution of jet airliners explained through the range equation, shows an interesting result where the range factor of latest airplane designs have increased 100% from 1970 to 2010.

### 28/2/2024

- For meeting!!
	- For normal linear systems, you can find system transfer functions in xyz ways. But what about when your modelled dynamics are in quaternions?
- question: Enns in his helicopter paper mentioned this thing called "zero-trim requirement". What is that?

![[./pictures/zero_trim_req.png]]
<figure>
<figcaption><b>Figure 2</b>, zero trim requirement?.</figcaption>
</figure>

### 2/3/2024

- For meeting
	- What is LOES in Jurian's scripts?
	- What is are these p,q,r stars? are they "non-dimensionalized" rates? (even though it looks like the units of the stars are in degrees)
	- Why is the trimmed aircraft not trimmed? Using the trimmed states does not make the airplane sit still
	- The RLS that was taught to me in the system id course taught by Coen and Daan is different than the RLS used for IDHP. I am wondering now if the RLS taught in that course is actually not RLS, but a "piece-wise" OLS which Zhou mentions in one of her papers.
	- ![[Pasted image 20240306140104.png]]
	- ![[Pasted image 20240306140839.png]]
	- ![[Pasted image 20240311114531.png]]
	- From what i understood, the only difference between IDHP and DHP is how the system model is identified, the actor and critic updates between these two algorithms are identical. Confirm with Erik and see if as far as he is aware, if my understanding is correct.
	- Setting RLS forgetting factor to 1.

### 4/3/2024

- Regarding what linear model to use, i have narrowed down to going with three options:
	- Linearize for short-period, find linear model between elevator <-> alpha and q. 
		- Pros: More logical to do reference following with such model as we can follow a pitch rate reference, very simple as only one input and two outputs, several past theses and research used this model.
		- Cons: Pitching motion is less troublesome on the flying-v than yawing motions so this model is theoretically less important than a yawing model,
	- Linearize for dutch-roll/yawing motion, find linear model between rudder <-> beta and r.
		- Pros: Directly models simplified Dutch roll which is unstable in the flying-v, also only has 1 input and two outputs.
		- Cons: logically can only do regularization task since you don't typically try to command a sideslip angle in cruise,

### 6/3/2024

- The RLS which is used in IDHP seems really cool, i want to checkout its derivation
- Flying-V meeting:
	- Elevator deflection up till ~15 is considered linear dynamics enough, beyond which would not be modelled well by VLM data.
	- Pitch break happens earlier the faster you go.
	- RANS is ran on full scale flying-V.

### 7/3/204

- Make different model matching for linear-nonlinear short period model match
- Burhan

### 10/3/2024

- realised that for the errors in IDHP, the time step from which different implementations take the reward value from differs, some do it in this way (junhyeon, zhou):
	$$e = V(t) + r_t + \gamma V(t+1)$$
	some do it this way (casper):
	$$e = V(t) + r_{t+1} + \gamma V(t+1)$$

### 11/3/2024

- Direnc explaining Jurian model meeting:
	- Algebraic loop, cool name, dunno what it is.
	- data_coupled, VLM_WTE_AeroData.HQ.json comes from simon ()
	- data, FV_reset.HQ.json is from airbus/odilila...

### 12/3/2024

- flying-v roelof meeting:
	- uncorrelated sine waves? Significance is....?
	- high frequency input has higher control effectiveness?
- Coding notes:
	- I know there's something wrong with the short-period model/how I'm interfacing with it since when I have a constant input of 20, which should mean 20 degrees elevator deflection, the alpha of the airplane stays at around 6 degrees.
	- And there's something wrong with RLS since the norm of epsilon is zero:
		- Tried
	- Possibly a bug in critic actor updates, since their weights make big jumps mid way through episode
- A sign for actor weight update in my IDHP pseudo code file is wrong? No idea why
- Text Erik to ask if he has Zhou's code/ parameters/ resources for her paper: Incremental Model Based Online Dual Heuristic Programming for Nonlinear Adaptive Control. Similar thing, text asking if he has any resources like progress meeting notes that documented how Casper encountered preliminary design of IDHP.

### 13/3/2024

- In RLS, whether i have the parameter updates start at 1sst or second timestep does not matter.
- Debugging report:
	- There was nothing wrong with the short period mode, i was simply using the wrong units for plotting. The elevator command is in degrees, but the states alpha and q were in radians.
	- Old RLS bug has been fixed, needed to correct variable updates in IDHP loop. **NEW** RLS bug arises, a) estimator is simply not really changing the parameters, b) the estimated F and G matrices breaks IDHP if i initialize & fix them as the true A and B matrices?
	- critic and actor updates have been verified to work, tried all possible combination of signs and double checked code with equations.
- **Create a pickling routine, where i pickle the hyperparameters of the algo, along with monte carlo results of this instance.**

### 15/3/2024

- Realised i did not initialize the networks correctly, i was not correctly varying the gaussian from which actor weights were chosen.
- Realized there was still some copy() bug, finally fixed it. Now can go ahead with meaningful experiments to the algorithm.

### 16/3/2024

- experiments:
	- Using state + error as info to networks
	- anneal the learning rates
	- Varying hidden layer size with everything else constant
- need to somehow:
	- gather frequencies of the hyperparameter set
	- gather episode time of the set
	- gather return of the set
- Refactoring tasks:
	- Remove hard coding, e.g. dont hard code state dimensions, action dimensions...
	- Revisit config dicts
	- Add assertion checks for array shapes!
	- Revisit function, variable, class names
	- Add doc strings
	- Revisit folder/file structures

### 17/3/2024

- trying 3 types of network inputs:
	- error: networks converge fastest, small variance in performances, and tracking performance is ok
	- states plus error: networks frequently diverge, when converged tracking performance ranges from good to oscillatory
	- states plus ref: 
	- tracked state plus ref state?
- Need to do hparam tests for high low eta with high eta super high (500+-)


### 19/3/2024

- transiency metric? Rate/speed of convergence metric?

### 25/3/2024

- Try using more reference signals!!! Need to see that observations on just the sinusoidal task generalize!

### 26/3/2024

- Switched neural network evaluation from tensorflow to manual, so instead of simply passing a network input to a tensorflow network function, the network input is pass through each network layer manually. This lead to an increase in compute time, with pure tensorflow python could compute approximately 100 time steps per second, the manual implementation can compute approximately 80 time steps per second.

### 27/3/2024
- for hyperparams, need to print the entire x array fammmmm pleasssssuh, otherwise can't do any cool data evaluation uh.....
- run some fault case tests
	- Eligibility traces basically do not improve convergence property of IDHP post fault, most likely hyperparameters were over fitted to the initialization phase.
- run some mc tests of with and without eligibility traces.


### 29/03/2024

- lambda = 0 is identical to if i just use tensorflow gradient tapes for critic and actor updates.
- For some reason, looks like the actor networks are usually initialized to be as positive gains? When it should be a more even spread of positive and negative gains
	- TURNS OUT, it is because the weights in first and second layer are IDENTICAL!?!? If they weren't then there would be  more 
	- Spend an hour and try to make my own initializer? this issue is kind of annoying
	- Current IDHP cannot flip the sign of the actor policy easily, it cannot go from positive to negative gain quick enough.
- IDHP is on-policy btw.

### 30/3/2024

- turns out my implementation of auto-diff was wrong, the old auto-diff for y=tanh(x) actually had dy = 1-x^2 instead of dy = 1-y^2, whoops. But even with this erroneous auto-diff, eligibility traces for some reason still brought IDHP back to a stable controller.
- python logging module seems useful

### 1/4/2024

- people often refer to model free reinforcement learning when the algorithm do not need a model to be initialized for the agent to learn.

### 2/4/2024

- changing reward scaling (kappa) seems to have big effect on idhp performance.
- maybe use low pass filters for the eligibility traces??
- Current state: IDHP vanilla is fault tolerant for citation though not to degree of stefan or casper, but is not fault tolerant for flying v. When vanilla eligibility traces are used, idhp weights diverge, though divergence can be solved by bounding the elements of the eligibility trace between -1 and 1; with this solution the fault tolerance of eligibility IDHP is found to be.
- Before anything, need to see if i can tune vanilla IDHP to fly the flying v under the inversion fault scenario, after i find a config that can do this, i try to make eligibility IDHP fly the faulty flying-v.

### 3/4/2024

- steps:
	1. finish running some more hparam for flying v
	2. do some manual tuning for flying v w kappa = 100, eta_c_h = 0.4, sweeping eta_a
	3. do some quick hparam tuning for citation with eligibility, some things to try:
		1. Try doing actor only elig
		2. Try doing bounded elig
	5. run 12 hour hparams for citation with the most successful elig setting from step 3.
	6. After this, it should be late thursday, implement replacing elig traces and repeat step 3.
	7. After that, run gridded hparam tests for vanilla idhp on citation, keep in mind to vary kappa! Thats what we want to investigate the efffect of!

### 5/4/2024

- hyperparameters of the IDHP are tuned to maximize the total return of an episode while using eligibility traces, and the traces are removed to demonstrate the effect which removing traces has on performance of the controller.

### 6/4/2024

- text spilios asking for quick 20 minutes meeting for help with making fault tolerant controller with making robust controller.
	- The course *from what i gathered* mainly taught robust control against disturbance or noise to maintain good reference tracking whilst minimizing control usage. But i want to see if i could come about with a controller robust against parameter uncertainties, or parameter changes in the system. I have heard that to tackle parameter uncertainties it is more traditional to use LFT's and model the uncertainties *explicitly*(?), however i want to probe how relevant mixed sens control would be for param uncertainty, or what it would take to try to obtain a controller robust against param uncertainty.
	- I'd like to chat with you so that i may know what attempting this task would entail, thus helping me decide if i should attempt it, and if so how i might go about solving it.
- idea for data representation: min max bounds for "nominal" idhp runs, and then transparent lines for the "outlier" unconverged idhp runs which oscillate a lot or went to min max elevator deflection.

### 9/4/2024

- interesting idea, what if we just keep solving an LQR, or perform pole placement, or some kind of linear controller synthesize routine for each time step using the system identified through the RLS model?

### 10/4/2024

- for each algorithm, find the best performing hparam of that algorithm under the inversion fault (but with low learning rate of critic eta_c_l equal to 0), and then use that as the best hparam for the given algorithm. Comparison of the different algorithms will be done by using this hparam.

### 16/4/2024

- write progress report summarize what happened in todays meeting
- look into shift cg, derive the correct equations for it and note it down in latex (in the progress report)
	- Use table 7-2 in FD reader for stability derivatives as a function of cg pos,
	- for the control derivatives, only the pitching moment derivative wrt elevator deflection is affected by shift in cg (vertical force as derivative wrt deflection unaffected, use the following equation to derive cmde as a function of cg position: ![[Pasted image 20240416192244.png]]
- implement correct shifted cg
- run shifted cg from start and middle and compare and note down observations on progress report

### 17/4/2024

- about data clustering. Could create clusters of samples in the hyperparameter space, and then colour the points in metric space according to these clusters!!
- When cg is shifted aft, divergence occurs even when gradient of critic set to 0

### 19/4/2024

- in the citation full-nonlinear model, try using average MSE over past time like in caspers paper, tune a little bit, and see if can get better results.
- tune the multistep idhp for the best shift_cg and damp elevator possible, and just take whatever inverted elevator we can get.

### 20/4/2024

- originally thought about doing the k-means clusters to show where in the metrics space do each hparam cluster lie in the inverted elevator fault and the shift cg fault, which would show (i think) that for the same hparam the controller performance is very different in these two scenarios. But now i think maybe that is not so relevant...


### 21/4/2024

- need to come up with a better convergence rate metric. With the current convergence metric of time for settling below 0.5 degrees error, this first controller has a better convergence rate than the second controller:
- 1st controller, 1 late converged controller 27.05 avg convergence time:
- ![[Pasted image 20240421225307.png]]
- ![[Pasted image 20240421225339.png]]
- 2nd controller, 9 unsteady convergence 36.25 avg convergence time:
- ![[Pasted image 20240421225552.png]]
- ![[Pasted image 20240421225603.png]]
- \k344_etaah4.351_etaal0.064_etach0.398_etacl0.0_lh0.848_ll0.249\config-8_d0_p0.1136_c-0.6281_t36.2187_uc9

### 1/5/2024

- the critic and actor update rules in pseudo code needs to be more explicit and correct.
- actor update in eligibility trace section needs to be made more clear.


### 4/5/2024

- the way i tuned hyperpaarmeters:
	- tune hyperparameter to find best shift cg fault tolerance behaviour 

### 5/5/2024

- The mdp flow diagram actually only needs to feed reward derivative from environment to agent, make this fix
![[Pasted image 20240505201607.png]]

### 6/5/2024

- trying 4 new midhp controller options, see if the lower etaal helps with convergence at end of episode!!

### 7/5/2024

- to measure settling time of different controllers, the original approach was to see when does the tracking error fall below x value. But this was a very poor criteria which resulted in metrics that did not reflect what was being shown in the time traces. More elaborate ways of measuring settling time were thought of (measuring "half life" of error by tracking time taken for error to go half way from max to min, fitting an exponential function and using the exponent as the decay rate ...). Ultimately, it was decided that these were simply too convoluted, and would make the results more difficult to interpret. Thus, a much simpler and straight forward approach from classical control is used: using settling time of a step reference tracking task.
- Tune idhpat, and then go home and start writing code to count how many runs diverged in the inverted case and record this number, then write code to clean the data and plot the upper lower max bounds for the four controllers.
- Divergence defined as if error did not settle below 0.5 degrees within 10 seconds of the fault
	- idhp diverged: 26
	- idhp(L) diverged: 98
	- midhp diverged: 52
	- midhp(L) diverged: 29
- Results:
	- Do nominal comparisons between vanilla and augmented (using same hyper parameters), show step response differences between the four algos. Here you can talk about convergence time (settle within +-0.25 deg of final error?) and convergence quality (difference between final error and reference value?).
	- Do faulty comparisons, here use the optimized hyperparameters. Start with the box plots, then can present some of the state action graphs
- How is mean absolute error calculated in the other papers?

### 9/5/2024

- write up the two experiment results first:
	- comparing the settling time and final error in exp 1
	- comparing sum tracking error in exp 2
- do the weight comparisons between 4 algorithms

### 10/5/2024

- the single subplots dont need to be a square (too big!)
- settling time boxplot data:

### 13/5/2024

- do final check and tweaks of objectives and put it into the thesis overleaf!!!!
- finish introduction
- finish conclusion for preliminary results

## 17/05/2024

- auto hparam work on later?
- mid term note
- write email to frank lewis

Thesis change list:

1. Add in results justifying the hparams and settings i used (number of neurons, MDP state selection, action selection, hyperparameters)
2. Add plots showing how the policy function looks like at each time step.
3. Add in conclusion, *definitive*.
4. Elaborate in introduction.
5. Flip the research objective
6. For the gradient figures
	1. increase label size DONE
	2. standardize scales DONE
7. Change boxplot scales to start from 0, dont leave misleading/difficult to comprehend plots!!! DONE
8. Go through all Erik comments and address them all.
9. Remove the "and" in objective!!!!
10. Fix "In answering research question xxx"