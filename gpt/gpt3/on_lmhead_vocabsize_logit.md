### Language Model Head
lm_head is a final layer that turns the model's internal representation back into vocabulary predictions.
- qa_head (question-answering)
- classification_head (text classifier)

### Vocab Size
vocab_size is just the number of distinct tokens the model knows about. each token could be a whole word, part of a word, punctuation,...
**For example:**
- "Cat" -> migth be one token
- "Unbelievable" -> "unn", "believ", "able"
- "!" -> probably its own token

**Note:** every token has an ID, an integer from 0 to vocab_size - 1. The embedding layer is basically a lookup table: vocab_size rows, one hidden_size -dimension vector per token.

### Logit
Logit is a raw, unnormalized score before it's been turned into probability. 
 
When the lm_head processes the model's hidden state for a position, it outputs a vector of length vocab_size. Each entry in that vector is the logit for one specific token, which is a number saying "how much the model favors this token as the next one," but not yet a valid probability (it can ve negative, greater than 1, whatever)

Example: let say we have vocab_size = 5 with token ["the", "cat", "sat", "dog", "ran"]. so the model might output the logits: [2.1, 3.4, 5.6, 2.3, -1.2]

Higher = more likely, but these numbers don't sum to 1 and can be negative, so they not probability yet. 

and to turn them into probabilities, we can apply the one and only Softmax 

    this exponentiates every value (making them all positive) and normalizes so they sum to 1. so the logits above might become something like: [0.06, 0.55, 0.007, 0.37, 0.002]

    and the "cat" (0.55) is clearly the model's top pick for the next token

    and why we still need the logit 
    the answer is it is mainly used for numerical stability and training convenience like the loss function (cross-entropy), which is typically computed directly from logits, and framework like the PyTorch (nn.CrossEntropyLoss) expect raw logits and do the softmax internally in a numerically stable way, rather than doing it manually. 


  token -> embeddings -> transformer blocks -> ln3 -> lm_head -> logits -> apply the softmax to turn it into the probabilities.
  
   