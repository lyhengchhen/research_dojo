# InstructGPT
**Problem poses by the GPT3:** it is trained to predict the next word which is good at generating plausible text, but it is not necessary good at doing what the user actually want as it might output untruthful, unhelpful, or produce toxic/biased content...

There are 3 important step to get the InstructGPT:
- Supervised Fine-tuning (SFT): Human labelers write high-quality example repsonses to a range of prompts. and this give the model a decent starting policy that already looks more like an assistant.
- Reward Model (RM): for a given prompt, the SFT model generate several different responses. And the human labelers rank these repsonses from best to worst. and a separate model (initialized from the SFT model, with a scalar output head) is trained to predict these rankings. aka it is learning how to score the response according to the human preference. 
- Reinforcement Learning via PPO: the SFT model then further optimized using the Reinforcement learning (PPO), where the reward signal comes from the reward model (RM) in step 2. The policy generates a responses, the RM scores it, and PPO updates the policy to produce higher-scoring responses. 
    A KL-divergence penalty against the original SFT model is included to prevent the policy from drifting too far from reward model or degenerating into gibberish. 

    KL Divergence (Kullback-Leibler) measures how different one probability distribution is from another. In this case, we have two policies, so we are trying to compare the old policy and the new policy, 
    - Small KL divergence = small shift in probability rate -> safe update 
    - Big KL divergence = big shift in probability rate -> safe update
    we would prefer the small KV divergence, becuase we dont want the policy to change its mind so fast, even if the gradient updates a lot. To simply, we want a smooth learning curve. 

and the whole pipeline is RLHF

The InstructGPT presented astonishing result: 
- It is prefered over the ChatGPT3
- Better truthfulness and less toxicity
- The model generalized reasonably well to instruction outside the distribution it was trained on, including non-english prompts, coding, albeit most of the labeled data was english-forced. 