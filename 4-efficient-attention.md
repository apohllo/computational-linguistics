# Course Task: Memory-Efficient Transformer Training Techniques

## Objective
The goal of this assignment is to **compare several modern memory-optimization techniques** used during Transformer training.  
Using the *same dataset*, *same model architecture*, and *same hyperparameters* (except for memory-related settings), your task is to measure and analyze how different optimization techniques influence:

- GPU memory usage  
- Maximum batch size that fits into memory  
- Training speed (time per step and total time for 1 epoch)  
- Final model performance (perplexity after 1 epoch)  

You will implement **four training regimes**:

1. **Baseline + BF16 mixed precision**
2. **FlashAttention**
3. **Windowed (local) attention**
4. **Gradient checkpointing**

Use the **same model checkpoint**, **same tokenizer**, **same dataset**, and **same training loop** structure for each experiment.

---

## Techniques to Compare

### 1. BF16 Mixed Precision (baseline)
Train the model using:
- `torch.cuda.amp.autocast(dtype=torch.bfloat16)`
- `GradScaler` if needed

This serves as the **baseline configuration**.  
It should be implemented **before** the other techniques.

### 2. FlashAttention
Replace the default attention mechanism with **FlashAttention** (v2 preferred, v1 acceptable).

You may use:
- `flash-attn` library  
- Hugging Face integration in `transformers` (if your model supports it)  

Aim to maximize throughput and batch size.

### 3. Windowed (Local) Attention
Replace full self-attention with sliding-window attention (e.g., `Longformer-style`, `Llama local attention`, custom block-local attention).

Requirements:
- Window size must be configurable
- Model architecture should stay otherwise identical

Measure the trade-off between memory reduction and degradation of perplexity.

### 4. Gradient Checkpointing
Enable:

```python
model.gradient_checkpointing_enable()


or use manual checkpointed layers if needed.

Measure memory savings and increased computation time.

## Dataset

Use the same dataset as in the 1st assignment.

* Use exactly the same number of tokens across all experiments
* Train for 1 epoch only
* Fix: sequence length, batch size (as high as fits), shuffle, tokenizer

## Model

Use the model from the 1st assignment.


## Measurements & Tools
### Memory Profiling

Use:
* PyTorch Memory Profiler

For each technique, record memory usage during:
* Forward pass of a single batch
* Backward pass
* Peak memory for one training step

Important:
For each experiment, increase batch size until OOM to find the maximum possible batch size that fits.

## Training Time

For each regime:
* measure seconds per step (mean over 20 steps)
* measure total training time for 1 epoch

## Final Perplexity

After one epoch:
* compute perplexity on a held-out validation set
* Use identical evaluation settings across experiments.

## Deliverables
1. Code

Submit scripts or notebooks containing:
* Model definition
* Training loop

Separate configuration toggles for:
* bf16 mixed precision
* flash attention
* windowed attention
* gradient checkpointing
* memory profiling code
* evaluation code (perplexity)

2. Report (4–6 pages)

Your report must include:
* Experimental Setup
* Dataset description
* Model architecture
* Hyperparameters
* Hardware used (GPU type, memory)
* Results Tables

For each technique include:

* Maximum batch size fitting into memory
* Peak memory usage
* Step time (forward + backward)
* Total epoch time
* Final perplexity
* Present results in clear comparison tables.

## Analysis & Discussion

Discuss:
* Why some techniques reduce memory more effectively
* Why some techniques slow down training
* The trade-off between memory, speed, and perplexity
* Whether combining techniques would help (optional)

Provide interpretations and not just raw numbers.

## Suggested Reading Materials
* *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness*, Tri Dao et al., 2022.
* *FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning*, Tri Dao et al., 2023.
* *Longformer: The Long-Document Transformer*, Iz Beltagy et al., 2020.
* *Efficient Transformers: A Survey*, Tay et al., 2020.
* NVIDIA AMP documentation
* PyTorch Automatic Mixed Precision Guide
* *Training Deep Nets with Sublinear Memory Cost*, Chen et al., 2016.

## Implementation Plan

1. Prepare dataset
   * Tokenize
   * Split into train/validation
   * Fix sequence length
2. Implement baseline model
   * Small GPT-like decoder-only Transformer
   * BF16 mixed precision
3. Implement the model using FlashAttention
   * Switch attention kernel
   * Benchmark memory and speed
   * Train for 1 epoch
4. Windowed Attention
   * Modify attention blocks (or call to the function) to use sliding window
   * Evaluate trade-offs
5. Use Gradient Checkpointing
   * Enable checkpointing
   * Measure memory savings and compute overhead
6. Profile memory
   * Run 20 warm-up steps
   * Confirm memory peak using PyTorch memory profiler
7. Run full 1 epoch training for each setting
   * Keep all variables identical
8. Evaluate perplexity
   * Use the same validation set
9. Prepare report


## Summary

1. You will implement, train, and analyze four memory-optimization techniques for Transformers:
* BF16 mixed precision
* FlashAttention
* Windowed attention
* Gradient checkpointing

For each method you must compare:
* Memory usage
* Maximum batch size
* Training speed
* Perplexity after 1 epoch

The final report should present clear comparisons and discuss the practical implications of each optimization technique.
