## **Course Task: Tokenization Efficiency Benchmark**

### **Objective**
The goal of this assignment is to analyze how different tokenization strategies influence the performance and behavior of a language model.  
Students will implement and compare **three tokenizers** and evaluate their effects on model efficiency and text representation quality.

---

### **Tokenizers to Use**
Each student must prepare **three tokenizers**:

1. **Pre-trained tokenizer**  
   - Use any existing trained tokenizer, e.g., **GPT-2’s Byte-Pair Encoding (BPE)** tokenizer from Hugging Face.  

2. **Whitespace-based tokenizer (custom implementation)**  
   - Implement a tokenizer that splits text on whitespace and punctuation.  
   - Treat punctuation marks (commas, periods, question marks, etc.) as **separate tokens**.  
   - Use a **fixed-size dictionary** (e.g., top *N* most frequent words, where N is the same as in the first tokenizer).
   - Introduce a special **`<UNK>` token** for words not in the dictionary (out-of-vocabulary items).  

3. **SentencePiece tokenizer**  
   - Train a **SentencePiece** model (BPE or Unigram algorithm).  
   - Use your own training corpus and specify the vocabulary size explicitly (where N is the same as in the first tokenizer).  

---

### **Model Training**
- Train **three identical language models** (same architecture, hyperparameters, and data) — one for each tokenizer.  
- The architecture can be an Transformer model of your choice (reuse the setup from the previous assignment if possible).  
- Ensure that only the tokenizer differs across experiments.

---

### **Evaluation Metrics**
Each trained model must be evaluated using:

1. **Perplexity**
   - Compute both:
     - **Word-level perplexity**, and  
     - **Character-level perplexity**  
   - **Do not report token-level perplexity**, as it is not comparable across tokenizers.

2. **OOV (Out-Of-Vocabulary) statistics**
   - Report the number and percentage of **OOV words** for the whitespace tokenizer.  

3. **Efficiency metrics**
   - Include **training and inference time**, or tokenizer throughput (tokens per second).
   - Average tokens per word for at least 1MB of text (the tested text has to be different than the text used to train the tokenizer).
   - Number of words directly present in the dictionary

---

### **Qualitative Analysis**
Provide at least **three sample texts** (each at least **30 words long**) and:
- Show the tokenized outputs for all three tokenizers.  
- Compare:
  - The **number of tokens per word** (average)  
  - The **percentage of words encoded directly** (without being split or replaced by `<UNK>`)  
- Include short commentary on how tokenization granularity and OOV handling differ.

---

### **Deliverables**
Your submission should include:

1. **Code**
   - Implementations and configurations of the three tokenizers.  
   - Training and evaluation scripts for the language model.  

2. **Report**
   - Description of:
     - The tokenizers and vocabulary sizes  
     - Model architecture and hyperparameters
     - Hardware specification
   - Quantitative results:
     - Word- and character-level perplexities  
     - OOV statistics  
     - Tokenization statistics (tokens/word, encoded words)  
   - Qualitative examples (tokenization comparisons)  
   - Discussion of trade-offs and observations

---

### **Study Material**
To better understand subword tokenization principles, review:
- **Kudo, Taku.** “SentencePiece: A simple and language independent subword tokenizer and detokenizer for Neural Text Processing.” *EMNLP 2018*  
- **Sennrich, Haddow, and Birch.** “Neural Machine Translation of Rare Words with Subword Units.” *ACL 2016*
- [Comparing perplexities](https://sjmielke.com/comparing-perplexities.htm) by Sabrina Mielke.

---

### **Summary**
- Implement **three tokenizers**: pretrained (e.g. GPT-2), whitespace-based (custom), and SentencePiece-based.  
- Train **three identical models**, one per tokenizer.  
- Compare using **word and character perplexity**.  
- Analyze **OOV rates**, **tokenization statistics**, and **qualitative examples**.  
- Submit code and a short **report** with quantitative and qualitative analysis.
