# Proximal Policy Optimization (PPO)
### Policy gradient method 
It is a reinforcement learning algorithm, used to nudge the model's behavior toward what the reward model likes or prefers with controlled steps. 

- Policy: a decision-making rule an agent uses to choose actions. Formally, it is a function (usually written in $\pi$) that indicates a state (and a state means what the agent currently observes about the environment) to an actions (what the agent does next)
$$\pi(a|s)$$
**Note:** this notation means: "the probability of taking action a, given that the agent is in the state s"

- Reward: the score given by the reward model
- Goal: update the policy's parameters so it genertes responses that get higher reward (the response that the human prefer)

The relationship between the Gradient Descent and the PPO 
- Gradient Descent: for a given function, the function that we want to minimize the loss, nudge the parameter toward the direction that could reduce the loss. 
- PPO: it is used to decide what the loss function (clipped objective) should look like so when we run the gradient descent on it, the agent get better at its task without risking the training to collapse or spiralling out of control