# 2nd literature review

The first literature review focused on getting a broad overview of what flight control related RL MSc research has been conducted in the past. As the thesis project is to apply RL for flight control of the Flying-V, it is also important to gain a grasp of **a)** the development history of the Flying-V,  **b)** the flight control properties of the Flying-V, and **c)** any special characteristics of the Flying-V that warrants special attention from the flight control system.

The list of literature reviewed in this batch are as follows:

1. *J Benad* (2015): The Flying V - A new Aircraft Configuration for Commercial Passenger Transport
2. *Benad J, Vos R* (2022): Design of a Flying V Subsonic Transport
3. *Simon van Overeem* (2023): Handling Qualities Improvements for the Flying-V aircraft using Incremental Nonlinear Dynamic Inversion
4. *Ruiz-Garcia A, Vos R, de Visser C C* (2020): aerodynamic model identification of the flying v from wind tunnel data
5. *M Palermo, R Vos* (2020): Experimental aerodynamic analysis of a 4.6%-scale flying-v subsonic transport
6. *Thibaut Cappuyns* (2019): Handling Qualities of a Flying V configuration
7. *Simon van Overeem* (2022): Modelling and Handling Quality Assessment of the Flying-V Aircraft
8. *Sjoerd Joosten* (2022): Piloted Assessment of the Lateral-Directional Handling Qualities of the Flying-V
9. *Kevin Siemonsma* (2022): Aerodynamic Model Identification of the Flying-V Using Flight Data
10. *Jurian Stougie* (2022): INDI with Flight Envelope Protection for the Flying-V
11. *van Empelen S.A, Vos R* (2021): Effect of engine integration on a 4.6%-scale flying-v subsonic transport
12. *hearwood T.R., Nabawy M.R.A., Crowther W.J., Warsop C* (2023): Coordinated Roll Control of Conformal Finless Flying Wing Aircraft
13. *Ganesh TS, Keerthi MC, Girish Sabari, Sreeja Kumar S, Mrunalini B* (2021): Control of Tailless Aircraft.
14. *Bolsunovsky AL, Buzoverya NP, Gurevich BI, Denisov VE, Dunaevsky AI, Shkadov LM* (2001): Flying wing - Problems and decisions
15. *Bravo-Mosquera Pedro D, Catalano Fernando M, Zingg David W* (2022): Unconventional aircraft for civil aviation: A review of concepts and design methodologies
16. *Campos Luis MBC, Marques Joaquim MG* (2021): On the handling qualities of two flying wing configurations
17. *Mcdonald Robert A, German Brian J, Takahashi T, Bil C, Anemaat W, Chaput A, Vos R, Hrrison N*: Future aircraft concepts and design methods
18. *CHEN Z., ZHANG M., CHEN Y., SANG W., TAN Z., LI D., ZHANG B*: Assessment on critical technologies for conceptual design of blended-wing-body civil aircraft
19. *Shan Y., Wang S., Konvisarova A., Hu Y*: Attitude Control of Flying Wing UAV Based on Advanced ADRC
20. *Wildschek A.a, Bartosiewicz Z.b, Mozyrska D.b*: A multi-input multi-output adaptive feed-forward controller for vibration alleviation on a large blended wing body airliner
21. *Zhang Liqi, Zhao Yonghui*: Adaptive Feed-Forward Control for Gust Load Alleviation on a Flying-Wing Model Using Multiple Control Surfaces
22. 

Literature sources:
- From Willem's thesis
- Scopus 

<figure>
    <figcaption><b>Table 1</b>, search queries used on Scopus.</figcaption>
</figure>

| Subject Area|  Query |
| ------------   |  ------- |
| Engineering | "flying-v" |
| Engineering | "flying wing" AND "tailless" |
| Engineering | "flying wing" AND "tailless" AND "flight control" |
| Engineering | "flying wing" AND "sweep" |
| Engineering | "flying wing" AND "high sweep" |
| Engineering | "blended wing body" AND "flight control" |
| Engineering | ( "flying wing" OR "blended wing body" ) AND ( "flight control" OR "handling" ) |
| Engineering | ( "flying wing" OR "blended wing body" ) AND ( "flight control" OR "handling" ) AND "fault tolerance"|
| Engineering | ( "flying wing" OR "blended wing body" ) AND ( "flight control" OR "handling" ) AND "fault"|
| Engineering | ( "flying wing" OR "blended wing body" ) AND ( "flight control" OR "handling" ) AND "safety"|


