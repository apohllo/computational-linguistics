## **Course Task: Training a Tiny Reasoning Model for Structured Reasoning Tasks**

### **Objective**
The goal of this assignment is to design, train, and evaluate a **tiny reasoning model (TRM)** on 
challenging **structured reasoning benchmarks**, and to compare its behavior with a large pre-trained language model.

The focus is on **reasoning quality**, **generalization**, and **data efficiency**, rather than scale.

---

### **Datasets**
Use the following reasoning datasets:
- **Sudoku Extreme**
- **ARC-AGI-1**
- **ARC-AGI-2**

Apply **data augmentation techniques** described by the dataset or benchmark authors (e.g., rule-preserving transformations, 
symmetry operations, grid permutations) to increase the effective dataset size.

You should **adapt the dataset sizes** so that:
- Training time is feasible on your machine  
- GPU/CPU memory limits are respected  

---

### **Model**
Train a **Tiny Reasoning Model (TRM)** with one of the following architectures:
- Feed-Forward Network (FFN)  
- Self-attention–based model

---

### **Training & Evaluation**
- Train a **separate model or configuration** per dataset if needed.  
- Inspect model outputs manually to assess:
  - Correctness  
  - Failure modes  
  - Partial reasoning success  

---

### **Comparison with Large Models**
Select **one commercial or open-weight LLM** (e.g., GPT-style, LLaMA-style) and:
- Evaluate it on **5 examples from each dataset**.
- Try different prompting techniques and different input representation to better align the data with the model (e.g. you can try to make sure that the input is always represented with the same number of tokens, corresponding to the problem size).

- Compare:
  - Success rate  
  - Error patterns  
  - Qualitative reasoning behavior  

---

### **Report Requirements**
Your report should include:
- Description of the model architecture and dataset representations  
- Augmentation techniques used and their impact  
- Quantitative results (scores per dataset/per model)  
- Qualitative comparison with the selected LLM  
- Discussion of strengths and limitations of small reasoning models  

---

### **Notes**
- Emphasis is on **reasoning structure**, not language fluency.  
- Clear visualizations or tables for ARC and Sudoku results are encouraged.  
- Manual analysis is a required component of the evaluation.

---

### Resources

["Less is More: Recursive Reasoning with Tiny Networks", Alexia Jolicoeur-Martineau](https://arxiv.org/pdf/2510.04871)
