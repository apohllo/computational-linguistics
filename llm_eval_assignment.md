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
   - Examples: `qwen2.5:1.5b`, `phi3:mini`, `gemma2:2b`, `bielik-1.5b-v3.0-instruct`
   - Pull with: `ollama pull qwen2.5:1.5b`

2. **Large reasoning Model (10-14B parameters)**
   - Examples: `deepseek-r1:7b`, `qwen3:30b`, `qwen3:14b` (if you have enough VRAM)
   - Pull with: `ollama pull qwen2.5:14b`
   - Note: Ensure you have sufficient RAM/VRAM (at least 16GB recommended)
   - For qwen 3 you can disable reasoning with `/no_think` added at the beggining of the prompt


### Ollama Setup

```bash
# Install Ollama (follow instructions at ollama.com)

# Pull your selected models
ollama pull qwen2.5:1.5b
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

Select **one representative example** from each of the following task categories. The tasks should be diverse and test different capabilities:

### 1. Instruction Following (IFEval-style)
**Task**: Follow precise formatting instructions

**Example**:
```
Write a short poem about artificial intelligence. 
Your response must satisfy ALL of the following constraints:
- The poem must be exactly 4 lines long
- Each line must start with a capital letter
- The word "learning" must appear exactly once
- End your response with "--- END ---"
```

**Evaluation Criteria**: Exact compliance with all formatting constraints

---

### 2. Logical Reasoning
**Task**: Solve a logic puzzle

**Example**:
```
Three friends - Alice, Bob, and Carol - each have a different pet: 
a cat, a dog, and a bird.
- Alice is allergic to fur
- Bob's pet can fly
- Carol doesn't have the dog

Who has which pet?
```

**Evaluation Criteria**: Correct deduction, clear reasoning steps

---

### 3. Creative Writing
**Task**: Generate creative content with specific requirements

**Example**:
```
Write a very short story (4-5 sentences) about a time traveler 
who accidentally changes history. The story must:
- Include a plot twist
- Be written in past tense
- Feature an unexpected consequence
```

**Evaluation Criteria**: Creativity, coherence, adherence to requirements, quality of twist

---

### 4. Code Generation
**Task**: Write functional code with explanation

**Example**:
```
Write a Python function that takes a list of integers and returns 
the second largest number. Handle edge cases (empty list, single element, 
all elements the same). Include docstring and example usage.
```

**Evaluation Criteria**: Correctness, edge case handling, code quality, documentation

---

### 5. Reading Comprehension
**Task**: Answer questions based on a given text

**Example**:
```
Text: "The platypus is one of the few venomous mammals. Male platypuses 
have a spur on their hind legs that can deliver venom capable of causing 
severe pain in humans. Despite this defense mechanism, platypuses are 
generally shy and avoid human contact. They are native to eastern Australia 
and Tasmania, where they inhabit freshwater rivers and streams."

Questions:
1. Which gender of platypus has venomous spurs?
2. Where do platypuses naturally live?
3. What is the platypus's typical behavior around humans?
```

**Evaluation Criteria**: Accuracy of answers, appropriate use of text information

---

### 6. Common Sense Reasoning
**Task**: Apply real-world knowledge to solve problems

**Example**:
```
You have a 3-gallon jug and a 5-gallon jug. You need to measure 
exactly 4 gallons of water. How can you do this?
Explain your solution step by step.
```

**Evaluation Criteria**: Correct solution, logical steps, practical applicability

---

### 7. Language Understanding & Ambiguity
**Task**: Resolve ambiguous language

**Example**:
```
Explain the two different meanings of this sentence:
"The chicken is ready to eat."

Then write two sentences that disambiguate each meaning.
```

**Evaluation Criteria**: Correct identification of ambiguity, clear explanations, good examples

---

### 8. Factual Knowledge & Retrieval
**Task**: Answer factual questions accurately

**Example**:
```
Answer the following:
1. In which year did the fall of the Berlin Wall occur?
2. What is the chemical symbol for gold?
3. Who wrote "One Hundred Years of Solitude"?

