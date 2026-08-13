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

### How the PPO works
1. Collect the experience: run the current policy in the environment, gathering trajectories (states, actions, rewards)
2. Compute the advantages: for each action taken, estimate the "advantages A", how much better this action was compared to the average action in that state. and this tells us the direction to push the policy. 
3. Compute the probability ratio: 
$$r(\theta) = \frac{\pi_{\theta}(a | s)}{\pi_{\theta_{old}}(a | s)}$$
this compares how likely the new policy is to take this action versus the old policy that actually collected the data. 
- r = 1 → new policy agrees exactly with old policy on this action
- r = 2 → new policy is now twice as likely to take this action
- r = 0.5 → new policy is now half as likely to take this action
4. Clip the objective: PPO's signature trick
$$L^{CLIP}(\theta) = \mathbb{E}\left[\min\left(r(\theta) A,\ \text{clip}(r(\theta), 1-\epsilon, 1+\epsilon)\, A\right)\right]$$
Where the $\epsilon  = 0.1, -0.2$
- If an action was good (A > 0), the objective wants to increase its probability — but the clip caps how much credit you can take once the ratio exceeds 1+ε. No incentive to push the policy arbitrarily far in one step.
- If an action was bad (A < 0), the objective wants to decrease its probability — same capping in the other direction, floored at 1−ε.
    - **As a side note:** A = Advantages = it is the signal that tells the update which direction to push the policy.
    $$A(s, a) = Q(s, a) - V(s)$$
    - $Q(s, a)$: the expected total future reward if you take action a in state s
    - $V(s)$: the expected total future reward if you just act averagely in state s (averaged over all actions the current policy might take)
        - A > 0 -> this action did better than the average action in that state -> increase its probability
        - A < 0 -> this action did worse than average -> decrease its probability. 
        - A $\approx$ 0 -> this action was about as good as the average -> so no need to tweak the probability.

- Taking the min of the clipped and unclipped versions makes this a pessimistic (conservative) bound as it never lets the update look better than it conservatively should.
5. Update and repeat: we can even use the same batch of data for several epochs of gradient updates (since the clipping keeps thing safe), improving sample efficient, then collect fresh data with the updated policy.