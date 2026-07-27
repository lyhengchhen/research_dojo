# GPT 2
## Language Models are Unsupervised Multitask Learners

The LLMs that are trained simply to predict the next word on a huge, diverse corpus of internet text will learn to do many different NLP tasks (translation, summarization, question answering, reading comprehension) without ever being explicitly trained on labeled data for those tasks. which is why it is called "Unsupervised Multitask Learners"

### Architecture
- The GPT 2 is a decoder-only transformer (masked self attention layer is injected beneath), essentially scaled-up version of the GPT 1
- it is autoregressive since it predict the next tokens given all previous tokens. 
- Despite being called scaled-up version of the GPT1, compared to the GPT1, the researchers just made it bigger and tweaked details liker Layer Norm placement, and increased the context size (from 512 to 1024 tokens)
- released in 4 sizes, the largest is 1.5 B parameters, which at the time was considered very large. but as of now, there are 1T parameters model out there competing for the spotlight. 

### Training data
- OpenAI built their own dataset called WebText (40GB of text scraped from outbound links on reddit posts with atleast 3 karma (used as a crude quality filter)), and by giving that random, broad, high-quality,.., makes the model absorb many more implicit tasks that GPT1 normally cannot do. 

### Zero-shot task performance
They adopted this technique to see whether GPT 2 could do useful things without being specifically trained to do them. 

- They did not finetune it on any task-specific data, but what they did was that typing in prompts in the natural language and saw what came out. For example, to get it to summarize a passage, they'd just paste the passage and add "TL:DR" at the end, then see what text the model generated next. 
    - Note: TL:DR = Too long:Didnot Read, it is an internet slang 

Here are what have encountered: 
- It set a new SOTA on several language modeling benchmarks (like LAMBADAM, PennTreebank, WikiText-2) in a zero-shot setting. 
- It could do the rudimentary reading comprehension, translation, and summarization, though generally worse than dedicated supervised systems of the time. 
- Performance scaled smoothly with the model size (The positive correlation between the performance and the model size)
