
- Date & time:  29/05/2024
- Tag: #personal
- Project:

---
## Context

Wanted to meet with Spilios about this one research question i posed: "How well does the proposed controller’s nominal performance and fault-tolerance compare to a traditional PID controller?"

Me and Erik's feeling was that the answer to this question is not useful, that it would be as if i am finding out a coin has two sides, that an adaptive controller can adapt and a fixed controller cannot adapt...
## Note

Robustness comes in layers. The layers that you should have in mind when you refer to something as "robust" are:
	- layer 1, gain phase margins, which show relative robustness, that variations to the nominal plant will still yield stability (also performance?)...
	- layer 2, robustness to uncertainty, that for the whole extent of possible plants given your uncertainty model (structured, unstructured) will still yield stability and/or performance
	- layer 3, robust stability or robust performance, that you can guarantee the system is stable or even has the desired performance in the face of uncertainty, noise, disturbances, ... etc.

But ultimately, robustness means being robust to model uncertainty in all the meanings of the word uncertainty.

Spilios posed the following more valid (in his opinion) alternative question to my research question: how does model based (robust, indi, mpc, ...) compare to data driven controller (rl, drl).

Robust controllers are guaranteed to be robust, period.

Spilios' remark regarding robust controller downfalls. Suppose we have a single controller which is robust for all regions of the flight envelope, that single robust controller will have certain downfalls. Primarily, its controller parameters will not vary, i.e. it is not adaptive. I followed up with a question:
- would another downfall be that the model is linear? 
	- Answer: theoretically yes, practically not really, airplanes are very linear. Put it another way, nobody has complained that a robust controller is not robust.

In his mind, data-driven controllers have one domain where they are the only feasible controller option, this domain are the situations where you cannot get any models simply. E.g:
- airplane loses engine,
- wing falls off
- any extreme changes to a system
- industrial processes where it is very difficult to get a model
- biological systems

