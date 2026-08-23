# KV Cache
it is an optimization technique that make autoregressive text generation with transformers much faster. 

So conventionally, when a transformer generate text, it does so one token at a time. for each new token, the self-attention mechanism needs to compute how that token related to every token that came before it. To do this, attention uses three vectors per token: Query (Q), Key (K), and the Value (V)

Without the caching, every time we generate a new token, we have to recompute the K and V vector for all previous tokens in the sequence, and the new one, even though the K and V vectors never change.

During the Autoregressive generation, catching the Key and Value matrices from the previous tokens so we don't recompute them at each step. Then, when generating the next token, we need only:
- Compute the Q, K, V for the new token 
- Append the new K and V to the cache 
- Run attention using the new query against all cached Key and Value 

**Why the new token specifically needs all past K and V?**
	Because the whole point of attention is looking backward. And the current token, has no context, it is only becoming meaningful once it is related to everything before it. To compute that relationship for every previous token (not just the last one), the model needs each of their K vectors (to score relevance) and V vectors (to pull in content). 

**It present:**
- Speed
- **Memory trade-off:** trading the speed for memory because the cache has to store K and V vectors for every layer, every attention head, and every token generated so far. 
	- This is why long-context generation can be memory-hungry, and that is when the MQA and the GQA and cache quantization come into the play, all aiming to shrink the cache's memory down. 
### Multi-head attention 
in multi-head attention, the model does not just have one K, V, Q. It split them into multiple "heads", each looking at the sequence from a different representational subspace. 

So for a model with:
- `n_layers` transformer layers 
- `n_head` attention heads per layer
- `d_head` dimension per head

and the cache has to store a separate K and V vector per token, per head, per layer. 
	it is not just one K/V pair per token, it is `n_layers x n_heads` pair per token. 
**Note:** each of the `n_heads` query heads has its own dedicated K and V projection.

**Total cache size**
`Total cache size = 2 (K and V) × n_layers × n_heads × d_head × sequence_length × batch_size`

**For example:** for a decent-sized model that has 32 layers, 32 heads, head dimension 128 and a sequence length of 8000 tokens, that adds up to hundred of millions of numbers just for the cache of a single sequence. 
#### Multi-Query Attention (MQA)
All `n_heads` query heads share a single K and single V projection. Only 1 K/V pair per token, per layer, regardless of how many query heads exist. Cache shrinks by a factor of `n_heads`, but quality can degrade since every query head is forced to read from the same K/V representation.

**Grouped-Query Attention (GQA)**
A middle ground. The `n_heads` query heads are split into `g` groups, and each group shares one K/V pair. So there are `g` K/V pairs per token per layer (where `1 < g < n_heads`). Cache scales with `g` instead of `n_heads`, recovering most of MQA's memory savings while staying closer to full multi-head quality.

**Note:** **MHA** = 1 K/V per head, **MQA** = 1 K/V total, **GQA** = 1 K/V per group of heads.

