
- Date & time: 08/03/2024
- Tag: #literature
- Project:

---

## Context

This paper was one of 3 papers sent to me from Erik-Jan after our discussion on the similarities between eligibility traces and momentum based gradient descent. It is a paper which tries to proof the hypothesis that 

> **Momentum on TD(0) can perform worse than TD(λ), and actually hinder performance on some environments possibly dues to biasing of updates.**

## Resource 

https://dhawgupta.com/resources/rl2.pdf

This paper first poses two momentum modifications:
1. Momentum augmented TD(0), so called momentum(0)
2. Momentum augmented TD($\lambda$), so called momentum($\lambda$)

His results shows that momentum(0) is worse than TD($\lambda$).

Slightly annoyingly, he does not test how the performance of momentum($\lambda$) is compared to TD($\lambda$). Effectively, this paper compares the utility of eligibility traces versus momentum gradient descent, and shows that eligibility traces are superior in sampling efficiency and asymptotic performance (note that asymptotic performances could not be clearly ascertained since he does not run the experiments long enough).


### Regarding distinction between eligibility trace and momentum:

He first makes the following distinction in the case of linear function approximators:
- Eligibility traces maintain a trace of previous *features* ($\mathbf{x}_t$)
- Momentum maintains a trace of previous *parameter updates* ($\delta_t \mathbf{x}_t$)
And he claims that momentum will bias updates with the older temporal difference errors $\delta_{t}$, which sheds some light on the difference between the two approaches. With eligibility traces, you only keep track of which exact parameters are updated, i think of this as indicating whichever parameter are legible for an update; and when you execute an update with eligibility traces, you use the current TD error to apply a change to the eligible parameters. Whereas with momentum, the algorithm instead just looks at the value by which past parameters are changed (**which notably factors in the TD error already**) and then repeats this update; thus momentum algorithms continually reuses previous TD errors at diminishing intensity, as oppose to eligibility traces which do not use past TD errors at all, only using the current TD error.

He then claims that:

> *Momentum differs from TD($\lambda$), where TD($\lambda$) maintains a trace of $\nabla \hat{v}$, but momentum maintains a moving average of the gradient of the objective function.*

Which is a pretty good summary of the distinction.