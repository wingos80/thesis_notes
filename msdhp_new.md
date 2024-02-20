\documentclass[../report.tex]{subfiles}
\begin{document}
\chapter{Literature Study}
\label{ch:literature_study}

\newfontfamily\myfont{Times new roman}

The thesis project commences with a study of literature, this is intended to introduce the main concepts that will be used as the foundation of the methodologies used, as well as supplement the discussion of any results gathered. Literature studies are conducted in fields related to the research project, thus the first field to be surveyed will be that of Reinforcement Learning, and the second field to be surveyed is the developments of the Flying-V. 


\section{Reinforcement Learning Foundations}

The basic idea of reinforcement learning is to have some agent associate rewards with actions that help realize a goal and promote the agent to take more of such actions by asking it to maximize the reward. The agent is not told what the goal is explicitly, its' only interface with the world around it is through executing these actions, and observing that it has transitioned into some state and received some reward. Reinforcement learning can be thought of as a way in which theorists have attempted to codify and formulate algorithms for, the universal experience of learning through trial and error. Much like how a child learns to walk, or a dog learns to sit, it is through reinforcement learning that machines can learn how to perform tasks that might not be easily programmed. Through this codification, computer programs have been made that demonstrated impressive levels of learning; for example, programs can learn through reinforcement learning to play various high-dimensional board games to a level of expertise surpassing any living player \cite{alpha_zero}, and a robotic arm can learn hand dexterity and mimic the hand movements of a human \cite{OpenAI_dexterity}.

Understanding how the reinforcement learning algorithms work behind such examples and how they may be applied to flight control will require understanding the foundations first, thus this section will introduce the basic terminologies and ideas used to build these algorithms.


\subsection{Markov Decision Process}

The Markov decision process is the mathematical framework that is used to model sequential decision processes such as how an agent interacts with an environment, and it is what contextualizes all the ideas in reinforcement learning, for instance, the basic notion that an agent performs an action and receives a reward.

In such a framework, there exist two entities: the agent and the environment, and information flows from one entity to another to model making decisions and their resulting consequences. In reinforcement learning the agent is sometimes also called the learner, it selects an action $A_t$ which gets fed to the environment, and the environment will provide the corresponding state $S_{t+1}$ which the agent has transitioned to as a result of action $A_t$ and the reward associated to that state transition $R_{t+1}$, the agent can subsequently use $S_{t+1}$ and $R_{t+1}$ to decide on the next time step's action $A_{t+1}$. A graphical depiction of this agent-environment interface in the Markov decision process is shown in \autoref{fig:mdp}.

\begin{figure}[H]
    \centering
    \includegraphics[width=\textwidth]{figures/01/mdp.png}
    \caption{Flow diagram of the agent-environment interaction central to the Markov decision process}
    \label{fig:mdp}
\end{figure}

This time trace of an agent-environment interaction is recorded in a so-called \textit{trajectory} $\mathcal{T}$, which is a chain of state-action-reward for the entire duration of the decision process written in the form expressed in \autoref{eq:trajectory}.

\begin{equation} \label{eq:trajectory}
    \mathcal{T} = S_{0}, A_{0}, R_{1}, S_{1}, A_{1}, R_{2}, \dots, S_{T-1}, A_{T-1}, R_{T}, S_{T}, A_{T}
\end{equation}


