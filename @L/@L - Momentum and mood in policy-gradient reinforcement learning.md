
- Date & time: 08/03/2024
- Tag: #literature
- Project:

---

## Context

This paper was one of 3 papers sent to me from Erik-Jan after our discussion on the similarities between eligibility traces and momentum based gradient descent. It is a paper which combines eligibility traces with momentum into one TD($\lambda$) algorithm. 


## Resource 

https://bennett-daniel.github.io/assets/pdf/2019_rldm.pdf

This paper introduces two modifications to TD($\lambda$):
1. Adding momentum to parameter updates $u_t$:
   $$
	\begin{align*}
		\delta_t &= r + V_{t-1}(s') - V_{t-1}(s) \\
		V_t(s) &= V_{t-1}(s) + \alpha \delta_t \\
		e_t &= \lambda e_{t-1} + \nabla_{\theta}log_{\pi_{\theta}}(s,a)\\
		u_t &= \eta e_t \delta_t + \underbrace{ m u_{t-1}}_{momentum}\\
		\theta_{t+1} &= \theta_t + u_t 
	\end{align*}
  $$
2. Adding mood to the momentum:
   $$
	\begin{align*}
		\delta_t &= r + V_{t-1}(s') - V_{t-1}(s) \\
		V_t(s) &= V_{t-1}(s) + \alpha \delta_t \\
		h_t &= \underbrace{h_{t-1} + (1-m)(\delta_t - h_{t-1})}_{mood}\\
		e_t &= \lambda e_{t-1} + \nabla_{\theta}log_{\pi_{\theta}}(s,a)\\
		u_t &= \eta e_t (\delta_t + \frac{m}{1-m}h_t)\\
		\theta_{t+1} &= \theta_t + u_t 
	\end{align*}
  $$

And shows that they both improve sample efficiency over the baseline TD($\lambda$) but same asymptotic performance, however with the momentum modification being better than mood. 
The interesting thing here is how the first modification is a TD algorithm with both eligibility traces and momentum gradient descent combined, and the paper shows this is an improved arrangement compared to the original algorithm. 

Even though the discussion which the search for this paper originated was on the difference between momentum and eligibility traces, this paper does not shed any insight on that matter.
