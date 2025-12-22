# Course Task: Evaluating Large Language Models with Diverse Prompting Strategies

## Objective
The goal of this assignment is to **systematically evaluate and compare open-source Large Language Models** across diverse task types using different prompting techniques.

You will work with models of **two different sizes** (small: 1-2B parameters, large: 10-14B parameters) of which at least **one is a reasoning-focused model**. 
Through careful prompt engineering and manual evaluation, you will analyze how model size, architecture type, and prompting strategy affect performance across 10 diverse tasks.

Your analysis should demonstrate:
- The impact of model size on task performance
- Effectiveness of zero-shot vs. few-shot prompting
- Benefits of chain-of-thought reasoning for standard models
- Performance differences between standard and reasoning-specialized models
- The importance of prompt engineering

---

## Models to Evaluate

You will use **Ollama** to run the models locally. Install Ollama from [ollama.com](https://ollama.com) and pull the required models.

### Required Models (3 minimum)

1. **Small Model (1-2B parameters)**
   - Examples: `qwen2.5:1.5b`, `phi3:mini`, `gemma2:2b`, `bielik-1.5b-v3.0-instruct`, `ministral-3:3b`
   - Pull with: `ollama pull qwen2.5:1.5b`

2. **Large reasoning Model (10-14B parameters)**
   - Examples: `deepseek-r1:7b`, `qwen3:30b`, `qwen3:14b`, `magistral:24b` (if you have enough VRAM)
   - Pull with: `ollama pull qwen2.5:14b`
   - Note: Ensure you have sufficient RAM/VRAM (at least 16GB recommended)
   - For qwen 3 you can disable reasoning with `/no_think` added at the beggining of the prompt


### Ollama Setup

```bash
# Install Ollama (follow instructions at ollama.com)

# Pull your selected models
ollama pull SpeakLeash/bielik-1.5b-v3.0-instruct:Q8_0
ollama pull ministral-3:3b
ollama pull qwen2.5:14b
ollama pull deepseek-r1:7b

# Test a model
ollama run qwen2.5:1.5b "Hello, how are you?"
```

You can interact with Ollama via:
- Command line: `ollama run <model>`
- Python API: `pip install ollama` and use the `ollama` package
- REST API: `curl http://localhost:11434/api/generate`

**Recommended**: Use the Python API for systematic evaluation.

---

## Ten Evaluation Tasks

Select **one representative example** for 10 different categories of model generation tasks,
You can use the following list as an inspiration, but you are not limited to those tasks:


1. Instruction Following (IFEval-style)
2. Logical Reasoning
3. Creative Writing
4. Code Generation
5. Reading Comprehension
6. Common Sense Reasoning
7. Language Understanding & Ambiguity
8. Factual Knowledge & Retrieval
9. Mathematical Problem Solving
10. Ethical Reasoning & Nuance


For each task define the following elements before you start writing the prompts:
* task description
* evaluation criteria

For instance for ethical reasoning:

**Task**: Analyze a situation with ethical considerations
**Evaluation Criteria**: Understanding of ethical frameworks, balanced analysis, nuanced reasoning

## Prompting Strategies

For each task and model combination, test the following approaches:

### 1. Zero-Shot Prompting
Provide only the task description without examples.

**Example**:
```
[Task description directly]
```

### 2. Few-Shot Prompting
Include 2-3 examples before the actual task.

**Example**:
```
Here are some examples:

Example 1:
[Input 1]
[Output 1]

Example 2:
[Input 2]
[Output 2]

Now solve this:
[Actual task]
```

**Important**: Examples should be from similar tasks but NOT the exact tasks you're evaluating. 
Create a separate development set for crafting few-shot examples.

### 3. Chain-of-Thought (CoT) Prompting

For **standard models** (not reasoning models), explicitly request step-by-step thinking.

**Example**:
```
[Task description]

Let's solve this step by step:
1. First,
2. Then,
3. Finally,
```

Or simply add: "Let's think step by step before answering."

**Note**: Do NOT use CoT prompting with reasoning models (like DeepSeek-R1), as they already 
perform internal reasoning. For reasoning models, use standard zero-shot or few-shot prompts.

---

## Experimental Design

### Prompt Engineering Phase

**Before evaluation**:
1. Create a **development set** of 2-3 alternative examples for each task type
2. Test your prompts on these development examples
3. Iterate on prompt formulations to find effective strategies
4. Document your prompt engineering process

**Important**: The development set must be separate from your evaluation examples. 
This ensures you're testing generalization, not memorization.

### Evaluation Phase

For each of the 10 tasks:
1. Test with **small model** (1-2B):
   - Zero-shot
   - Few-shot (2-3 examples)
   - Chain-of-thought

2. Test with **large reasoning model**:
   - Zero-shot
   - Few-shot (2-3 examples)
   - (No CoT - these models reason internally)

**Total experiments**: 10 tasks × 2 models × 2.5 prompting strategies = 50 evaluations

---

## Deliverables

### 1. Code

Submit well-documented code including:
- Task definitions and prompts
- Functions for each prompting strategy
- Model querying logic using Ollama
- Result storage and organization
- Clear comments explaining your approach

### 2. Final Report (6-8 pages)

Your report must include:

1. Introduction
2. Methodology
3. Results
4. Discussion

## Suggested Reading

- *Language Models are Few-Shot Learners* (GPT-3 paper), Brown et al., 2020
- *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*, Wei et al., 2022
- *The False Promise of Imitating Proprietary LLMs*, Gudibande et al., 2023
- *Instruction Tuning for Large Language Models: A Survey*, Zhang et al., 2023
- Ollama Documentation: https://ollama.com/docs

---

## Summary

This assignment requires you to:
1. Set up and run 2 open-source LLMs locally using Ollama
2. Evaluate performance on 10 diverse tasks
3. Compare zero-shot, few-shot, and chain-of-thought prompting
4. Manually score outputs using consistent rubrics
5. Analyze how model size, architecture, and prompting affect performance
6. Document your prompt engineering process
7. Write a comprehensive report with insights

The goal is to develop practical skills in LLM evaluation, prompt engineering, and critical analysis of model capabilities and limitations.
