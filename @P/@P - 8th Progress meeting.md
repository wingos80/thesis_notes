
- Date & time:  
- Tag: #personal
- Project:

---

## Context

progress meeting ntoes with erik jan, expected results till this meeting is an implemented and working vanilla IDHP.

## Note
 
Prep notes for meeting:
- cool things: found difference btwn eligibility & momentum, got a working IDHP, got working PSD's, ran many hyperparameter configurations, have ideas for multi-processing to be more compute efficient for data generation.
- Should RLS identify the exact linear state space model essentially?
- Did experiment with the ''buggy IDHP. First realised what bug was causing RLS eps norm 0 bug, had to fix update of action variable. With the RLS bug fixed, i suspect it should only be the environment model that's buggy, the chances of the IDHP being buggy is pretty low considering how much i looked over the code/equations. Then compared the reward history of buggy IDHP with and without RLS param updates, i.e. buggy IDHP with and without correct RLS estimator. The two showed essentially identical reward history, which is strange since i would expect RLS estimator to provide better performance, but could be because of the bug in IDHP or because the control task is not difficult enough to highlight difference in the two.
- With RLS estimator:
- ![[2024-03-12 195932, identity rls params with innovation.png]]
- Without RLS estimator:
- ![[2024-03-12 195212, identity rls params no innovation.png]]
- While giving a sinusoidal reference signal, the lower the frequency the more likely the agent was going to act really wavey/oscillatorily.
- In RLS, whether i have the parameter updates start at 1sst or second timestep does not matter.
- If i initialize covariance of RLS low, then the tracking becomes better than if covariance was high, and then high covariance fixes the diverging issue of the algorithm? Pic of high init cov:
- ![[2024-03-16 164435, accurate RLS inaccurate IDHP.png]]
- 

Random pre-notes for meeting:
- Tell erik to pass on the word that students using python should try to minimize number of dependencies utilized and use anaconda to maximize reproducibility, from personal experience i can tell this is the biggest hurdle to reproducibility.
- Where is Dr Chu, I've seen him online in random places and it seems like he's a professor at C&S, but i have never seen/heard about him in person.
- Knowledge embargo took around 5 weeks to get approved.
