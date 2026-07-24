# GPT 1 
## Improving Language Understanding by GPT 

**Problem:** before the GPT1, most NLP modela were trained from scratch on labeled data for one specific task (sentiment analysis, entailment,...). and Labeled data is scarce and expensive, while the raw and unlabeled data text is abundant. 

GPT 1 has two main parts: 
- Pretrain model on unlabeled data (unsupervised learning): train a model (predict the next word, given everything before it) on a large unlabeled corpus. Used: BooksCorpus = 7000 unpublished books, chosen specifically because it contains long strecthes of contiguous text, letting the model learn long-range dependencies rather than shuffled, sentence-level text that other approaches like ELMo used. 
- Finetune the model on label and task-specific data: taking the pre-trained model, add a small task-specific output layer on top, and continue training on a labeled dataset for the aimed task. and almost all the pre-trained parameter/weight remain unchanged or modified. 

Key take away: 
- rather than redesigning the architecture per task, the paper reformat every task's input into a single flat token sequence the same transformer can process, then adds one linear layer + softmax on top. 
    **Example:** for multiple-choice questions, each answer choice is concatenated with the context as a separate sequence, all fed through the same model, and the outputs are combined and passed through a final linear layer that outputs a probability for each choice being correct. Special delimiter tokens mark where different pieces of input (question, answer, etc.) begin and end.


 