Provide brief explanations for each answer.
```

**Evaluation Criteria**: Factual accuracy, appropriate level of detail

---

### 9. Mathematical Problem Solving
**Task**: Solve a multi-step math problem

**Example**:
```
A store is having a sale. A jacket originally costs $120. 
First, it's discounted by 25%. Then, an additional 10% is taken 
off the already discounted price. What is the final price?
Show your calculations step by step.
```

**Evaluation Criteria**: Correct calculation, clear methodology, proper step-by-step explanation

---

### 10. Ethical Reasoning & Nuance
**Task**: Analyze a situation with ethical considerations

**Example**:
```
A self-driving car's brakes fail. It must choose between:
- Swerving left: hitting a barrier (certain injury to 1 passenger)
- Continuing straight: potentially hitting 2 pedestrians

Analyze this dilemma from two different ethical frameworks 
(e.g., utilitarian and deontological). Which choice would each 
framework support and why? Keep your response under 150 words.
```

**Evaluation Criteria**: Understanding of ethical frameworks, balanced analysis, nuanced reasoning

---

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

2. Test with **large model** (10-14B):
   - Zero-shot
   - Few-shot (2-3 examples)
   - Chain-of-thought

3. Test with **reasoning model**:
   - Zero-shot
   - Few-shot (2-3 examples)
   - (No CoT - these models reason internally)

**Total experiments**: 10 tasks × 3 models × ~3 prompting strategies = ~90 evaluations

---

## Implementation Guide

### Using Ollama with Python

```python
import ollama

def query_model(model_name, prompt, temperature=0.7):
    """
    Query an Ollama model with a prompt
    """
    response = ollama.generate(
        model=model_name,
        prompt=prompt,
        options={
            'temperature': temperature,
            'num_predict': 512,  # max tokens
        }
    )
    return response['response']

# Example usage
model = "qwen2.5:1.5b"
prompt = "Write a haiku about machine learning."
response = query_model(model, prompt)
print(response)
```

### Recommended Code Structure

```python
# Define your tasks
tasks = {
    "instruction_following": {
        "prompt": "...",
        "expected_criteria": ["4 lines", "starts with capital", ...]
    },
    "logical_reasoning": {
        "prompt": "...",
        "expected_criteria": ["correct answer", "clear reasoning"]
    },
    # ... more tasks
}

# Define prompting strategies
def zero_shot(task_prompt):
    return task_prompt

def few_shot(task_prompt, examples):
    return f"{examples}\n\nNow solve:\n{task_prompt}"

def chain_of_thought(task_prompt):
    return f"{task_prompt}\n\nLet's think step by step:"

# Run experiments
results = []
for task_name, task_data in tasks.items():
    for model in ["qwen2.5:1.5b", "qwen2.5:14b", "deepseek-r1:7b"]:
        for strategy in ["zero_shot", "few_shot", "cot"]:
            # Generate prompt based on strategy
            # Query model
            # Store result
            pass
