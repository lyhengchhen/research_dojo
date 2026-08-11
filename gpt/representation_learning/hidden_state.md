# hidden_state
It is the tensor of values at a specific layer, for a specific input, at a specific point in the forward pass. = it is the content that the model computes
- Shape: (batch_size, sequence_length, d_model), also the hidden_size determine one dimension of the hidden_state
- every layer produces a new hidden state as its output, which becomes the input to the next layer
- and remember that these values change constantly, they depend on the actual input tokens, and are different for every forward pass. A hidden state for the sentence "the cat is running" is different from the hidden state of the sentence "the dog is running"
- plus, sometimes, it is also called an Activation = the values are literally the output of applying weights + nonlinearities to the previous layer's output. 