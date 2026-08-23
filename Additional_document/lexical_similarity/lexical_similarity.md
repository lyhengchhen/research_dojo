# Lexical Similarity

**Lexical similarity:** measures how much two texts overlap. You can do this by first breaking each text into smaller tokens. In its simplest form, lexical similarity can be measured by counting how many tokens two texts have in common. 
		- **Example:** 
			- My cats eat the mice 
			- Cats and mice fight all the time 
		- One way to measure lexical similarity is *approximate string matching*, known *colloquially* as *fuzzy matching.* It measures the similarity between two texts by **counting how many edits it's need to convert from one text to another**, a number called *edit distance*. 
			- **Example:** 
				- Deletion: "brad" --> "bad"
				- Insertion: "bad" --> "bard"
				- Substitution: "bad" --> "bed"
		- Another way to measure it, *n-gram similarity*, measured based on the overlapping of sequences of tokens, n-gram, instead of single token.
		- **Common metric:** BLEU, ROUGE, METEOR++, TER, and CIDEr. They differ in how the overlapping is calculate