# Byte Pair Encoding
Byte Pair Encoding is a text tokenization technique in NLP. It breaks down the

BPE is a data compression and subword tokenization algorithm. 

### Process 
- **Initialization:** break all the words down into individual characters or raw bytes. Add an end-of-word symbol if needed
- **Counting:** scan the text corpus and count the frequency of all adjacent pairs of symbols
- **Merging:** find the most frequent pair and merge them into a brand-new single symbol. 
- **Repeating:** add the new symbol to the vocabulary and repeat the counting and merging steps until reaching a target vocabulary size. 
### Why it is used in AI 
- **No unknown words:** rare or unfamiliar words are split into smaller subwords or base characters, meaning the model never fails on new text.
- **efficiency:** common words stay as single tokens while rare words are broken down, keeping text sequence short and computationally Manageable
- **Root meaning:** it helps language models like GPT-2, GPT-4m and Llama recognize word parts, prefixes, and suffixes across different variation of a word

Use it when: 
- Character-level tokenization (too granular, sequences get very long)
- Word-level tokenization (vocabulary explodes, cannot handle rare or unseen words)

It can represent/encode any word using its base units, even ones not in its training data. i.e., from Hellooo to Hello 
- By decomposing it into known subword piecese.

After splitting into subword chunks, the text is not stored as text internally, the tokenizer has a fixed vocabulary (a lookup table) mapping every known chunk into the integer ID. 
**Example:** ["un", "happi", "ness"] -> [403, 235, 4656] (a list of numbers...)

Then for the embedding part, the 403 will then be converted or mapped into the 4096 Dimension vector, passing through the transformer layer (attention + Feedforward blocks) sequentially, figuring out how each token relate to other in the sequence and returning the representation layer by layer. Outputing the probability distribition over the entire vocabulary. then pick the most likely one, then append it, and repeat the process. Eat->Process->Output->Repeat!

```
Text → [BPE tokenizer] → Token IDs → [Embedding lookup] → Vectors 
→ [Transformer layers] → Output probabilities → Next Token ID 
→ [Decode back to text]
```