# Scaling Law 
### Scaling Laws for Neural Language Models
The model's performance is predictable.

Loss scale as a power-law with model size, dataset size, and the amount of compute used for training.

To simply put, whether we use the a wide-shallow or narrow-deep transformer, or whether we tweak attention heads or layer count, just keep in mind that, none of that matter, and what matters is how big is the model, how much data we have, and how much component do we spend. and aside that, everything should be the second-thought not the heuristic factors.
#### 3 Individual Power Laws
To visualize how the loss shrink as we scale up one particular resource, while assuming that the other resources are not the bottleneck. 
1. **Loss and Model Size (N)**

$$L(N) = \left(\frac{N_{c}}{N} \right)^{\alpha_{N}}, \alpha_{N} \approx 0.076$$
2. **Loss and Dataset Size (D)**
$$L(D) = \left(\frac{D_{c}}{D}\right)^{\alpha_{D}}, \alpha_{D} \approx 0.095$$
3. **Loss and Compute (C)**
$$L(C) = \left(\frac{C_{c}}{C}\right)^{\alpha_{c}}, \alpha_{C} \approx 0.050$$
**Note:** $N_{C}, D_{C}, C_{C}$ are empirically fit constant, meaning that they are just used as reference scales to calibrate the curve, not something conceptually meaningful. 

#### Combined Equation
$$L(N, D) = E + \frac{A}{N^{\alpha}} + \frac{B}{D^{\beta}}$$
- **1st term:** describe as the irreducible loss which corresponds to the entropy of natural text.
- **2nd term:** undermine the fact that a model with N parameters underperforms this ideal generative process. 
- **3rd term:** present that we train it on a finite sample of the data and do not train the model to convergence. 
	- thus, as both N and D go to infinity, loss will approach the E. 

Larger model are sample-efficient, such that optimally compute-efficient training involves training very large models on a relatively modest amount of data and stopping significantly before the convergence.

So, we would rather train a huge model on comparatively little data and stopping early, rather than training a smaller model to convergence. 

- Kaplan's exponents implied "go big on parameters, use relatively little data"
- Chinchilla (2022) later corrected this, showing N and D should scale **equally**, which is reshaping how modern LLMs are trained