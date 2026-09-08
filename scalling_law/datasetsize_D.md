# datasetsize_D

The D is how many token we actually feed the model during the training (but not the size of the dataset), but the numbers of tokens seen, including repeats if we loop over the training data multiple times.

- $D = \text{batch\_size} \times \text{sequence\_length} \times \text{number\_of\_training\_steps}$

Thus we can control the D by deciding how many optimizer steps to run (or equivalently, when to stop training)

Example: let's say we have a training set up of $\text{batch\_size} = 1024 (\text{sequence per step})$, $\text{sequence\_length} = 2048 (\text{token per sequence})$, $\text{training\_steps} = 300000$, thus $D = 1024 \times 2048 \times 300000 = 6.29 \times 10^{11} \approx 630 \text{ billion tokens}$


And here we are, the step to determine the Chinchilla ratio. 

so with $N \approx 6.4 \times 10^{9}$ and $D \approx 6.29 \times 10^{11}$ We can simply just do this: $\frac{D}{N} = \frac{6.29 \times 10^{11}}{ 6.44 \times 10^{9}} \approx 98$

so that is the ~98 tokens per parameter, which is well above the ~20:1 optimal ratio. and this suggests a hypothetical run, the overtrained relative to pure loss-optimality 
so if we want to hit the 20:1 ratio just solve the equation for the $D$  

$D_{optimal} = 20 \times N = 20 \times 6.4 \times 10^{9} = 1.28 \times 10^{11} \text{tokens} \approx 128 B$
then work the other way around to figure out the training_steps 
