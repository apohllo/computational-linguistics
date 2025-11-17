## **Course Task: Fine-Tuning vs. From-Scratch Training for Text Classification**

### **Objective**
The goal of this assignment is to compare two approaches to training decoder-only language models for **full-text classification**:

1. **From-scratch training**  
   - A small decoder-only model created, initialized, and trained entirely by the student.  
   - Designed to avoid overfitting by limiting parameter count.  

2. **Fine-tuning a pre-trained model**  
   - Select a suitable pre-trained decoder-only model from Hugging Face.  
   - Fine-tune it on the same downstream dataset.  

Students should analyze the differences in training behavior, generalization, stability, and performance.

---

### **Dataset Selection**
Choose **one text classification dataset** from Hugging Face with at least a few thousand examples.  
Examples include (but are not limited to):

- AG News  
- Yelp Review Polarity  
- Amazon Reviews  
- Tweet Eval (sentiment or hate speech)  
- Toxic Comment Classification  

Important requirements:
- The dataset must involve **full text classification** (not token classification).  
- The texts should be long enough to demonstrate the difference between the two approaches.  
- The dataset must be split into **train/validation/test** (use provided splits or create them).

---

### **Models to Train**

#### **1. From-Scratch Model**
- A **decoder-only architecture** (e.g., small GPT-like model).  
- The model must be **significantly smaller** than the fine-tuned model to avoid overfitting.  
  - Typical size: 0.5M–10M parameters.  
  - Shallow depth, small embedding dimension.  
- Add a **classification head**:
  - Either take the final hidden state of the last token and feed it into an MLP classifier, **or**
  - Use mean/max pooling over hidden states followed by an MLP classifier.
- Training considerations:
  - Use a tokenizer of your choice (trained or pre-trained).  
  - Limit sequence length based on your GPU memory.  
  - Expect the from-scratch model to require longer training and achieve weaker results.

#### **2. Fine-Tuned Pre-trained Model**
- Select a **decoder-only model available on Hugging Face** (e.g., GPT-2, GPT-Neo, GPT-J, Pythia, Phi-1.5 or any smaller model depending on your GPU).  
- Attach a **classification head**:
  - Add an MLP on top of the final transformer block.  
  - Or use an existing Hugging Face classification architecture if compatible.
- Fine-tune the entire model or choose a technique such as:
  - LoRA  
  - Freeze-then-unfreeze  
  - Adapter modules  
- Expect this model to train **much faster**, converge better, and achieve higher performance.

---

### **Evaluation Metrics**

Each model must be evaluated using:
- **Classification accuracy**  
- **F1 score** (macro or weighted)  
- **Training time** (total and per epoch)  
- **Inference time** on the test set  
- **Model size** (parameter count)

You should also:
- Inspect whether the from-scratch model overfits (validation curves).  
- Discuss stability and convergence speed.

---

### **General Plan for the Experiments**

#### **1. Dataset Preparation & Verification**
- Load dataset splits and inspect several examples.  
- Verify class distribution; apply stratification if generating custom splits.  
- Clean data if needed (remove empty texts, fix encoding issues).  
- Tokenize the corpus:
  - For the fine-tuned model: use its pre-trained tokenizer.  
  - For the from-scratch model: you may reuse a previous tokenizer or train a small one.  
- Set up a reasonable maximum sequence length (e.g., 128–512 tokens).

#### **2. Running the From-Scratch Experiment**
- Initialize the small decoder-only model.  
- Add a classification head.  
- Train with:
  - AdamW optimizer  
  - Learning rate warmup  
  - Gradient clipping  
  - Regularization (dropout, smaller model size) to reduce overfitting  
- Save model checkpoints and record training curves.  

#### **3. Running the Fine-Tuning Experiment**
- Load the selected pre-trained model and corresponding tokenizer.  
- Attach a classification head or use an existing one (e.g., GPT2ForSequenceClassification).  
- Fine-tune:
  - Much lower learning rate than from-scratch  
  - Fewer epochs  
  - Monitor validation accuracy/f1 regularly  
- Compare training curves with the from-scratch run.

#### **4. Running Evaluation**
- Compute performance metrics on the held-out test set.  
- Measure inference time (e.g., classify 1,000 examples).  
- Analyze training logs and learning curves.

---

### **Deliverables**

#### **1. Code**
Provide:
- Model definitions for both approaches  
- Training scripts  
- Evaluation scripts  
- Tokenization/configuration code  

#### **2. Report (4–6 pages)**
Your report must include:

##### **Model Descriptions**
- Size, parameter count, architecture outline  
- Description of the classification head  
- Tokenizer type and vocabulary size  

##### **Dataset Description**
- Basic statistics  
- Examples  
- Preprocessing steps  

##### **Training & Evaluation**
- Training curves for both models  
- Classification metrics (accuracy, f1)  
- Training and inference time  
- Observations of convergence, stability, overfitting  

##### **Comparative Analysis**
Discuss:
- Why fine-tuning outperforms from-scratch  
- When training from scratch might be preferred  
- Sensitivity to model size and dataset size  
- Practical insights from implementation

--- 

### Literature

* [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/pdf/1810.04805), Jacob Devlin Ming-Wei Chang Kenton Lee Kristina Toutanova
* [Language Models are Unsupervised Multitask Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf), Alec Radford, Jeffrey Wu, Rewon Child, David Luan, Dario Amodei, Ilya Sutskever

---

### **Tips & Hints**

- **Expect the from-scratch model to struggle**, unless it is very small and the task is easy.  
- **Do not oversize the from-scratch model** — it will overfit immediately.  
- **The fine-tuned model may require careful LR scheduling** to avoid catastrophic forgetting.  
- Try using **gradient accumulation** if GPU memory is limited.  
- Use **early stopping** for both models to get clean comparisons.  
- If models diverge:
  - Lower the learning rate  
  - Reduce sequence length  
  - Increase batch size (or accumulation)  

---

### **Summary**
- Select a moderate-sized classification dataset.  
- Train **two decoder-only models**:
  1. A **small from-scratch model**  
  2. A **fine-tuned pre-trained model**  
- Compare performance, training behavior, parameter count, and computational efficiency.  
- Submit code and a detailed comparative report.
