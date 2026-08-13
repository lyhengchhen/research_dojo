# Weigth
### how weigtha are applied through every step

Weights get applied at every single layer, every time data passes through. 

### Forward pass
1. Input --> Embedding
Token_id = [464, 6415, 3332] then the embedding matrix (a weight) is applied by looking up the row for each token ID and returning that row as a vector. 
$$h_{0} = Embedding(x)$$

**Shape:** Before the embedding [batch_size, seq_lenght] and after the embedding, each token ID gets replaced by its corresponding row in the embedding matrix and the shape becomes [batch_size, seq_length, d_model]

**2. In each transformer layer**
for each of the $n_{layers}$ layersm the hidden state gets multiplied by several weight matrices in sequences. 
- **Attention:** so the incoming hidden state $h$ is multiplied by three separate weight matrices to produce the Q, K, V
$$Q = hW_{Q}, K = hW_{K}, V = hW_{V}$$

and after computing attention scores and weighting the values, the result passes through one more weight matrix (the output projection that we normally face when doing the gpt 1-3 lab)

$$AttnOut = Attention(Q, K, V)W_{o}$$

- **Feed-forward network:** the result gets mutliplied by two more weight matrices, with a nonlinearity in between $$FFOut = \sigma (hW_{1})W_{2}$$
    - Projected the d_model to 4 x d_model
    - Projected the 4 x d_model to d_model  

Thus, in a single transformer layer, weights are applied 6 times ($W_{Q}, W_{K}, W_{V}, W_{o}, W_{1}, W_{2}$) and this is exactly where the $12 \cdot d_{model}^{2}$ parameter formula comes from (roughly 4 attention matrices + 2 FF matrices, each contributing $d_{model}^{2}$) or multiple of it

and this repeats for every layer: the output hidden state of layer 1 becomes the input to layer 2, which applies its own seperate set of weigths (different values, same structure) and so on through all the n_layers 

**3. Final layer**
this is the layer where we do the unembedding (last weight application). after the last transformer layer, the final hidden state is multiplied by the unembedding matrix to produce logits (one score per vocabulary word): $logits = h_{final} \cdot W_{unembeded} \text{ Untie weights}$ 

**As a side note:** in the parameter calculation formula, we assume that the $W_{embed}$ and $W_{unembed}$ are separate, untied matrices, so we need two full $\text{vocab\_size} \times \text{d\_model}$ matrices, and multiply by 2. 
But if we assume or use the tied weight, it means that the $W_{unembed} = W_{embed}^{T}$

and the untie weight is now the default at frontier scale. 