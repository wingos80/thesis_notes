
- Date & time:  27/03/2024
- Tag: #literature
- Project:

---

## Context

Came across this when first writing up the IDHP augmentations document, but realised this is more useful than expected since it addresses incompatibility issues between neural networks and eligibility traces, specifically the issue of "diverging gradients", and proposes a solution.

## Resource

The paper: https://doi.org/10.1016/j.robot.2021.104019

Need to read in more detail, but from preliminary reading this paper proposes to use a distance metric between eligibility trace and current gradient to decide what gradients to use for weight updates, thus solving the "gradient divergence issue".