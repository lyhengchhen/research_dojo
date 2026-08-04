# NLI (Textual Entailment)

it is a NLP task where the model is given two sentences (a premise and a hypothesis) and the model has to decide how they logically relate to each other. 
### Three possible labels
1. **Entailment:** The hypothesis logially follows from the premise (If the premise is true, the hypothesis must also be true)
	- Premise: "A man is playing a guitar on the stage"
	- Hypothesis: "A person is performing"
		- So this is the Entailment
2. **Contradiction:** the hypothesis directly conflicts with the premise (they cannot both be true)
	- Premise: "A man is playing guitar on stage"
	- Hypothesis: "No one is on the stage"
		- So this is the contradiction
3. **Neutral:** the hypothesis might be true or false, the premise does not provide enough information to know either way. 
	- Premise: "A man is playing a guitar on the stage"
	- Hypothesis: "the man is playing at a rock concert"
		- So this is a neutral, meaning that it could be true but the premise does not confirm it, it could be a jazz bar, a rehearsal, a busking gig...

The NLI is considered hard because it demand reasoning about the meaning like to
- to understand the implication and logical consequence not just the word overlap
- to handling negation correctly ("no one is on stage" contradicts, does not just differ in wording)
- Common-sense knowledge (e.g., knowing that "playing guitar" implies "performing music")
- to resolve ambiguity and figuring out what's actually stated vs what's merely plausible. 

**Common benchmarks**
- **SNLI** (Stanford Natural Language Inference)
- **MNLI** (Multi-Genre NLI)
- **ANLI** (Adversarial NLI) specifically designed to be hard for models by using adversarial, human-in-the-loop example generation