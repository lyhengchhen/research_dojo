## More on the Pre-norm and Post-norm 
It describe where the Layer Norm is placed in the transformer block relative to the sub-layer (attention and FFN) and skip/residual connection (think of it as a single continuous wire running end-to-end through the whole network)
(a) **Post-norm:** the original transformer.
$$\text{Output} = \text{LayerNorm}(\text{Sub-layer}(x)+x)$$
In post-norm, gradients have to pass _through_ the LayerNorm on every residual path, which can cause gradients to shrink or explode in deep networks, especially early in training. Post-norm transformers typically need careful learning-rate warmup to avoid divergence.
- It need the learning rate warmth up, harder for the deep net
(b) **Pre-norm:** the modified/modernize transformer.
$$\text{Output} = x+\text{Sub-layer}(\text{LayerNorm}(x))$$
In pre-norm, the residual stream is a clean, unimpeded highway so the x flows straight through addition without being renormalized. This means gradients can flow directly back through the residual connections almost unchanged, making very deep networks much easier to train and less sensitive to warmup schedules.
- It is very stable, scale 