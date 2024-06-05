
- Date & time:  28/05/2024
- Tag: #personal #progress_meeting
- Project:

---

## Context

progress meeting notes with erik-jan

## Note

- does cascaded actor network require cascaded critic network?
- Vlad says that freezing DRL weights and then using them is what the industry see being viable to do for flight controllers, but then why not just use robust controllers
- publishing into journal, what will it take to get my research into a journal
- any companies in the industry that does rl for flight control/closely related work?
- make clear what the main products of this second research phase should be, research paper + completed thesis + additional results...
- How to do cascaded policy structure? How will the update equations for the separate actors look like?
- Does sending the trim input to the aircraft achieve the exact same effect of finding a trim actor network? Am i missing something about the trim actor network?
- critic for idhp should be derivative of value function with respect to MDP states right? Not wrt to model states (which may contain all or some elements of MDP states)?
- Thoughts about my analysis and evaluation of the four algorithms
- Any suggestions on interesting topics to explore in this second research phase.
- Reconciling the lambda(s) term. Since the critic output is defined as derivative of the state value function with respect to the MDP states, then that causes issues with the update rules since the model and MDP states do not always perfectly coincide. This issue is apparent when computing actor and critic update equations, which requires a matric multiplication of an identified state/input transition matrix with the lambda function.
	- The critic output is defined as $$\lambda(s) = \frac{\partial V(s)}{\partial s}$$
	- Which has the same dimension as the mdp state space. But in my work, i write lambda(s) as: $$\lambda(s) = \frac{\partial V(s)}{\partial x}$$
	- as a vector with the same dimension as the model state space. This difference can be reconciled by expanding my definition of lambda(s) to also include the partial derivative between the mdp state space and model state space: $$\lambda(s) = \frac{\partial V(s)}{\partial s}\frac{\partial s}{\partial x}$$
	- Where now the second partial, derivative of s wrt x, is actually a vector with the same dimension as the model state space, yay, lambda(s) dimension reconciled.
	- And then with this ds/dx partial, you can make the actor and critic update equations work, where the F ang G matrices from the RLS are the aircraft state and input transition matrices, and then u use the ds/dx partial to make the shapes of lambda work with the shape of F and G.
- why no course requirement to do thesis under you?
- the nMAE, how do i normalize? What do i use as the normalization factor? Does it matter what factor i use as long as i am consistent?
	- answered by Vlad, normalize it so that the normalized error ranges from -1 to 1 basically, or from 0 to 1



- remove reverser boxes