While the gathered literature all pertain to flying wing, it is important to note that the Flying-V is a special kind of flying wing due to its' high sweep angle, distinguishing it from more rectangular wing flying wing planforms; it is also shaped like a V instead of a Delta, distinguishing it from the delta flying wing planforms. An implication of this distinction is that flight control and handling properties specific to the Flying-V can conceivably not be observed in other flying wing designs, and vice-versa.

The Flying-V as an aircraft concept is in the mid to early Technology Readiness Level. Active research efforts are being conducted on many fronts of the aircraft's design, ranging from aerodynamics, to structure and materials, to flight dynamics & controls and much more. It is the very act of carrying out these research efforts, in addition of course to the academic and engineering fruits of the research labour, that an early stage aircraft concept like the Flying-V will be brought closer to maturity; just as importantly, it will bring the level of awareness and acceptance by the industry and wider public alike higher. 



---

## Item 1: The Flying V - A new Aircraft Configuration for Commercial Passenger Transport

### Summary -

This paper proposes a flying wing design with a V design. The shape was chosen such that a circular cross section can be adopted for the passenger cabin to minimize wall stresses, and so that the streamwise cross-section of the cabin is elliptical which will fit more naturally into airfoil profiles. 

In regards to controls, the Flying-V was designed to have longitudinal static stability by placing the neutral point aft of the center of gravity, and to have dynamic directional stability by giving the outer section of the wing a dihedral. The handling qualities of this aircraft design were demonstrated through the flight test of a remote controlled scaled down model, which was built with only one set of elevons. The model is then flight tested by an experienced remote control pilot, who comments the aircraft was easy to fly. The stall recovery ability of the design was also demonstrated, by incrementally increasing the angle of attack and reducing velocity, which eventually resulted in a stall. The design could then recover from the stall as its' nose dropped, sending the aircraft into a dive which allowed it to recover velocity and thus lift.

### Questions & Answers -
### Notes -

- cylinder is a structurally efficient shape for a pressure vessel. By placing the passenger cabins along the span of a heavily swept back wing, it is possible to use a cylindrical shape to retain this structural efficiency, in addition to more efficiently fitting the cabin inside an airfoil profile.
---

## Item 2: Design of a Flying V Subsonic Transport

### Summary -

This paper accounts the development timeline of the Flying-V aircraft concept focusing on five fronts: the early formulation of the Flying-V concept, the aerodynamics and structural design, how the concept was extended to encompass a family of designs, flight dynamics and control of the design, and finally the interior design.

The flying V concept was started by [Benad] and sought to improve the fuel efficiency of passenger airliners by blending the wings and body of an aircraft. This blending came in the form of two cylindrical passenger cabins that spanned the length of two highly swept back wings, resembling a V shape. Through this layout, several advantages were gained over the traditional tube and wing aircraft design. These advantages were: elliptical spanwise lift distribution, lack of need for high-lift devices, 10% higher lift to drag ratio, 2% lower empty weight compared to the reference aircraft A350-900, and reduced ground noise (see [Benad]).

The concept was made more detailed through multi-disciplinary optimization of the aerodynamic properties and structural characteristics of the aircraft by [Faggiano]. Here, the circular cross-sections of the passenger cabins were replaced by oval cross sections to make the cabin shape conform more to the airfoil shapes. The aerodynamic design optimization conducted resulted in a Flying-V which had a 25% higher lift-to-drag ratio than the NASA CRM. Further studies into structural design were conducted by [van der Schaft] and [Claeys] to yield a structure that could handle a variety of loading scenarios, including high-g turns, side-gusts, taxiing, and pressurization; which had the conclusion that the Flying-V has a 17% decrease in FEM weight in comparison to an A350-like design.

The Flying-V is made into a family of designs through the works of [Hillen], where the outer mould line was parameterized to enable the design to be varied easily. With this parameterization, 3 variants of the design were made, which could carry 293, 328, and 378 passengers respectively.

