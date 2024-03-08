
- Date & time: 08/03/2024
- Tag: #question
- Project:

---

## Question

What is covariance windup and why does it occur in RLS estimators?

## Context

Trying to learn more about the RLS estimator used in IDHP. In Casper and Stefan's work, they set the gamma of RLS to be 1, in order to counter the effects of covariance exponentially increasing when the system is unexcited. This effectively reduces the RLS into an OLS which uses all past data to identify the system model, **which makes the controller less adaptive!!**

So i got interested in the theory behind covariance windup.


## Answer

Covariance windup is when the covariances of the estimator increases exponentially, which happens when the system is not persistently excited.

### Intuitive explanation

There are certain steps in the RLS algorithm that requires some sort of division or inversion of matrices, so eventually a division by zero would happen when the system is not excited. 

### Mathematical Proof


$$
\begin{align*}
P_{n+1} &= \frac{1}{\gamma}(P_n - k_n u_n^{\top}P_n)\\
k_n &= \frac{P_n u_n}{\gamma + u_n^{\top}P_nu_n}\\
\text{Now assume that}&\text{ u is the same at every time step} \\
P_0 &= I \\
k_0 &= \frac{P_0 u_0}{\gamma + u_0^{\top}P_0u_0}\\
k_0 &= \frac{u}{\gamma + u^{\top}u}\\
P_1 &= \frac{1}{\gamma}(I - k_0 u^{\top})\\
k_1 &= \frac{Pu}{\gamma + u^{\top}Pu}\\
k_1 &= \frac{\frac{1}{\gamma}(I - k_0 u^{\top})u}{\gamma + u^{\top}\frac{1}{\gamma}(I - k_0 u^{\top})u}\\
k_1 &= \frac{(I - k_0 u^{\top})u}{\gamma^2 + u^{\top}(I - k_0 u^{\top})u}\\
k_1 &= \frac{u - k_0 u^{\top}u}{\gamma^2 + u^{\top}(u - k_0 u^{\top}u)}\\
k_1 &= \frac{u - k_0 u^{\top}u}{\gamma^2 + u^{\top}u - u^{\top}k_0 u^{\top}u}\\
k_1 &= \frac{u - \frac{u}{\gamma + u^{\top}u} u^{\top}u}{\gamma^2 + u^{\top}u - u^{\top}\frac{u}{\gamma + u^{\top}u} u^{\top}u}\\
k_1 &= \frac{(\gamma + u^{\top}u)u - u u^{\top}u}{(\gamma + u^{\top}u)(\gamma^2 + u^{\top}u) - u^{\top}u u^{\top}u}\\
k_1 &= \frac{\gamma u}{\gamma^3+\gamma^2 u^{\top}u + \gamma u^{\top}u}\\
k_1 &= \frac{u}{\gamma^2+\gamma u^{\top}u + u^{\top}u}\\
P_2 &= \frac{1}{\gamma}(P_1 - k_1 u^{\top}P_1)\\
P_2 &= \frac{1}{\gamma}(\frac{1}{\gamma}(I - k_0 u^{\top}) - k_1 u^{\top}\frac{1}{\gamma}(I - k_0 u^{\top}))\\
P_2 &= \frac{1}{\gamma^2}((I - k_0 u^{\top}) - k_1 u^{\top}(I - k_0 u^{\top}))\\
P_2 &= \frac{1}{\gamma^2}(I - k_1 u^{\top})(I - k_0 u^{\top})\\
k_2 &= \frac{P_2 u_2}{\gamma + u_2^{\top}P_2u_2}\\
k_2 &= \frac{P_2 u}{\gamma + u^{\top}P_2u}\\
\end{align*}
$$

Short version:

$$
\begin{align*}
\text{Update rules: }
P_{n+1} &= \frac{1}{\gamma}(P_n - k_n u_n^{\top}P_n)\\
k_n &= \frac{P_n u_n}{\gamma + u_n^{\top}P_nu_n}\\
\text{Initialization: }
P_0 &= I \\
k_0 &= \frac{u}{\gamma + u^{\top}u}\\
\text{Now assume that}&\text{ u is the same at every time step} \\
P_1 &= \frac{1}{\gamma}(I - k_0 u^{\top})\\
k_1 &= \frac{u}{\gamma^2+\gamma u^{\top}u + u^{\top}u}\\
P_2 &= \frac{1}{\gamma^2}(I - k_1 u^{\top})(I - k_0 u^{\top})\\
&\cdots\ \text{maybe there's a pattern?}\
\end{align*}
$$

Since $u^{\top}u$ is an inner product of two identical vectors, its product is a non zero positive scalar. Then it can be concluded that:
$$
\begin{align*}
	\gamma + u^{\top}u &> \gamma^2+\gamma u^{\top}u + u^{\top}u\\
	\gamma &> \gamma^2+\gamma u^{\top}u\\
	1 &> \gamma+ u^{\top}u\\
\end{align*}
$$

Without making assumption on structure of P:
$$
\begin{align*}
P_0 &= P \\
k_0 &= \frac{Pu}{\gamma + u^{\top}Pu}\\
P_1 &= \frac{1}{\gamma}(I - k_0 u^{\top})\\
k_1 &= \frac{Pu}{\gamma^2+\gamma u^{\top}Pu + u^{\top}Pu}\\
\text{not complete....}
\end{align*}
$$


