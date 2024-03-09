
- Date & time: 09/03/2024
- Tag: #literature
- Project: 

---

## Context

This paper was one of 3 papers sent to me from Erik-Jan after our discussion on the similarities between eligibility traces and momentum based gradient descent. It very clearly presents the difference between eligibility traces and momentum based approached by deriving the equation for parameter updates at every step.

## Resource

doi: 10.1109/CEEC.2017.8101599

To show the difference between momentum and eligibility traces, this paper derives the equation for the parameter update at any given time step. 

For momentum, this equation is as follows:

$$
	\begin{equation}
		\Delta \theta_{t} = \sum\limits_{k=0}^{t-1}\mu^k\delta_{t-k}\nabla_{\theta_{t-k}}Q(s_{t-k},a_{t-k})
	\end{equation}
$$

While for eligibility, this equation is as follows:

$$
	\begin{equation}
		\Delta \theta_{t} = \sum\limits_{k=0}^{t-1}\mu^k\delta_{t}\nabla_{\theta_{t-k}}Q(s_{t-k},a_{t-k})
	\end{equation}
$$

The difference in coefficients in these two equations are trivial, the only difference which matters is the timestep from which the TD error is taken. In momentum, the parameter update uses every single TD error that has been observed thus far, while for eligibility traces, the update only uses the most recently observed TD error.

The paper then suggest this difference intuitively suggests eligibility updates are more useful than momentum updates. As the agent improves over time, it should not use the previous TD errors for training since they arise from when the agent was less optimal, and this does not happen in the case of eligibility updates.

This paper also references a different paper which uses ACD's as the context to discuss the difference between eligibility and momentum.