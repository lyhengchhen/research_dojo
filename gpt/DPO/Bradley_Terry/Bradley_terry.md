# Bradley-Terry
It is a probability model that predict the outcome of pairwise comparisons. 
It assigns a latent "strength" score to each item, and the chance of one item beating another depends on the ratio of their score. 

In AI development, it is used to understand the human preferences (used in RLHF, DPO)

### How it is utilized in AI training
- Preference Pairs: Humans or AI judges evaluate two model outputs ($y_{1}$ and $y_{2}$) for a single prompt ($x$)
- Labeling: One output is labeled as "preferred" = ($y_{w}$) and the other as "disfavored" = ($y_{l}$)
- Reward modeling: the nn acts as a reward function r(x,y), outputting the scalar value
- Probability mapping: the probability that the reward model choose $y_{w}$ over $y_{l}$ follows the equation:$$P(y_{w} > y_{l}|X) = \frac{\exp(r(x, y_{w}))}{exp(r(x, y_{w}))+\exp(r(x,y_{l}))}$$

the reasons that it is a default for AI training are simply because: 
- Loss optimization: LLMs use this specific probability function inside a binary cross-entropy loss loop to maximize the gap between the good and bad responses. 
- Elo ratings: This math allows the chatbots on leaderboards like LMSYS ChatBot Arena to get dynamic Elo scores based on side-by-side user votes. 
