# Weigth_tying
Weigth_tying is the operation that we use the same matrix for two different jobs in the model, instead of learning two separate matrices.

and the two matrices involved: 
1. Token embedding matrix
(`token_embedding.weigth`)
- Shape: `[vocab_size, hidden_size]`
- Job: token ID -> vector. Given a token like "cat" (say ID 42), look up row 42 to get its `hidden_size`-dimensional vector. 

2. lm_head matrix (`lm_head.weight`)
- Shape: `[vocab_size, hidden_size]`
- Job: vector -> scores per token. Given that the model's final hidden state ( a `hidden_size`-dim vector), multiply it by this matrix to get `vocab_size` logits, one score per token in the vocabulary.

So in the embedding matrix, think about what each row of the embedding matrix represents row 42, which is "the vector representing token 42". and for the `lm_head `, to score how well the model's output matches token 42, it computes the dot product of the hidden state with the row 42 of its matrix. Thus these two present conceptually the same idea. 