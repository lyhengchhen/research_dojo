# GPT 3
## Language Model is a few-shot Learner

**Problem:** Pre-training the language model on a large corpus of text followed by finetuning on a specific task, it is efficient but still requires task-specific fine-tuning datasets of thousands or tens of thousands of examples. With context given above, we can distill an idea out of that, we want the model to be specifically capable of doing what human can do like human can literally perform a new language task from only a few examples or from a simple instruction. Thus, if we make a language model large enough, we do not need to fine-tuning at all. instead, we can just show the model a few examples of a task in the prompt itself at inference time, no gradiennt updates, no weight changes. and this is called **In-Context Learning**

**GPT3:** 
- Autoregressive language model with 175 billion parameter = 10 times larger than the previous non-sparse model
- a decoder-only transformer
- Trained on a huge mix of internet text:  a filtered Common Crawl (web scrapping), WebText2, Books1/Books2, and wikipedia.
- Trained on 8 different model size (from 125 M to 175 Billions parameters) specifically to study how performances scales with the model size. 
- For all tasks, it is applied without any gradient updates or fine-tuning, with tasks and few-shot demonstrations specified purely via text interaction with the model. 

**As a side note:** Dense = Non-sparse 
- **Dense model** = every parameter is used for every input. When you run a token through GPT-3, all 175 Billion parameters participate in that computation. Nothing is skipped or selectively activated. To simply put, the whole network fires every time. 
	- Computationally harder per parameter as every parameter costs compute on every forward pass, so scaling a dense model is more expensive than scaling a sparse one by the same parameter count. 
- **Sparse model** = only a subset of the total parameters are activated for any given input. the most common architecture for this is a Mixture of Experts (MoE): the model has many expert sub-networks, and a router decides which one or two experts to sent each token to. so event if the model has, a trillion total parameter, maybe only a few billion are actually used per token. 
	- they can store more knowledge while keeping inference cost closer to a smaller dense model, but they come with their own challenges, routing instability, memory/communication overhead (since the expert might spread across the device), and harder optimization dynamics. 
	- trickier to get it right

### Three modes of Learning
- Zero-shot: give the model just a task description, no examples (i.g., Translate English to French)
- One-shot: give one examples, then ask it to do the task
- Few-shot: Give a handful (typically 10-100) examples in the prompt, then ask it to do the task.
 
**Note:** All of this happens purely through the prompt, plus the model's weight never change. 

### Key findings
1. **Scale drives few-shot ability:** Larger models get disproportionately better at in-context learning. Small models barely benefits from being shown examples, GPT-3 175 B benefit a lot. 
2. **Competitive with fine-tuning models:** on many tasks (translation, question-answering, cloze tasks, some commonsense reasoning), a few-shot GPT-3 matched or beat prior SOTA fine-tuned models without ever trained on task-specific data
3. **Still struggling with:** tasks requiring multi-step reasoning, some reading comprehension benchmarks, and tasks like natural language inference (NLI) or Textual Entailment. Also showed weaknesses in tasks needing precise bidirectional context (since it is is a left-to-right model)
4. **Emergent-ish behavior:** some abilities like doing the basic arithmetic, or unscrambling words only reliably appreared at the largest scale, which is a hint at what later became known as "Emergent Capabilities"

### Contributions and Concerns
- Data contamination analysis: They specifically checked whether test sets leaked into the training data (a real risk with a Common Crawl-based corpus).
- Bias and fairness: analyzing gender, race, and religion biases in the GPT3's output
- Misuse potential: early discussion of risks like generating misinformation, spam, or phising content at scale, which is foreshadowing a lot of the AI safety conversation that followed
- Energy and compute costs: a rare-for-the-time explicit/discussion of the compute and environmental cost of training such large models. 

In a nutshell, this paper is a foundation of the term "Prompt Engineering" era. and it also reframed language models from task-specific tools you fine-tune to general-purpose reasoners you talk to. It cemented the "scaling laws" mindset, which is the idea that just making models bigger (with enough data and compute) reliably makes them more capable, a philosophy that shaped GPT-4 and much of the field since.