Flight dynamics modelling of the aircraft were carried out in multiple efforts. [Palermo and Vos] built a half-wing model at 4.6% scale and conducted wind tunnel experiments to study the stall and lift characteristics of the design, noting the pitch breaking characteristic of the aircraft, and which provided data for [Garcia] to produce an aerodynamic model. The full aircraft at 4.6% scale was later constructed, and was used to gather outdoor flight data, providing a source for [Garcia et al] to further identify an aerodynamic model. 

[Cappuyns]  then performed a study into the handling qualities of the Flying-V, which showed the lateral flight authority of the aircraft was limited in addition to the Dutch roll mode being mildly unstable, likely consequences of the lack of an effective vertical stabilizer. Which warrants for additional attention through additional sizing on the vertical tail planes, and the implementation of flight control systems designed to improve the damping behaviour and control authority of the pilot.



[Benad]: https://pure.tudelft.nl/ws/portalfiles/portal/139663741/370094.pdf
[Faggiano]: https://arc.aiaa.org/doi/epdf/10.2514/6.2017-3589
[van der Schaft]: https://repository.tudelft.nl/islandora/object/uuid:d9c9c02f-d67a-4e3c-93a7-eb20ed67cd03?collection=education
[Claeys]: https://repository.tudelft.nl/islandora/object/uuid:ee7f2ecb-cdb6-46de-8b57-d55b89f8c7e6?collection=education
[Hillen]: https://repository.tudelft.nl/islandora/object/uuid:f4863ae4-2792-4335-b929-ff9dfdb6fed5?collection=education
[Palermo and Vos]: https://arc.aiaa.org/doi/epdf/10.2514/6.2020-2228
[Garcia]: https://arc.aiaa.org/doi/epdf/10.2514/6.2020-2739
[Garcia et al]:https://arc.aiaa.org/doi/10.2514/6.2022-0713
[Cappuyns]: https://repository.tudelft.nl/islandora/object/uuid:69b56494-0731-487a-8e57-cec397452002?collection=education


### Questions & Answers -
### Notes -

---

## Item 3: Handling Qualities Improvements for the Flying-V aircraft using Incremental Nonlinear Dynamic Inversion

### Summary - 

Previous studies into the handling qualities of the Flying-V demonstrated that the design itself lend to undesirable flight dynamics, especially for lateral control and the Dutch roll mode. Tailor made flight control system is one of the answers that can address this issue, and this paper presented INDI as a suitable control system candidate. INDI is an advanced form of control law that has proven robustness characteristics and only needing a control effectiveness model of the system, rather than the full system model like in the design of traditional control laws, thus reducing sensitivity to system model error. Nonetheless, INDI requires a sufficiently high sampling rate sensor suite to be effective.

Using a combination of INDI and traditional PID controllers, a stability augmentation system and a control augmentation system were implemented, the former providing a stabilizing and damping effect to flight dynamics, and the latter providing pilots with fly-by-wire control over the aircraft. The robustness of this system was tested by adding white noise to the aerodynamic coefficients in the model of the Flying-V, and using the designed flight controller to follow a reference trajectory. It was shown that the median performance of the controller remains the same as the nominal case for model uncertainties of up to 25%, albeit with a marginally higher spread in performance. 


### Questions & Answers -
### Notes -

- INDI requires a control input model of the aircraft, which is a potential disadvantage for INDI compared to using RL based controllers as INDI would be sensitive to changes between modelled and actual control effectiveness.

---

## Item 4: Aerodynamic model identification of the flying V from wind tunnel data


### Summary - 

A set of aerodynamic models modelling the forces and moments in the longitudinal plane are created, by using wind tunnel data to identify the aerodynamic coefficients in the models. The set of models are duplicated and modelled using polynomials and splines respectively, and then identified using an ordinary least squares method. These models are validated against a set of training data and can be considered to be accurate representation of the Flying-V's longitudinal flight dynamics.

From the models, it can be seen that indeed the aircraft is longitudinally stable, as the $C_{M_{\alpha}}$ coefficient is negative in the polynomial model. This, however, is only true at lower angles of attack. Beyond high angles of attack around 19 degrees the $C_m$ data points increase as a function of angle of attack, meaning that $C_{M_{\alpha}}$ becomes positive. This observation in wind tunnel measurements is also translated into both the aerodynamic models.


### Questions & Answers -

1. "A global longitudinal aerodynamic model is estimated", global in what sense though?
	1. 
