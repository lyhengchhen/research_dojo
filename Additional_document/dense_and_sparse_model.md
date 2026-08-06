# Dense (Non-sparse) and Sparse model
Dense = Non-sparse 
- **Dense model** = every parameter is used for every input. When you run a token through GPT-3, all 175 Billion parameters participate in that computation. Nothing is skipped or selectively activated. To simply put, the whole network fires every time. 
	- Computationally harder per parameter as every parameter costs compute on every forward pass, so scaling a dense model is more expensive than scaling a sparse one by the same parameter count. 
- **Sparse model** = only a subset of the total parameters are activated for any given input. the most common architecture for this is a Mixture of Experts (MoE): the model has many expert sub-networks, and a router decides which one or two experts to sent each token to. so event if the model has, a trillion total parameter, maybe only a few billion are actually used per token. 
	- they can store more knowledge while keeping inference cost closer to a smaller dense model, but they come with their own challenges, routing instability, memory/communication overhead (since the expert might spread across the device), and harder optimization dynamics. 
	- trickier to get it right
