## **Course Task: Implementation of a Language Model (RNN and Transformer-based)**

### **Objective**
The goal of this assignment is to design, implement, and evaluate a small-scale **language model** using **two architectures**:
1. A **Recurrent Neural Network (RNN)** variant (e.g., LSTM, GRU)  
2. A **Transformer-based model**

Both models have to be trained for the causal language modeling task (prediction of the next token).

Optionally, students may explore or experiment with a **“Baby Dragon Hatchling GPU model”** ([BDH-GPU](https://github.com/pathwaycom/bdh)) - a very recent model designed to resamble the organization of human brain.

---

### **Model Size and Resources**
You are encouraged to adapt the **size and complexity** of your models to fit the **GPU resources** available to you.  
- A small model is **perfectly acceptable** if that’s what your hardware can handle.  
- The main goal is to **demonstrate understanding and correct implementation**, not to train a massive model.

---

### **Data**
You are free to choose any text corpus as your **training data**.  
If you do not have a preferred dataset, you can use **[Speakleash Polish dataset](https://github.com/speakleash/speakleash)** or the **[Kobza Ukrainian dataset](https://huggingface.co/datasets/Goader/kobza)**. The second dataset make sense only if you know Ukrainian.

For evaluation, you should use a **held-out Wikipedia corpus** in a language of your choice.

### **Inference**
Once the model is trained, use inference to obtain completion of at least 10 prompts of your choice. 
Present the prompts and the outputs in the report.

---

### **Evaluation Metrics**
Your models should be evaluated using **two key metrics**:

1. **Perplexity** on the held-out corpus  
   - Report and analyze your model’s perplexity score.  
   - Compare your results with values reported in the literature for similar-sized models.

2. **Time Efficiency**  
   - Measure and compare **training time** and **inference time** for both RNN and Transformer architectures.  
   - Reflect on how model size and architecture affect computational performance.

---

### **Deliverables**
Your submission should include:
- The **source code** for both models.  
- A **short report** (2–4 pages) including:
  - Model architecture details (layers, parameters, optimizer, etc.)  
  - Description of the dataset(s) used  
  - Evaluation results (perplexity and time metrics)  
  - Comparison of prompt completions and interpretation of findings  
  - Discussion of implementation challenges and insights  

---

### **Study Material**
Before starting, carefully read the paper:  
* **[Attention Is All You Need](https://arxiv.org/abs/1706.03762)** (Vaswani et al., 2017) 
* **[The Dragon Hatchling: The Missing Link between the Transformer and Models of the Brain](https://arxiv.org/abs/2509.26507)** (Kosowski at al., 2025)

This paper provides the theoretical foundation for the Transformer architecture and its advantages over RNN-based models.

---

### **Next Meeting**
There will be a **short quiz** on the *“Attention Is All You Need”* paper at the start of the next class session.  
Make sure you understand:
- The **core ideas** of self-attention and multi-head attention  
- The **overall architecture** of the Transformer  
- The **motivation and limitations** of RNNs that the Transformer addresses

---

### **Summary**
- Implement **two models**: RNN and Transformer  
- Train on a text corpus of your choice (e.g., Kobza dataset)  
- Evaluate using **perplexity** and **training/inference time**  
- Adapt model size to your **GPU capabilities**  
- Study **“Attention Is All You Need”** for the upcoming test  
