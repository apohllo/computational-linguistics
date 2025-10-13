## **Course Task: Implementation of a Language Model (RNN and Transformer-based)**

### **Objective**
The goal of this assignment is to design, implement, and evaluate a small-scale **language model** using **two architectures**:
1. A **Recurrent Neural Network (RNN)** variant (e.g., LSTM, GRU)  
2. A **Transformer-based model**

Optionally, students may explore or experiment with a **“dragon hatchling baby GPU model”** — a highly compact or custom hybrid model tailored for very limited GPU resources.

---

### **Model Size and Resources**
You are encouraged to adapt the **size and complexity** of your models to fit the **GPU resources** available to you.  
- A small model is **perfectly acceptable** if that’s what your hardware can handle.  
- The main goal is to **demonstrate understanding and correct implementation**, not to train a massive model.

---

### **Data**
You are free to choose any text corpus as your **training data**.  
If you do not have a preferred dataset, you can use the **Kobza Ukrainian dataset**, which contains a rich collection of Ukrainian texts suitable for language modeling.

For evaluation, you should use a **held-out Wikipedia corpus** in a language of your choice.

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
  - Comparison and interpretation of findings  
  - Discussion of implementation challenges and insights  

---

### **Study Material**
Before starting, carefully read the paper:  
📘 **“Attention Is All You Need”** (Vaswani et al., 2017)  

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
