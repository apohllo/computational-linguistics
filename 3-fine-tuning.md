## **Course Task: Text Classification with decoder only models**

### **Objective**
The goal of this assignment is to compare approaches of classification with decoder-only language models:

1. **From-scratch training**  
   - A small decoder-only model created, initialized, and trained entirely by the student.  
   - Designed to avoid overfitting by limiting parameter count.  

2. **Fine-tuning a pre-trained model**  
   - Select a suitable pre-trained decoder-only model from Hugging Face.  
   - Fine-tune it on the same downstream dataset.
  
3. **Zero-shot Classification with Token-Class Prediction**
   - Use the same model as in 2.
   - Use constraint decoding and select appropriate classes as classification indicators.

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
- [Polish youth slang](https://huggingface.co/datasets/jziebura/polish_youth_slang_classification)

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
  - Alternatively you can add an instruction at the end and compare probability of two (or more) output tokens representing the competing classes.
- Training considerations:
  - Use a tokenizer of your choice (trained or pre-trained).  
  - Limit sequence length based on your GPU memory.  
 
You can also train a encoder-only model from scratch, but this will require additional works, since the previous labs were concentrated on decoder-only models.

#### **2. Fine-Tuned Pre-trained Model**
- Select a **decoder-only model available on Hugging Face** (e.g., GPT-2, GPT-Neo, GPT-J, Pythia, Phi-1.5, Bielik, for decoder-only models).  
- Attach a **classification head**:
  - Add an MLP on top of the final transformer block.  
  - Or use an existing Hugging Face classification architecture if compatible.
- Fine-tune the entire model or choose a technique such as:
  - Unfreez speficic layers
  - [LoRA](https://huggingface.co/docs/peft/main/conceptual_guides/lora) 
to speed-up the training process compared to full-model training.


#### **3. Constrained prediction**
- This model is not trained for the task, so there aren't any training instructions. The scheme is called "zero-shot classification", due to the fact that we don't apply any training and we don't provide any examples for the model.

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
- Initialize the small decoder/encoder-only model.  
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

#### **4. Running Prediction using constrained decoding**
- Load model + tokenizer
- Define tokens corresponding to classes in the category, such as *positive*/*negative* for the **sentiment** category.
- Define simple prompt such as "<text> Based on the provided context, the sentiment of the text is:"
- Compute the probability distribution of the classes using constrained decoding.
- The idea of the constrained decoding is that we allow the model only to predict the texts that correspond to the allowed classes, such as *positive*/*negative*. If the classes do not share any prefix, this is the same as predicting the probability of the first token for each class.
- The probability distribution is computed according to the softmax over the predicted values. You need to ask the model to return logits, rather than probability distribution.
- The default approach is that for a binary problem you return the class with the highest probability (so prob distribution does not have to be computed). But you can also apply a different threshold, if the dataset in question is highly unbalanced.

#### **5. Running Evaluation**
- Compute performance metrics on the held-out test set.  
- Measure inference time (e.g., classify 1,000 examples).  
- Analyze training logs and learning curves.

---

### **Deliverables**

#### **1. Code**
Provide:
- Model definitions for all approaches  
- Training scripts
- Evaluation scripts
- Tokenization/configuration code  

#### **2. Report (4–6 pages)**
Your report must include:

##### **Model Descriptions**
- Size, parameter count, architecture outline  
- Description of the classification head  
- Tokenizer type and vocabulary size
- Class names for the constrained classification and threshold, if different from 0.5

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
- Strengths and weaknesses of each approach
- When each of the method might be preferred  
- Sensitivity to model size and dataset size  
- Practical insights from implementation

--- 

### Literature

* [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/pdf/1810.04805), Jacob Devlin Ming-Wei Chang Kenton Lee Kristina Toutanova
* [Language Models are Unsupervised Multitask Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf), Alec Radford, Jeffrey Wu, Rewon Child, David Luan, Dario Amodei, Ilya Sutskever

---

### **Tips & Hints**

- **Do not oversize the from-scratch model** — it will overfit quickly.  
- **The fine-tuned model may require careful LR scheduling** to avoid catastrophic forgetting. Usually 10^-4 - 10^-5 is the max LR value yielding reasonable training.
- Try using **gradient accumulation** if GPU memory is limited. Still a batch size of 1 is pretty bad. It's better to constraint the length of the input and use larger batch size
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
- Use an existing model with constrained decoding.
- Compare performance, training behavior, parameter count, and computational efficiency.  
- Submit code and a detailed comparative report.
