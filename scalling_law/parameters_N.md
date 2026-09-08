# parameters_N

The N is fixed by the architecture before the training event starts: 
- we have to decide the
    - Number of layers
    - Hidden dimension size
    - Number of attention heads
    - Vocabulary size
    - Embedding dimension
    - Feed-forward dimension

then we can compute the N by summing up the size of every weight matrix in the model (roughly, for a transformer: $N \approx 12 \times n_{layer} \times d_{model}^{2}$) for the bulk of it, plus the embedding/vocab terms. 

*Example:* let's say we have a model with $n_{layers} = 32$ and $D_{model} = 4096$ so the parameter would be $N \approx 12 \times 32 \times 4096^{2} \approx 6.44 \times 10^{9} \approx 6.4B $

### Full parameter formula

$$N_{total} \approx \underbrace{12 \cdot n_{layers} \cdot d_{model}^2}_{\text{non-embedding params}} + \underbrace{2 \cdot vocab\_size \cdot d_{model}}_{\text{embedding params}}$$

- the second term is the embedding matrix (mapping token IDS -> vector) plus the unembedding matrix (mapping the final hidden_states -> vocabulary logits) and the factor of 2 is because we need both (one at the input, and one at the output), though some architecture tie these weights (use the same matrix for both), in which case it is just $\text{vocab\_size} \times d_{model}$, not double

*As a side note:* the two parameters behave differently in ways that matter for scalling laws specifically. 
- Embedding parameter is essentially just a look up table, meaning that each row is a static vector for one token, 
    - It does not perform the layered, compositional computation (attention, nonlinear transformation) that the rest of the network does. 
    According to the scalling law paper by Kaplan et al., they found that when we fit scalling laws using the total N, the curves get noisier and less clean than when we use non-embedding N only. 
    - Remember that the embedding parameter only contributes FLOPs and storage
    - It scales with vocabulary, not the model capacity (N):
        Example: $\text{vocab\_size}$ is usually a fixed design choice (driven by the tokenizer), and it does not grow with the $d_{model}$ or $n_{layers}$. so as the model get bigger, the embedding term will likely shrink fraction of total N, like for a 100B + model, embeddings might be <1% of the total parameters, whereas for a smaller model with large vocabulary size, so it can be a significant size. 


    Scalling laws: when they use N = without the embedding parameters, but in the PyTorch `model.numel()` will output the total parameters (practical number or the marketing number) including both type of parameters.  