### LLaMA 
The game changer!

**Goal:** Optimize the model for Inference efficiency.


#### Architecture: 
- RMSNorm ([[Pre-norm]]) instead of LayerNorm, Root mean square Norm which is computationally simply since it does not use the mean-centering step and only rescale by the its statistic. 
- SwiGLU (Shazeer's GLU paper) instead of ReLU/GELU, which is good for budget hardware, LLaMA adjust the FFN's hidden dimension to only 2/3 compared to x4 in the original. 
- RoPE (RoFormer's Positional Embedding) instead of learned/absolute positional embeddings, by rotating the query/key vectors as a function of their position, which is then applied at each attention layer. 

these three show up to every model architecture since then (Qwen, Mistral,....) so they are the game changer. 

besides everything that is changed, the
- Multihead self-attention (no sparse/linear attention trick)
- Decoder only, causal mask 
- AdamW 
- Byte-pair encoding tokenizer 

#### The training-compute tradeoff
so this paper has a lot to do with the scalling law by Chinchilla et al., guiding us to understand why over-training a small model past Chinchilla-optimal makes sense when the inference cost is greater than training cost. so scalling law infer that for a 10B model, we have to train it on 200B token, then stop it. but for this paper, they just ignore that stopping point, they took a smaller model 7B and then train it iteratively on the way to 1 trilion tokens, and surprisingly the model kept getting better. 

#### Data Curation
It build upon the public dataset only. and it also set up the floor for RAG and later synthetix data/fine tuning discussions (LoRA/QLoRA) 