```

---

## Evaluation & Scoring

### Manual Evaluation

For each model output, assess:

1. **Correctness** (0-5 scale)
   - 0: Completely wrong or nonsensical
   - 1: Mostly wrong with minor correct elements
   - 2: Partially correct but significant errors
   - 3: Mostly correct with minor errors
   - 4: Correct with minor imperfections
   - 5: Fully correct and complete

2. **Instruction Following** (0-5 scale)
   - Did the model follow all specified constraints?
   - Rate based on percentage of requirements met

3. **Reasoning Quality** (0-5 scale, where applicable)
   - Clarity of explanation
   - Logical coherence
   - Step-by-step progression

4. **Overall Quality** (0-5 scale)
   - Consider fluency, coherence, usefulness
   - Holistic assessment

### Scoring Guidelines

Create a **detailed rubric** for each task explaining:
- What constitutes each score level
- Specific criteria to check
- Example responses for different score levels

**Document your scoring rationale** for each evaluation.

---

## Deliverables

### 1. Code

Submit well-documented code including:
- Task definitions and prompts
- Functions for each prompting strategy
- Model querying logic using Ollama
- Result storage and organization
- Clear comments explaining your approach

### 2. Prompt Engineering Documentation

A separate document describing:
- Your prompt development process
- Different prompt versions tested
- Why you chose your final prompts
- Examples of prompts that didn't work well
- Lessons learned about prompt engineering

### 3. Results Spreadsheet

A structured table with columns:
- Task name
- Model name
- Prompting strategy
- Raw model output
- Correctness score
- Instruction following score
- Reasoning quality score
- Overall quality score
- Notes/observations

### 4. Final Report (6-8 pages)

Your report must include:

#### Introduction
- Assignment objectives
- Selected models and rationale
- Hardware specifications (RAM, VRAM, CPU/GPU)

#### Methodology
- Task descriptions and why they were chosen
- Prompting strategies implemented
- Evaluation criteria and scoring rubrics
- Ollama setup and configuration

#### Results

**Quantitative Analysis**:
- Summary statistics for each model across all tasks
- Performance comparison tables by:
  - Model size (small vs. large)
  - Model type (standard vs. reasoning)
  - Prompting strategy (zero-shot vs. few-shot vs. CoT)
- Visualizations (bar charts, heatmaps) showing performance patterns

**Qualitative Analysis**:
- Which tasks were hardest/easiest for each model?
- Where did few-shot prompting help most?
- When did chain-of-thought improve performance?
- How did reasoning models differ from standard models?
- Notable failure cases and interesting outputs

#### Discussion

Analyze and interpret:
- **Model Size Impact**: How much does increasing parameters help? Is it consistent across tasks?
- **Reasoning Models**: Do specialized reasoning models justify their cost? On which tasks?
- **Prompting Strategy Effectiveness**: When is few-shot worth the extra tokens? When does CoT help?
- **Task-Specific Observations**: Which capabilities seem to scale with size vs. requiring specialized training?
- **Practical Implications**: For resource-constrained scenarios, which model/strategy combinations offer best value?

#### Prompt Engineering Insights
- Most effective prompt patterns discovered
- Common pitfalls in prompt design
- How prompt quality affected results
- Recommendations for future prompt engineering

#### Conclusion
- Key findings summary
- Recommendations for model selection
- Limitations of your evaluation
- Future work suggestions

---

## Grading Rubric (100 points)

- **Task Design & Diversity** (15 points)
  - Selection of 10 diverse, well-designed tasks
  - Clear evaluation criteria for each task
  
- **Prompt Engineering** (20 points)
  - Evidence of iterative prompt development
  - Quality of final prompts
  - Appropriate use of different prompting strategies
  - Documentation of process

- **Implementation** (15 points)
  - Correct use of Ollama
  - Clean, documented code
  - Proper handling of different models and strategies

- **Evaluation Quality** (20 points)
  - Thorough manual evaluation with clear rubrics
  - Consistent scoring methodology
  - Detailed documentation of scores and rationale

- **Analysis & Insights** (20 points)
  - Depth of quantitative analysis
  - Quality of qualitative observations
  - Thoughtful interpretation of results
  - Connections between findings and model capabilities

- **Report Quality** (10 points)
  - Clear writing and organization
  - Effective visualizations
  - Professional presentation

---

## Tips for Success

1. **Start Early**: Setting up Ollama and downloading models takes time
2. **Test Your Setup**: Run a few simple queries before starting full evaluation
3. **Document Everything**: Keep notes during evaluation about interesting observations
4. **Be Consistent**: Use the same evaluation criteria throughout
5. **Iterate on Prompts**: Don't settle for your first prompt attempt
6. **Consider Context Length**: Some models have token limits; keep prompts reasonable
7. **Monitor Resources**: Watch RAM/VRAM usage, especially with larger models
8. **Save Outputs**: Store all raw model responses for later re-evaluation if needed

---

## Suggested Reading

- *Language Models are Few-Shot Learners* (GPT-3 paper), Brown et al., 2020
- *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*, Wei et al., 2022
- *The False Promise of Imitating Proprietary LLMs*, Gudibande et al., 2023
- *Instruction Tuning for Large Language Models: A Survey*, Zhang et al., 2023
- Ollama Documentation: https://ollama.com/docs

---

## Timeline Suggestion

- **Week 1**: Setup Ollama, select models, design tasks, create development set
- **Week 2**: Prompt engineering phase, iterate on prompts with dev set
- **Week 3**: Run all evaluations, collect and organize results
- **Week 4**: Analysis, create visualizations, write report

---

## Summary

This assignment requires you to:
1. Set up and run 3+ open-source LLMs locally using Ollama
2. Evaluate performance on 10 diverse tasks
3. Compare zero-shot, few-shot, and chain-of-thought prompting
4. Manually score outputs using consistent rubrics
5. Analyze how model size, architecture, and prompting affect performance
6. Document your prompt engineering process
7. Write a comprehensive report with actionable insights

The goal is to develop practical skills in LLM evaluation, prompt engineering, and critical analysis of model capabilities and limitations.
