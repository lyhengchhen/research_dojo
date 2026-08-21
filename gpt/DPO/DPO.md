# DPO
### Direct Preference Optimization 

Training a reward model than using PPO is a tedious task and thus time-consuming and not energy efficient. So we can directly train the model using the pair preference (rejected and chosen answer in the prompt).

**Problem with RLHF pipeline:**
- SFT: train the model on demonstration
- Reward model training: collect the human preference outputs and then train a separate RM model on those data/preference
- PPO: use the algorithm PPO to fine-tune the model to maximize the RM score while penalizing it with KL divergence (keep the model to stay close to the original model)


In the DPO: 
- Reference model: it is used as baseline, and normally the original model that has been through the SFT process 
- PPO model: a model that is initially the SFT model ($\pi_{\theta} = \pi_{ref}$), later optimized/trained ($\pi_{\theta} \neq \pi_{ref}$). and the reason that we need the Reference model is simply because we want to use it as a baseline for the PPO model to make prefered answers more likely while remaining reasonably close to the Reference model. $$\frac{\pi_{\theta}(y|x)}{\pi_{ref}(y|x)}$$
it is a basic comparision: Reference model and PPO model

### Math equation for DPO Loss equation
$$
\begin{aligned}
&\text{For the chosen answer } y_w \text{, we have:} \\[4pt]
&r(x, y_w) \approx \beta \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} \\[10pt]
&\text{For the rejected answer } y_l \text{, we have:} \\[4pt]
&r(x, y_l) \approx \beta \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)} \\[10pt]
&\text{The reward difference becomes:} \\[4pt]
&r(x, y_w) - r(x, y_l) = \beta \left[ \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)} \right] \\[10pt]
&\text{Put this into the Bradley--Terry model:} \\[4pt]
&P(y_w \succ y_l \mid x) = \sigma\!\left( \beta \left[ \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)} \right] \right) \\[10pt]
&\text{DPO then minimizes the negative log-likelihood:} \\[4pt]
&\boxed{
\mathcal{L}_{DPO} = -\log \sigma\!\left( \beta \left[ \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)} \right] \right)
}
\end{aligned}
$$