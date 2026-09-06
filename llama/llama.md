### LLaMA 
The game changer!

#### Architecture: 
- RMSNorm ([[Pre-norm]]) instead of LayerNorm
- SwiGLU instead of ReLU/GELU
- RoPE instead of learned/absolute positional embeddings

these three show up to every model architecture since then (Qwen, Mistral,....) so

#### The training-compute tradeoff
so this paper has a lot to do with the scalling law by Chinchilla et al., guiding us to understand why over-training a small model past Chinchilla-optimal makes sense when the inference cost is greater than training cost

#### Data Curation
It build upon the public dataset only. and it also set up the floor for RAG and later synthetix data/fine tuning discussions (LoRA/QLoRA) 


to learn 
- compute-optimal point 
- inference-optimal point

