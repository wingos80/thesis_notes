
- Date & time:  27/03/2024
- Tag: #literature
- Project:

---

## Context

Came across this when first writing up the IDHP augmentations document, but realised this is more useful than expected since it addresses incompatibility issues between neural networks and eligibility traces, specifically the issue of "diverging gradients", and proposes a solution.

## Resource

The paper: https://doi.org/10.1016/j.robot.2021.104019

This paper observes that in the case of neural networks, the gradients that are accumulated can diverge from the gradients computed using the latest parameters. This effect is called "gradient divergence", and is more marked the deeper the neural network is.

To overcome this divergence, this paper proposes to use a series of eligibility traces, instead of only 1 trace, with each trace corresponding to different time scales by decaying each trace with different rate $\lambda$s. The parameter updates of the neural network is done using only the top most trace (the trace with highest $\lambda$, which decays slowest!). Furthermore, each trace's decay rate is made a function of the output divergence between subsequent neural networks with parameters $\theta_{t}$ and $\theta_{t-1}$ respectively, this divergence can be the Euclidean norm in the case of deterministic neural networks (which is the case for both actor and critic in IDHP), or a probabilistic measure such as KL-divergence if stochastic networks were considered.

Sidenote, they refer to this one other source that says shallow NN's do not face the issue of gradient divergence as much as DNNs: https://doi.org/10.1016/j.neunet.2017.12.012


