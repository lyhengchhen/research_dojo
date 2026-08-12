# Policy 
- Policy: a decision-making rule an agent uses to choose actions. Formally, it is a function (usually written in $\pi$) that indicates a state (and a state means what the agent currently observes about the environment) to an actions (what the agent does next)
$$\pi(a|s)$$
**Note:** this notation means: "the probability of taking action a, given that the agent is in the state s"

### Type of policy: 
- Deterministic policy: state -> single specific action. E.g., if the light is green, continue
- Stochastic policy: state -> probabilistic distribution over actions. E.g., if the light is red, 95% chance stop, 5% chance go. 
    - this is beneficial as the agent does not always do the exact same thing, so it can use this advantage to explore better strategies. 

Generally, in deep RL, the policy is a nn, and it takes the state as input and outputs a probability distribution over actions. 
- Discret actions (like move left/right/up/down), it outputs a probability for each possible action (via softmax)
- Continuous actions (like applying this much torque), it outputs the parameters of a distribution 
**As a side note:** the specific numbers that define the shape of a probability distribution. It is a family of curves controlled by a small number of values. For a Gaussian Distribution, this family is described by just two numbers: 
    - $\mu$ = the mean: where the distribution is centered (aka the most likely value)
    - $\sigma$ = the standard deviation: how spread out/uncertain the distribution is. 
        - For example: we input the state (like the "joint angles", "sensor readings'), then the network computes those two numbers,outputing and injecting $\mu$ and $\sigma$ into a Gaussian distribution, then randomly draw an action from it. 