2. The $C_{M_{\alpha}}$ in the identified spline model is positive? Indicating longitudinal static instability? Confirm Roelof Vos measurements from his wind tunnel data paper.
	1. In the polynomial model $C_{M_{\alpha}}$ is negative, suggesting stability. Why this coefficient has opposite signs between the spline and polynomial model is unknown.

### Notes -

- This paper did not give much not on the aerodynamic properties of the Flying-V, more so on how to identify the model and the identified models' quality.

---

## Item 5:  Experimental aerodynamic analysis of a 4.6%-scale flying-v subsonic transport


### Summary - 

The flying-V's aerodynamic characteristics were analyzed using wind tunnel measurements of a scaled down model. The analysis done were only concerned with the longitudinal characteristics of the design, wherein the angle of attack, velocity, and elevon deflections were varied and resulting forces and moments were measured. Out of these test campaigns, a series of force and moment measurements were obtained which is used by other researchers to perform system identification on the Flying-V, in addition to notable results on its' flight dynamics.

Firstly, the design demonstrates static stability up until angle of attack of around 19 degrees, whereafter instability sets in as the $C_{M_{\alpha}}$ value becomes positive, which will cause the aircraft to enter an aggressive pitch up: a phenomenon known as *pitch-break*. This is caused by the aerodynamic center of the wing shifting forward, presumably past the center of gravity, at high alphas.

Secondly, the experiments showed that the control effectiveness of the elevons remain constant as a function of angles of attack. This is shown by the pitching moment derivatives of the two inboard elevons exhibiting marginal correlation to angle of attack, whilst remaining at effective magnitudes. Because the modelled Flying-V has multiple sets of elevons, all adjacent to one another, the wind tunnel experiments also tested the interacting behaviour between elevons. This interaction is captured in a coefficient $\xi_{ij}$, which roughly conveys by what factor the pitching moment derivative of elevon $i$ varies as the elevon $j$ is deflected. It is observed that the control effectiveness of an elevon falls as an adjacent one is deflected, this effect is weak for low angles of attack (below 10 degrees), but for higher angles of attack the interactions become more significant. When an adjacent elevon is negatively deflected, an elevon's control effectiveness increases between 20% to 100%, but when the an adjacent elevon is positively deflected, control effectiveness can be estimated to decrease by as much as 80%.

### Questions & Answers -

1. The power setting of the engine apparently has an effect on the aerodynamics?


### Notes -

- negative elevon deflection means downward deflection, remember that the deflection angle is defined wrt the right handed axis system. So maximum *positive* deflection means full down elevons, maximum *negative* deflection means full up elevons.

---

## Item 6: Handling Qualities of a Flying V configuration

### Summary - 
### Questions & Answers -
### Notes -

- While the Flying-V is in the trim point on approach, the elevons would be more than 50% saturated if only 1 elevon is used, but if both are used then they would only be about 30% saturated.
- The CAP of the Flying-V during cruise and approach (category B & C respectively) are found to lie mostly within the level 1 quality, showing that the design has a good is not too agile or too sluggish. Opposite of what Simon van Overeem found??
- Thibaut has the CAP plots with $\omega_{sp}$ on y axis and $N_{\alpha}$ on the x axis, it is generated from a flight mechanics model of the Flying-V using self made VLM data.
- Simon van overeem has the CAP plots with $CAP$ on y axis and $\zeta_{sp}$ on x axis, and is generated from a Flying-V model identified from combined previously obtained VLM and wind tunnel data.
- Dutch roll during cruise and approach is the only unstable mode.


--- 

## Item 7: Modelling and Handling Quality Assessment of the Flying-V Aircraft

### Summary - 

A 6 DOF rigid body nonlinear flight simulation of the Flying-V is built, by combining aerodynamic data from CFD (VLM specifically) data and wind tunnel experiments. A linearization of the full nonlinear model is performed, from which it is then used to assess the handling quality of the Flying-V through first computing the eigenvalues corresponding to the 5 dynamic modes, and then by computing control anticipation parameters.

The eigenvalue analysis is split between approach and cruise conditions, which are then split between fore and aft cg location. Several unstable modes during approach can be identified; these are the Dutch roll, phugoid, and spiral modes, while during cruise only the Dutch roll mode with a fore cg position is unstable. This comprises the primary findings of the eigenvalue analysis. The secondary findings is that the handling quality of the Flying-V can be slightly sluggish, where the damping ratio of several modes do not meet level 1 quality, with some unstable modes failing to meet the lowest quality of level 3.

