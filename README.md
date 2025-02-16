# Question to Questions 

`Question to Questions` is an system prompt designed to make an AI model to do step-by-step responses to user queries. 

---

## Table of Contents

1. [Overview](#overview)
2. [System Requirements](#system-requirements)
3. [Installation](#installation)
4. [Usage](#usage)
   - [With Ollama](#with-ollama)
   - [With vLLM](#with-vllm)
   - [With Transformers](#with-transformers)
   - [With LM Studio](#with-lm-studio)
   - [With Other AI Runners](#with-other-ai-runners)
5. [System Prompt Format](#system-prompt-format)
6. [Contributing](#contributing)

---

## Overview

`Question to Questions` is built to handle complex queries by breaking them into simpler sub-questions, answering each step-by-step, and combining the results into a coherent final response. It ensures ethical guidelines are followed and provides structured outputs using XML tags.

---

## System Requirements

- Python 3.8+ (for most AI runners)
- CUDA-enabled GPU (optional but recommended for faster inference)
- At least 8GB of RAM (16GB+ recommended for larger models)
- Internet connection (for downloading models or dependencies)

---

## Installation

### General Setup
1. Open any AI provider

2. Set the system prompt from the `system-prompt.txt`


### Model-Specific Setup
- For **Transformers**: Install Hugging Face Transformers.
  ```bash
  pip install transformers torch
  ```
- For **Ollama**: Download and install Ollama from [ollama.ai](https://ollama.ai).
- For **vLLM**: Follow the installation guide at [vLLM GitHub](https://github.com/vllm-project/vllm).
- For **LM Studio**: Download the desktop application from [lmstudio.ai](https://lmstudio.ai).

---

## Usage

### With Ollama

1. Install Ollama & start Ollama :
   ```bash
   ollama serve
   ```
2. Load the model:
   ```bash
   ollama run <model-name>
   ```
3. Provide the system prompt:
   `/set system ` system prompt from the `system-prompt.txt`

4. Interact/chat with the model.

---

### With vLLM

1. Install vLLM:
   ```bash
   pip install vllm
   ```
2. Run the model with the system prompt:
   ```python
   from vllm import LLM, SamplingParams

   llm = LLM(model="<model-name>")
   sampling_params = SamplingParams(temperature=0.7, max_tokens=512)

   system_prompt = """
   You are named ‘Question to Questions’.
   ```
   system prompt from the `system-prompt.txt
   ```
   """

   output = llm.generate([system_prompt + "Your question here"], sampling_params)
   print(output)
   ```

---

### With Transformers

1. Load the model and tokenizer:
   ```python
   from transformers import AutoModelForCausalLM, AutoTokenizer

   model_name = "<model-name>"
   tokenizer = AutoTokenizer.from_pretrained(model_name)
   model = AutoModelForCausalLM.from_pretrained(model_name)
   ```
2. Generate responses:
   ```python
   system_prompt = """
   You are named ‘Question to Questions’.
   ```
   system prompt from the `system-prompt.txt`
   ```
   """

   input_text = system_prompt + "Your question here"
   inputs = tokenizer(input_text, return_tensors="pt")
   outputs = model.generate(**inputs, max_length=512)
   print(tokenizer.decode(outputs[0], skip_special_tokens=True))
   ```

---

### With LM Studio

1. Open LM Studio and load the desired model.
2. Copy and paste the system prompt from the `system-prompt.txt` into the chat interface
3. Begin interacting with the model.

---

### With Other AI Runners

Most AI runners support custom system prompts. Simply copy the system prompt system prompt from the `system-prompt.txt` and configure it in your runner's settings:

---

## System Prompt Format

The AI generates responses in the following XML format:

```xml
<think>
Ok, let us break [Write the main question] into simpler questions that are easier to answer.
[Create broken-down questions]
Now that I have simpler questions, I will answer them one by one.
[Write broken-down question] Answer: [Answer step by step, regardless of length or complexity.]
Now let me continue to the next question. Repeat this process until all broken-down questions are answered.
Now, since I have answered all the broken-down questions, I need to present the final answer in a structured and neat form.
</think>
<answer>
[Final answer based on earlier responses.] 
Here is my detailed explanation and reasoning:
- [Explanation in points and yes/no questions.]
Now I am going to check for any mistakes or errors: [Review the entire response as if the user said the answer was incorrect or inaccurate.]
[If a mistake is found:]
Oh, I have made a mistake. [Explain the mistake and correct it.]
[If no mistakes are found:]
Let me provide you with my neat and concise conclusion.
**Conclusion**: [Provide a concise conclusion and review the answer.]
</answer>
```

---

## Contributing 
Just modify the `system-prompt.txt` and save it as a branch 