One central component of Markov decision processes is the dynamics of the environment. In the simpler case of finite and discrete processes, the dynamics of the environment can be considered to be a discrete conditional probability distribution $p(s', r|s,a)$ as shown in \autoref{eq:mdp_dynamics}. which returns the probability of transitioning to a state $s'$ and obtaining a reward $r$ given that the agent observed a previous state $s$ and executed an action $a$.

\begin{equation} \label{eq:mdp_dynamics}
    p(s', r|s,a) \doteq Pr\{St=s', R_t=r|S_{t-1}=a,A_{t-1} =a\}
\end{equation}

From \autoref{eq:mdp_dynamics}, it is possible to compute the expected reward for any state-action pairs $r(s,a)$:

\begin{equation}
    r(s,a) \doteq \mathbb{E}\{R_t | S_{t-1}=s, A_{T-1}=a\} = \sum\limits_{r}r\sum\limits_{s'}p(s', r|s,a)
\end{equation}

Modeling the environment dynamics, also referred to as modeling the Markov decision process dynamics, can also be done using other modeling methods. For example, state space systems of equations are commonly used when creating control systems, and as such are also used to serve as the dynamics model in the Markov decision processes \cite{ss_example_1, ss_example_2, ss_example_3}. In both cases, one important property that the models possess is the \textit{Markov property}, also referred to as the memoryless property. This property states that to predict the system in the next time step, only information from the current or time step before is necessary, which means that having any information from previous time steps does not influence the outcome of the prediction. This property is captured in \autoref{eq:markov_property}.

\begin{equation} \label{eq:markov_property}
    Pr\{St, R_t|S_{t-1},A_{t-1}\} = Pr\{St, R_t|S_1, \dots, S_{t-1}, A_1, \dots, A_{t-1}\}
\end{equation}


In reinforcement learning, having the dynamics of the environment possess the Markov property is useful as many algorithms assume that the evolution of the system can be perfectly predicted by only using information from the current time step \cite{markov_property}, which should be sufficient for a learner to decide what actions to take in order to enter into a trajectory which maximizes its' rewards.

In the context of flight control, the Markov decision process can be conveniently formulated using state space systems. For example, the action, state, and reward in \autoref{fig:mdp} can be considered equivalent to the control vector, the state vector, and an output vector in the state space formulation respectively. 

\subsection{Environment}

\subsection{Rewards and Returns}

The way in which learners in a reinforcement learning problem gauge their performance is through reward signals, this reward signal is made to be representative of the goal of the reinforcement learning problem.

Reward signals are central to the \textit{reinforcement} in reinforcement learning, as they are made to be associated with states and actions that get the agent closer to the goal, thus incentivizing the learner to repeat more of such actions with the ultimate effect of the agent becoming more proficient in the posed task. Strong parallels can be drawn between an agent in the reinforcement learning context becoming better at obtaining higher reward signals, and that of animal behavior adapting to receive more desirable stimuli in the context of domestic animal training, a so-called "Law of Effect"\cite{law_of_effect}. This parallel arises from the strong connections between reinforcement learning as a computer science and mathematical theory, and reinforcement learning as a field of psychology, and shows how ideas in reinforcement learning are often grounded in ideas from nature. 

In reinforcement learning nomenclature, a distinction in terminology exists between the reward being received every time step, and the cumulative reward that an agent receives over many time steps. While rewards are received every time step, when they are summed up over time, they are then referred to as the \textit{return}. For example, an episodic return refers to how much cumulative reward an agent has received throughout an episode. Formally, returns are defined by \autoref{eq:return} as the sum of all future discounted rewards, where the discount factor $\gamma \in [0,1]$ is introduced which allows for returns to be computed even when a task is not episodic but continuing, i.e. $T=\infty$. Increasing $\gamma$ from 0 to 1 increases the weight of rewards received later in time, which makes the agent more ``far-sighted", the opposite case of decreasing $\gamma$ causes the agent to be more ``myopic".

\begin{equation}\label{eq:return}
    G_t \doteq R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{T-t-1}R_T = \sum\limits_{k=t+1}^T \gamma^{k-t-1} R_k
\end{equation}

The goal of a learner in reinforcement learning is to maximize the return $G_t$, this return can also serve as a convenient metric to evaluate the relative performance of agents.

\subsection{Policy}

A policy is what an agent follows to decide on what action to choose. Formally, a policy $\pi$ is defined as a functional mapping from state $s$ to the probability of choosing an action $a$, i.e. probability of action $a$ conditioned on state $s$:

\begin{equation}
    \pi(a|s) \doteq Pr\{A_t=a | S_t=s\}
\end{equation}

The specific formulation of $\pi(a|s)$ differs greatly from algorithm to algorithm, in fact, there is no restriction on the form of the probability distribution that $\pi(a|s)$ takes. For instance, it is permissible that $\pi(a|s)$ is deterministic, i.e. if the state is $s_1$, then action is $a_1$. The overarching theme is that an agent should follow a policy that will maximize its returns.

\subsection{Value Function}

Value functions predict how much return an agent will receive in expectation if it were to follow a policy $\pi$. Two types of value functions are commonly used in reinforcement learning, the state-value function $V_{\pi}(s)$ and the action-value function $Q_{\pi}(s, a)$. The state-value function is the expected return from being in any single state, mathematically this is defined as:

\begin{equation}\label{eq:state_value_function}
    v_{\pi}(s) \doteq \mathbb{E}_{\pi}\{G_t | S_t = s\}
\end{equation}

Where $V_{\pi}(s)$ is the value of state $s$, and $G_t$ is the return from time $t$ onwards when following the policy $\pi$. Notation of the expectation operator is slightly abused to include the $\pi$ to indicate that the agent is following policy $\pi$. The state-value function can be intuitively understood as how worthwhile it is to be in a certain state. The action-value function is defined as the expected return of a particular state-action pair, this can again be mathematically stated as follows:

\begin{equation}\label{eq:action_value_function}
    q_{\pi}(s, a) \doteq \mathbb{E}_{\pi}\{G_t | S_t = s, A_t = a\}
\end{equation}

Where $Q_{\pi}(s, a)$ is the value of taking action $a$ in state $s$, and $G_t$ is the return from time $t$ onwards when following the policy $\pi$. The action-value function can be intuitively understood as how worthwhile it is to take a certain action in a certain state.


\subsection{Bellman Equation}
The equation commonly referred to as \textit{the} Bellman equation is the Bellman equation for the state-value function, presented in \autoref{eq:state_bellman}, which is derived starting from the definition of the state-value function \autoref{eq:state_value_function}:

\begin{align*}
    v_{\pi}(s) &\doteq \mathbb{E}_{\pi}\{G_t | S_t = s\} \\
    &= \mathbb{E}_{\pi}\{R_{t+1} + \gamma G_{t+1} | S_t = s\}\\
    &= \sum\limits_{r, g_{t+1}}p(r,g_{t+1}|s)[r + \gamma g_{t+1}]\\
    &= \sum\limits_{a, s'}\sum\limits_{r, g_{t+1}}p(a, s', r, g_{t+1}|s)[r + \gamma g_{t+1}]\\
    &= \sum\limits_{a}\sum\limits_{s',r}\sum\limits_{g_{t+1}}\underbrace{p(a|s)}_{= \pi(a|s)}p(s',r|s,a)\underbrace{p(g_{t+1}|s', r, s, a)}_{= p(g_{t+1}|s') \text{, Markov}}[r + \gamma g_{t+1}]\\
    &= \sum\limits_{a}\pi(a|s)\sum\limits_{s',r}p(s',r|s,a)[r + \gamma \sum\limits_{g_{t+1}}p(g_{t+1}|s')g_{t+1}]\\
    &= \sum\limits_{a}\pi(a|s) \sum\limits_{s', r} p(s', r|s, a)[r + \gamma v_{\pi}(s')] \stepcounter{equation}\tag{\theequation}\label{eq:state_bellman}\\
\end{align*}

This equation defines an analytical relationship between the value function of successive states, suggesting that the value of a previous state can be calculated once the subsequent state's value function, the environment dynamics, and the followed policy are known.

Notice that the value function in \autoref{eq:state_bellman} is recursively defined. This recursive formulation lends itself readily to dynamic programming techniques, which simply means that \autoref{eq:state_bellman} is adopted as an update equation for estimates of the state value function, where by iteratively applying this update the estimate converges towards the true value. The Bellman equation of \autoref{eq:state_bellman} was first derived by Richard Bellman and is associated with the basic foundations of the field of dynamic programming \cite{dp}.

There also exists a Bellman equation for the action-value function, which is presented in \autoref{eq:action_bellman}.

\begin{equation}\label{eq:action_bellman}
    q_{\pi}(s,a) \doteq \sum\limits_{s', r} p(s', r|s, a)[r + \gamma v_{\pi}(s')] 
\end{equation}


\subsection{Basic Reinforcement Learning Algorithms}

The goal of reinforcement learning algorithms is to teach an agent to behave optimally in some Markov decision process. This is done by incrementally improving the value function estimate of a process, as well as the policy used to guide the agent. Three basic approaches can be identified to achieve this goal, these are \textit{Monte Carlo Learning}, \textit{Temporal Difference Learning}, and \textit{Dynamic Programming}. The first two of these approaches are described in this subsection, while the last of these approaches is elaborated in more depth under \autoref{sec:dp}.

\subsubsection{Monte Carlo Learning}
\ac{mc} learning uses the sampled experiences from an agent taking actions and transitioning states to improve the estimation of the value function. The \ac{mc} algorithm detailed here will be made to estimate the action-value function.

Here, an agent initialized with a complete but random table of action-values along with an arbitrary policy is placed in a Markov decision process and begins executing actions. With each action it takes, the environment transitions to a different state and the agent observes this new state along with the new reward. To allow the agent to learn, the action-value table is improved by summing up the rewards observed from each state-action pair that the agent comes across until this trajectory reaches a terminal state, and using this sum as a \textit{monte carlo} estimate of the return $G$ for the state action pair at the root of that trajectory.

\subsubsection{Temporal Difference Learning}
\ac{td} learning also uses sampled experiences to improve an agent's action and value estimates. But unlike \ac{mc} learning, it does not wait for the sum of rewards to reach a terminal state for a return's estimate to be defined, it updates the action-value of a state-action pair the instant which the agent receives the reward for that pair:

\begin{equation}\label{eq:td_bootstrapping}
    Q(S,A) = Q(S,A) + \alpha[R + \gamma Q(S',A') - Q(S,A)]
\end{equation}



\section{Dynamic Programming}\label{sec:dp}

One major field of reinforcement learning research focuses on the use of \textit{Dynamic Programming} techniques with an emphasis on tackling optimal control problems\cite{rl_and_dp}. Dynamic programming is a term coined by Richard Bellman, it has roots in the field of optimization and is used to refer to the problem-solving approach of divide and conquer, where a more complicated problem is broken into sub-problems for which recursive algorithms can be devised to come up with their solutions\cite{dp}. In the context of reinforcement learning, classical applications of dynamic programming revolve around the Bellman equation stated in \autoref{eq:state_bellman} and results in the method known as \textit{Policy Iteration}, which is used to obtain the optimal value function and policy for \textit{finite} Markov decision processes, processes whose state $s$ and action $a$ variables belong to countable sets denoted $\mathcal{S}$ and $\mathcal{A}$ respectively. This method will be described in \autoref{subsec:pi}. Modern applications of dynamic programming extend the method of policy iteration to make it more computationally tractable and is described in \autoref{subsec:adp}.

\subsection{Policy Iteration}\label{subsec:pi}

This method iteratively applies two steps to find the optimal value function and policy, these steps are \textit{policy evaluation} and \textit{policy improvement}. The method is graphically depicted in \autoref{fig:pi} can be outlined as follows:

\begin{enumerate}
    \item Initialize $\pi(a|s)$ and $V_{\pi'}(s)$ arbitrarily.
    \item Perform Policy Evaluation to compute the value function of $\pi$
    \item Perform Policy Improvement to find a more optimal policy 
    \item Repeat from Step 2 until $\pi$ and $v_{\pi}(s)$ have converged.
\end{enumerate}

\begin{figure}[H]
    \centering
    \includegraphics[width=0.3\textwidth]{figures/01/gpi.png}
    \caption{The method of policy iteration, by iteratively evaluation a policy's value function, and subsequently determining the greedy policy for that value function, the policy and value function eventually converges to a fixed iteration point when they are optimal for the given Markov decision process.}
    \label{fig:pi}
\end{figure}

This algorithm is proven to converge toward the optimal value function and policy\cite{intro_rl} but with some caveats, namely, it assumes environment knowledge and the curse of dimensionality which will be described further in this text. Nonetheless, the theoretical guarantee for optimality makes it an important foundation method in reinforcement learning and is detailed further. 


\subsubsection{Policy Evaluation}

For a given policy $\pi$, the value function of this policy $v_{\pi}$ is computed by starting with an initial estimate $v_{\pi, 0}$ and recursively applying the Bellman equation on all $s$ as an update rule as shown in \autoref{eq:policy_eval}. This update will be applied until the value function has converged, where convergence can be defined in several ways, the simplest of which is when the norm of the difference between subsequent value functions is smaller than a small constant $\epsilon$.

\begin{equation}\label{eq:policy_eval}
    v_{\pi, k+1}= \sum\limits_{a}\pi(a|s) \sum\limits_{s', r} p(s', r|s, a)[r + \gamma v_{\pi, k}(s')] \quad \forall s \in \mathcal{S}
\end{equation}

Note that to compute the update formulated in \autoref{eq:policy_eval}, the environment dynamics $p(s',r|s,a)$ needs to be known and defined, which is an assumption made during the policy evaluation step.

By inspection of \autoref{eq:state_bellman}, it can be seen that the update equation \autoref{eq:policy_eval} will only converge when the estimation $v_k$ is equal to the true value function $v_{\pi}$.

\subsubsection{Policy Improvement}

For any given value function $v_{\pi}$, an improved policy $\pi'$ is obtained by making $\pi'$ deterministic and greedy with respect to $v_{\pi}$. This is done by defining $\pi'$ to choose the action that has the highest action value in each state $s$:

{\myfont
\begin{equation}\label{eq:policy_improvement}
    \pi'(a|s) = \begin{cases}
    1,& \text{if } a = \underset{a}{\text{argmax}} \ q_{\pi}(s,a) \\
    0,              & \text{otherwise}
\end{cases}
\end{equation}
}

The policy improvement step is guaranteed to find a policy that is at least as good as the previous policy, and in the case of a finite Markov decision process, the policy improvement step is guaranteed to find the optimal policy \cite{dp}. After finding a greedy policy on the given value function, the previous value function will no longer be accurate for the new policy \textit{if it is suboptimal for the given Markov decision process}. This means that when a policy evaluation step is subsequently performed, a sub-optimal policy will cause the evaluation step to yield a different value function, but an optimal policy will yield the same value function. In other words, this algorithm only converges when the optimal policy and its corresponding value function are determined\cite{intro_rl}.

\subsection{Generalized Policy Iteration}

In the policy iteration method just described, the policy evaluation and improvement steps are carried out with the specific update rules in \autoref{eq:policy_eval} and \autoref{eq:policy_improvement}. However, the idea behind policy iteration: evaluating and improving upon some estimated policy or value function, is very useful and can be used to describe on a high level the idea behind many reinforcement learning algorithms as stated by Sutton and Barto \cite{intro_rl}, who they use the term \textit{generalized policy iteration} to capture this idea.

For example, instead of iterating the policy evaluation step until the value function converges, it is possible to only take one or a few iterations and proceed with policy improvement thereafter. This is the idea behind Value Iteration algorithms, which is applied to optimal control problems without using Bellman equations\cite{vi_for_optimal_control, vi_for_optimal_control_2}.

Another example comes from outside of the dynamic programming field, the class of actor-critic can be described using the idea of generalized policy iteration, wherein an estimate of the critic function (the value function) is improved based on sampled transitions from the Markov decision process, and the policy function (the actor) is improved using information from the critic.

\subsection{Approximate Dynamic Programming}\label{subsec:adp}
Finite Markov decision problems are characterized by their discrete and small number of states and actions that are possible, this makes it possible to use dynamic programming to solve for the optimal value function and policy of such a process. However, many systems that are interesting to model and find solutions for have many states and actions possible, and real-life processes frequently have continuous variables instead of discrete ones. Such circumstances can cause a combinatorial explosion in the number of state-action pairs that exist, which means having a specific value and a specific action probability distribution defined for each state becomes impractical. This is the so-called \textit{curse of dimensionality}, alluded to in \autoref{subsec:pi}. Moreover, evaluation of the Bellman equations also requires a known transition probability distribution of the environment. Not only does this also suffer from the curse of dimensionality in the case of large and/or continuous state spaces, but knowledge of such distribution might not be available for some systems, or at the very least inconvenient or difficult to identify. 

One solution to circumvent these issues is to use function approximators for the value function, policy function, and environment model, which leads to the class of dynamic programming based reinforcement learning algorithms called \ac{adp}. In this class, it is common to refer to value functions as \textit{cost-to-go} or \textit{cost} functions instead. Two basic methods exist in this class, the first is the \ac{pi} algorithm whose high-level algorithm is outlined in \autoref{alg:pi}.


\begin{algorithm}[H]
	\caption{Policy iteration, adapted from \cite{pi_and_vi_algorithm}} \label{alg:pi}
	\begin{algorithmic}[1]
        \State \textbf{Initialize}. Define a policy $\pi_0(x_k)$ which is admissible, i.e. stabilizing, and an arbitrary initial value function $V_0(x)$.
        \State \textbf{Policy Evaluation}. Determine the value function of the current policy using the Hamilton-Jacobi-Bellman Equation:
        \begin{equation}\label{eq:adp_poli_eval}
            V_{i+1}(x_t) = r(x_t, \pi_i(x_t)) + \gamma V_{i+1}(x_{t+1})
        \end{equation}
        \State \textbf{Policy Improvement}. Determine an improved policy. 
        {\myfont
        \begin{equation}\label{eq:adp_poli_improv}
            \pi_{i+1}(x_t) = \underset{\pi(x_t)}{\text{argmin}}[r(x_t, \pi_i(x_t)) + \gamma V_i(x_{t+1})]
        \end{equation}}
        \State If $V_i(x) \equiv V_{i-1}(x)$ then terminate, else $i = i+1$ and return to step $2$
	\end{algorithmic} 
\end{algorithm}

\autoref{alg:pi} fits into the mold of generalized policy iteration, which incorporates certain aspects of optimal control theory and has some note-worthy aspects. Firstly, the policy evaluation step \autoref{eq:policy_eval} of this algorithm is not formulated in the Markov decision process framework like in \autoref{eq:policy_eval}, and instead is the \ac{hjb} equation which is generally difficult to evaluate especially online\cite{frank_lewis_hjb} but can be solved approximately by methods such as least squares approximation using environment observations\cite{ls_approximation}, or by iteratively evaluating the equation until convergence\cite{pi_and_vi_algorithm}. Secondly, instead of the policy being a probability distribution, this algorithm generally uses deterministic policies. Thirdly, if the system dynamics can be formulated as \autoref{eq:normal_system_dynamics} with $n$ and $m$ being the number of state and control variables respectively, and the reward of the system is formulated in a quadratic manner as shown in \autoref{eq:lqr_cost}, then it is known that the policy improvement step takes the form of \autoref{eq:optimal_policy_improvement}\cite{bertsekas2012dynamic}. 

{\myfont
\begin{align*}
    x_{t+1} & = A(x_t) + g(x_t)u_t \stepcounter{equation}\tag{\theequation}\label{eq:normal_system_dynamics}\\
    r(x_t, u_t) &= x_t^{\top}Qx_t + u_t^{\top}Ru_t \stepcounter{equation}\tag{\theequation}\label{eq:lqr_cost}\\
    \text{Where} \quad \{x, f(x)\} \in \mathcal{R}^n, \; g(x) \in& \mathcal{R}^{n \times m}, \; u \in \mathcal{R}^m, \; Q \in \mathcal{R}^{n \times n}, \; R \in \mathcal{R}^{m \times m}
\end{align*}
}
\begin{equation}\label{eq:optimal_policy_improvement}
    \pi_{i+1}(x_t) = -\frac{\gamma}{2}R^{-1}g^{\top}(x_t)\frac{\partial V_i(x_{t+1})}{\partial x_{t+1}}
\end{equation}

The second basic algorithm of the \ac{adp} class is \ac{vi}, whose high-level algorithm is shown in \autoref{alg:vi}.


\begin{algorithm}[H]
	\caption{Value iteration, adapted from \cite{pi_and_vi_algorithm}} \label{alg:vi}
	\begin{algorithmic}[1]
        \State \textbf{Initialize}. Define an arbitrary policy $\pi_0(x_k)$, and an arbitrary initial value function $V_0(x)$.
        \State \textbf{Policy Evaluation}. Determine the value function of the current policy using the Hamilton-Jacobi-Bellman Equation:
        \begin{equation}\label{eq:adp_poli_eval_vi}
            V_{i+1}(x_t) = r(x_t, \pi_i(x_t)) + \gamma V_i(x_{t+1})
        \end{equation}
        \State \textbf{Policy Improvement}. Determine an improved policy. 
        {\myfont
        \begin{equation}
            \pi_{i+1}(x_t) = \underset{\pi(x_t)}{\text{argmin}}[r(x_t, \pi_i(x_t)) + \gamma V_i(x_{t+1})]
        \end{equation}}
        \State If $V_i(x) \equiv V_{i-1}(x)$ then terminate, else $i = i+1$ and return to step $2$
	\end{algorithmic} 
\end{algorithm}

Here the same method of policy evaluation and reduction of policy improvement in the \ac{lqr} case holds. The difference between \ac{pi} in \autoref{alg:pi} and \ac{vi} in \autoref{alg:vi} is that the policy evaluation step in \autoref{alg:vi} involves only one evaluation of \autoref{eq:adp_poli_eval_vi}, this is distinguished by the LHS value function having the subscript $i+1$ and the RHS having $i$. Furthermore, \ac{vi} do not require the initial policy to be admissible, which means that to use \ac{vi} algorithms it is not necessary to perform any a priori step on the control policy.

Various forms of these algorithms have been developed further and shown a convergence rate that could allow them to be applied to online control of systems. For instance, Wei et al. \cite{wei_adp} present an optimal control algorithm based on the framework of \ac{adp} using neural networks as function approximations. They demonstrate via simulation of an inverted pendulum control problem that even though their solution of the \ac{hjb} equation is only approximately optimal, by virtue of iteratively performing the police evaluation and improvement steps, their \ac{adp} method can converge to the optimal control law within 5 $s$ or 10 iteration steps. A similar method based on action-value or Q function is proposed by Lin et al. \cite{lin_adp_unknown_system} and demonstrates similar convergence speeds. 

By linearizing a nonlinear system's dynamics, it is possible to perform the policy evaluation and improvement steps in a much more efficient manner. This approach is adopted by zhou et al. \cite{zhou_iadp_1,zhou_iadp_2} who used \ac{rls} regression to identify a linear model at each time step, thus obtaining a time-varying linear model, which reduced the control problem to an \ac{lqr} like problem. Note that this approach relies on a sufficiently high sensor sampling rate, in the order of $10^2\ Hz$, which is needed for the assumption of linear dynamics to hold, as any smooth nonlinear function can be approximated locally by linear functions.

\subsubsection{Multi-step and Eligibility Trace Extensions of ADP Methods}

One trade-off that \ac{adp} methods have to face is between adopting a more policy iteration or a more value iteration approach. In the former approach, algorithms developed generally see faster convergence in their estimates, naturally a desirable characteristic. However, such algorithms require the initial policies to be admissible, otherwise, the control policy would drive the system towards instability, causing the \ac{hjb} equation can grow unbounded. This issue is not faced by the latter approach, as \ac{vi} algorithms do not seek to fully converge towards the optimal solution of the \ac{hjb} equation, instead \ac{vi} only require the value function estimation to be iterated by one step, instead of a large or infinite number of steps.

This binary situation can be reconciled by considering the idea of \textit{multi-step} iterations or updates, which is to take multiple transitions or timesteps worth of information while updating estimates in a reinforcement learning algorithm \cite{intro_rl}.

With the incorporation of a multi-step augmentation to the policy evaluation update rule, this dichotomy is turned into a spectrum of algorithms, where an \ac{adp} method can select how many steps should a policy evaluation iteration take. At the extreme of taking infinite steps, a multi-step \ac{adp} method is simply the \ac{pi} algorithm, on the other extreme, a single-step \ac{adp} method is the \ac{vi} algorithm \cite{mshdp_og}.

A high-level view of the multi-step \ac{adp} algorithm is shown in \autoref{alg:ms_adp}, with $n$ being the number of steps. To make the choice of $n$ automatic, Wang et al. \cite{mshdp_new} derived a criterion for switching $n$ from a low number during the initial iterations of the algorithm, where the initial arbitrary policy is not guaranteed to be admissible, to a high number during the latter iterations when the policy is verified to be admissible under the proposed criterion. 

\begin{algorithm}[H]
	\caption{Multi-step value iteration, known as multi-step heuristic dynamic programming in and adapted from \cite{mshdp_og}.} \label{alg:ms_adp}
	\begin{algorithmic}[1]
        \State \textbf{Initialize}. Define an arbitrary policy $\pi_0(x_k)$, and an arbitrary initial value function $V_0(x)$.
        \State \textbf{Policy Evaluation}. Determine the value function of the current policy using the multi-step approximation of the Hamilton-Jacobi-Bellman Equation:
        \begin{equation}\label{eq:adp_poli_eval_msvi}
            V_{i+1}(x_t) =  \gamma^{n} V_i(x_{t+1}) + \sum\limits_{l=t}^{t+n-1}\gamma^{l-t}r(x_l, \pi_i(x_l))
        \end{equation}
        \State \textbf{Policy Improvement}. Determine an improved policy. 
        {\myfont
        \begin{equation}
            \pi_{i+1}(x_t) = \underset{\pi(x_t)}{\text{argmin}}[r(x_t, \pi_i(x_t)) + \gamma V_i(x_{t+1})]
        \end{equation}}
        \State If $V_i(x) \equiv V_{i-1}(x)$ then terminate, else $i = i+1$ and return to step $2$
	\end{algorithmic} 
\end{algorithm}


In a similar vein of leveraging the loose policy condition which \ac{hdp} has, and finding ways of increasing the convergence rate of \ac{hdp}, eligibility traces can also be employed \cite{elig_hdp,elig_gdhp}. Eligibility traces are an alternative method of incorporating additional samples from past timesteps for updating estimates, the objective here is again to improve convergence rates of algorithms, however, the algorithm behind this method is different than multi-step ideas.

Instead of explicitly retrieving samples from certain time steps, eligibility traces simply store the past updates made to the critic or actor, and persistently but at a decaying rate add such updates to the critic or actor for subsequent timesteps. This method is illustrated in \autoref{eq:eligbility_trace}, where $w(t)$ is the set or vector of parameters for a certain function approximator at time $t$, this approximator could be the critic or the actor, $E(t)$ is the eligibility trace at time $t$, and $\Delta w(t)$ is the update made to the function approximator at time $t$.

\begin{align*}
    w(T+1) &= w(t) + \mathcal{E}(t) \stepcounter{equation}\tag{\theequation}\label{eq:eligbility_trace}\\
    \mathcal{E}(t+1) &= \mathcal{E}(t) + \Delta w(t) ,\quad \mathcal{E}(0) = 0\\
\end{align*}

\subsection{Adaptive Critic Designs}\label{subsec:acd}

Adaptive dynamic programming contains a subset of algorithms called \ac{acd}. In this class of algorithms, the estimate of value functions is referred to as the \textit{critic}, the critic is thus ``responsible" for policy evaluation; while the estimate of the policy functions is referred to as the \textit{actor}, which is thus ``responsible" for policy improvement. It should be noted that in literature, the terms \ac{adp} and \ac{acd} can be found to be used interchangeably \cite{adp_for_control, acd, old_acd}, notably with the progenitor of \ac{acd} Werbos also using these terms and \ac{rl} interchangeably \cite{vi_for_optimal_control}. Furthermore, practical implementations of some \ac{adp} and \ac{acd} algorithms are very much alike, as will be stated later during the description of \ac{hdp}. However, in this literature study, the distinction between \ac{adp} being dynamic programming algorithms which are more optimal control-oriented, and \ac{acd} being dynamic programming algorithms which are more reinforcement learning-oriented is made.

Adaptive critic designs can be considered to be reinforcement learning algorithms developed from generalized dynamic programming algorithms \cite{old_acd} whose algorithms are structured similarly to the \ac{pi} \autoref{alg:pi} or the \ac{vi} \autoref{alg:vi}. Just like \ac{adp}, \ac{acd} algorithms are sample efficient enough for entirely online trained controllers to be implemented, which can converge towards a stable controller within a short period of time \cite{tshdp, mshdp_og, adp_for_control}. 

Several main algorithms form the basis of this class: the first and simplest algorithm is \ac{hdp}, the second and slightly more complicated algorithm is \ac{dhp}, and the third but most complicated algorithm is \ac{gdhp}. The increase in complexity comes from what the critics of each algorithm estimate, where \ac{hdp} only estimates the value function, \ac{dhp} estimates the gradient of the value function, and \ac{gdhp} estimates both the value and gradient of the value functions. Two extensions of all three of these basic algorithms exist, the first kind of extension makes the critic function \ac{ad}, which changes the critic from being an estimate of the state-value function to an estimate of the action-value function \cite{og_action_dependent}. The second kind of extension changes how the model estimate is obtained, where an \ac{rls} regressor is used to identify linear systems for the immediate time steps, as opposed to using online supervised learning with neural networks or with offline identified models.

In the \ac{acd} context, the value function is often named the cost-to-go function, whose definition remains to be the return expected from a given state, then the rewards are referred to as one-step costs. 

\begin{align*}
    V(x_t) &= r_t + \gamma V(x_{t+1}) \stepcounter{equation}\tag{\theequation}\label{eq:acd_value_function}\\
    r_t &= \frac{1}{2}(x_t - x_{t, ref})^2 \stepcounter{equation}\tag{\theequation}\label{eq:acd_reward}
\end{align*}
\subsubsection{Heuristic Dynamic Programming}

\ac{hdp} is characterized by using the critic to estimate the value function $V(x_t)$ itself. This critic is evaluated by the critic \ac{td} error $e_c(t)$ defined in \autoref{eq:hdp_critic_td}, and is trained to minimize the critic error function \autoref{eq:hdp_critic_error_function}. To perform training, the function approximator of the critic is updated using a gradient descent method to minimize the error function, meaning the parameters $w(t)$ are updated according to \autoref{eq:hdp_critic_update}.

\begin{align*}
    e_c(t) &= V(x_t) - r_t - \gamma V(x_{t+1}) \stepcounter{equation}\tag{\theequation}\label{eq:hdp_critic_td}\\
    E_c(t) &= \frac{1}{2}[e_c(t)]^2 \stepcounter{equation}\tag{\theequation}\label{eq:hdp_critic_error_function}\\
    w_c(t+1) &= w_c(t) - \Delta w_c(t) \stepcounter{equation}\tag{\theequation}\label{eq:hdp_critic_update}\\
    \text{Where} \quad \Delta w_c(t) &= \eta_c \frac{\partial E_c(T)}{\partial w_c(t)} = \eta_c \frac{\partial E_c(T)}{\partial e_c(t)}\frac{\partial e_c(t)}{\partial V(x_t)}\frac{\partial V(x_t)}{\partial w_c(t)}
\end{align*}

Correspondingly, there is also the actor loss, the actor error function, and the actor update rule that are expressed in the following equations:

\begin{align*}
    e_a(t) &= V(x_t) \stepcounter{equation}\tag{\theequation}\label{eq:hdp_actor_td}\\
    E_a(t) &= \frac{1}{2}[e_a(t)]^2 \stepcounter{equation}\tag{\theequation}\label{eq:hdp_actor_error_function}\\
    w_a(t+1) &= w_a(t) - \Delta w_a(t) \stepcounter{equation}\tag{\theequation}\label{eq:hdp_actor_update}\\
    \text{Where} \quad \Delta w_a(t) &= \frac{\partial E_a(t+1)}{\partial w_a(t)} = \frac{\partial E_a(t+1)}{\partial e_a(t+1)}\frac{\partial e_a(t+1)}{\partial V(x_{t+1})} \frac{\partial V(x_{t+1})}{\partial x_{t+1}} \frac{\partial x_{t+1}}{\partial u_t} \frac{\partial u_t}{\partial w_a(t)} \stepcounter{equation}\tag{\theequation}\label{eq:hdp_actor_weight_chainrule}\\
\end{align*}

Observing the formulation of \ac{hdp} thus far presented, an interesting note can be made of the close resemblance in the practical implementation of \ac{hdp} and \ac{pi} or \ac{vi} algorithms or even the interchangeable use of \ac{adp} with \ac{hdp} \cite{costate_adp, policy_iteration_adp, mshdp_og}.

Observing the update equations, it can be seen that the actor update contains the partial derivative $\frac{\partial x_{t+1}}{\partial u_t}$, this is a derivative that needs to be obtained using a system model which would define the relation between the state $x_{t+1}$ and the action $u_t$. As a result, this makes the \ac{hdp} algorithm model-dependent. However, this dependency can be removed if \ac{hdp} is made \ac{ad}, in which case the value function would be a function of both state and action $V(x_t, u_t)$, this allows the chain rule expansion in \autoref{eq:hdp_actor_weight_chainrule} to reduce the term $\frac{\partial V(x_{t+1})}{\partial x_{t+1}} \frac{\partial x_{t+1}}{\partial u_t}$ to only $\frac{\partial V(x_{t+1})}{\partial u_t}$ since $V$ would be an explicit function of $u_t$, thus eliminating the system dynamics $\frac{\partial x_{t+1}}{\partial u_t}$.

An alternative way of alleviating model dependency is to use the incremental method of Zhou et al., who developed the \ac{ihdp} algorithm and successfully applied it to several tasks, including a flight control task \cite{ihdp}, and a launch vehicle control task \cite{ihdp_lv}. In this case, the partial $\frac{\partial x_{t+1}}{\partial u_t}$ can be replaced by a control effectiveness matrix from an online identified linear system. Note that this does not make the algorithm entirely model free, since the formulation of the update rules still involves using system dynamics, i.e requires environment modeling.

This algorithm performs relatively poorly compared to \ac{dhp} and \ac{gdhp} in terms of control performance and disturbance rejection \cite{dhp_vs_hdp, old_acd}, but nonetheless can still be deployed successfully \cite{adhdp_use_1, adhdp_use_2, hdp_use}.

\subsubsection{Dual Heuristic Programming}

\ac{dhp} is characterized by the critic estimating the gradient of the value function, instead of the value function directly. This gradient can also be referred to as the \textit{costate} \cite{pi_and_vi_algorithm}:

\begin{equation}\label{eq:costate}
    \lambda(x_t) = \frac{\partial V(x_t)}{\partial x_t}
\end{equation}

Here, the actor formulation is identical to the \ac{hdp} algorithm, and only the critic formulations are changed. The critic \ac{td} error is now defined using gradients of the value function, as shown in \autoref{eq:dhp_critic_td}, with the critic error function and function parameter update remaining unchanged.


\begin{align*}
    e_c(t) &= \lambda(x_t) - \frac{\partial r_t}{\partial x_t} - \gamma \lambda(x_{t+1}) \frac{\partial x_{t+1}}{\partial x_t}\stepcounter{equation}\tag{\theequation}\label{eq:dhp_critic_td}\\
    E_c(t) &= \frac{1}{2}[e_c(t)]^2 \stepcounter{equation}\tag{\theequation}\label{eq:dhp_critic_error_function}\\
    w_c(t+1) &= w_c(t) - \Delta w_c(t) \stepcounter{equation}\tag{\theequation}\label{eq:dhp_critic_update}\\
    \text{Where} \quad \Delta w_c(t) &= \eta_c \frac{\partial E_c(T)}{\partial w_c(t)} = \eta_c \frac{\partial E_c(T)}{\partial e_c(t)}\frac{\partial e_c(t)}{\partial \lambda(x_t)}\frac{\partial V(x_t)}{\partial w_c(t)}
\end{align*}

With this variation, the model-dependency of the algorithm has increased, as the critic \ac{td} error also uses the system dynamics in the form of $\frac{\partial x_{t+1}}{\partial x_t}$ in its formulation. Here, introducing an \ac{ad} variant will not remove the model dependence from the algorithm entirely, only from the actor component.

Because the critic function in \ac{dhp} directly outputs value function gradients, which are needed in actor updates, there are no additional numerical errors that get injected into the actor function parameter update, which cannot be said for the \ac{hdp} algorithm. 

This algorithm is extended to an incremental model identification version, resulting in \ac{idhp}. Application of \ac{idhp} to the task of flight control demonstrated improved reference tracking performance and fault tolerance than a pure \ac{dhp} algorithm \cite{idhp},  wherein the \ac{idhp} controller was able to recover control of the simulated aircraft even after the flight dynamics were reversed mid-flight. 

\subsubsection{Globalized Dual Heuristic Programming}
\ac{gdhp} is characterized by the critic estimating the value and gradient of the value function simultaneously.

\section{Deep Reinforcement Learning}


\subsection{Function Approximators and Deep Learning}\label{subsec:func_approx}

Deep learning refers to the subfield of machine learning which studies deep neural networks and their applications. This field has its origins in the ideas of artificial neural networks and is a scientific effort that has dramatically changed the idea of what an artificial neural network is and what it might be capable of. Deep neural networks fundamentally are an expansion of artificial neural networks, they use the same basic architecture of a neural network, with layers of interconnected neurons between a surface or input layer and a final output layer. The distinction between deep and artificial neural networks is that deep neural networks use many more layers and nodes than are typical in artificial networks,

\subsection{Merging TD \& Deep Learning}

Basic temporal difference algorithms utilize tabular look ups to retreive the value function of each state or state-action pair, this can hinder scaling to larger problem spaces which have many state or action variables, as the number of values to be stored increases combinatorially. The idea was thus put forth that function approximators should be used to approximate these look up tables which provided the scaling power necessary for higher dimensional tasks \cite{function_approximators}, with deep neural networks and multi layer perceptrons becoming popular approximators as they demonstrate a better scalability and approximation power \cite{drl_benchmarking, drl_atari, drl_humanlvl}. However, these function approximators need not necessarily be deep neural networks; in fact, early works focused on the application of simpler and smaller-scale approximations. Such as using coarse tile encoding, linear polynomials, Fourier basis, and many more.


\section{Classes of Reinforcement Learning Algorithms}

At the highest level, RL algos can be classified along three main dimensions:

\begin{itemize}
    \item model-based vs model-free
    \item policy-based vs value-based
    \item on-policy vs off-policy
\end{itemize}


\section{Reinforcement Learning Synopsis}


Algorithmically, adaptive critic designs share many features with the actor-critic class of algorithms developed from the intersection of deep learning and reinforcement learning, both use function approximations instead of explicit functions, both approximate an actor and a critic, and they both can be considered to be emulations of the biological process of learning. 


\section{Flying-V}

\end{document}