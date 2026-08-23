# BLEU 

**BLEU** = **Bilingual Evaluation Understudy** = is a widely used algorithm metric for evaluating the quality of the text which has been translated by machines from one natural language to another. 

By comparing a machine's translation to one or more human-created reference translations, it measures how closely the outputs match, scoring from 0 (no overlapping) to 1 (perfect match)

**How it works:** 
- **n-gram matching:** Instead of evaluating the whole sentence at once, BLEU evaluates how many n-gram (contiguous sequences of words) in the machine translation appear in the reference translations. 
- **Modified Precision:** to stop machines from gaming the system by repeating the same word, the algorithm caps the count of each word match based on its maximum occurrence in the reference sentence. 
- **Brevity Penalty:** BLEU penalizes the translations that are significantly shorter than the human references to prevent the model from only outputting a few high-precision, incomplete words. 

**Score interpreting:** 
- **(0-100):** the higher the better the translation. 
	But the BLEU scores are not absolute indicators of human readability or grammar correctness. 

### Final Formula

![{\displaystyle BLEU_{w}({\hat {S}};S):=BP({\hat {S}};S)\cdot \exp \left(\sum _{n=1}^{\infty }w_{n}\ln p_{n}({\hat {S}};S)\right)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/f399e95d719741400c2242ff9556f514c5e2eff0)