Then a CAP analysis of the Flying-V shows that the agility falls as center of gravity shifts aft, a finding consistent in [flying wing designs]. Furthermore, the aircraft exhibits primarily level 2 handling qualities, and in certain cg positions it borders on possessing level 1 qualities.

Overall the flying V has a split level 1 and level 2 handling quality, with a number of level 3 or worse qualities; an undesirable characteristic for the aircraft to have. This could be seen as a sacrifice made by discarding the tube and wing design with horizontal and vertical stabilizers, which is a design that can be made naturally stable with relatively small and simple modifications. Nonetheless, these are flying qualities prior to design modifications or implementation of a stability or control augmentation system, also known as a flight controller or a fly by wire system.

[flying wing designs]: https://www.dglr.de/publikationen/2018/450043.pdf

### Questions & Answers -
### Notes -

- This introduction is very good at explaining the advantages of Flying-V
- The intro is also very good and concise at explaining the handling quality/flight dynamic challenges faced by the Flying-V.
- Level 1 handling quality is best, level 2 is stable but sluggish, level 3 is extra pilot effort or instability. 
- During approach, phugoid, spiral, and Dutch roll are unstable.
- During cruise, only dutch roll with forward cg is unstable.

--- 

## Item \*: Safe Exploration Algorithms for Reinforcement Learning Controllers

doi: 10.1109/TNNLS.2017.2654539

### Summary - 

SHERPA is a safety filter algorithm that is used to evaluate whether an action is safe or not, by testing if the resulting state transition from the action will bring the agent outside of a set of safe state space interval, and if so it goes through a stored list of backup actions until it either finds a safe action or reverts to the previous action.

### Questions & Answers -
### Notes -

- *SSS* means safe state space.
- original SHERPA can crash if the controlled system is difficult, and it is unable to distinguish between almost satisfactory controls and fatal controls. OptiSHERPA is thus introduced to address these drawbacks.

--- 

## Item \*: Safe Curriculum Learning for Optimal Flight Control of Unmanned Aerial Vehicles with Uncertain System Dynamics

MSc thesis of Tijmen Pollack.

### Summary - 


### Questions & Answers -
### Notes -

- The actor is a collection of NN's, and the critic is another collection of NN's
- Implication of a system having Ergodicity is that there exists a path between any two points in the safe state space, the mathematical idea of Ergodicity applies more generally as well to meaning that a system will eventually visit all points in its' state space. 
	- [this paper] talks about safe exploration of agents in MDPs, and elaborates on ergodicity: "*The essence of ergodicity is that any state is eventually reachable from any other state by following a suitable policy*".
	- The paper also states that existing reinforcement learning algorithms provide strong exploration guarantees, but assumes this idea of ergodicity by assuming that it would be possible to go from a potential unsafe point in the state space back to a safe point. This of course is not true for all state spaces, for example it is possible to pilot an airplane to explore the stalling region of the state space, but in some cases it is not possible for an airplane to return from this stall region back to the nominal flight conditions; therefore not all state spaces are ergodic.
- [this other paper] talks about curriculum learning.
- *intra-task* learning is when the dynamics of the target or final MDP is shared by any and all of the intermediate MDPs,  meaning that the distinctions between subsequent stages of learning is in how much of the state space or action space are accessible. *Inter-task* on the other hand refers to learning where the target MDP has different dynamics than the intermediate MDP's.
- He used an intra-task learning framework for the quadrotor training task, but i could not tell how he transferred (technically it's called mapping) the actor and critic networks from one learning stage to another stage.
	- A detailed account of transfer learning (the field that has theories useful for transferring knowledge between learning stages in CRL) [can be found here]

[this paper]: https://arxiv.org/abs/1205.4810
[this other paper]: https://www.researchgate.net/publication/221250428_Assisting_Transfer-Enabled_Machine_Learning_Algorithms_Leveraging_Human_Knowledge_for_Curriculum_Design
[can be found here]: https://www.jmlr.org/papers/volume10/taylor09a/taylor09a.pdf

--- 

## Item \*: Safe Curriculum Learning for Optimal Flight Control of Unmanned Aerial Vehicles with Uncertain System Dynamics

MSc thesis of Tijmen Pollack.

### Summary - 
