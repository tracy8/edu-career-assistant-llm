# Rwanda A-Level Combination & Career Guidance Assistant

## Project Overview

This project implements a domain-specific Large Language Model (LLM) assistant designed to guide Rwandan O-Level students in selecting appropriate A-Level subject combinations aligned with their desired career paths.

The assistant answers structured questions such as:

- "Which combination is best for Medicine?"
- "Is HEG suitable for Law?"
- "What careers can I pursue with MPC?"

The system was built by fine-tuning a pre-trained LLM using parameter-efficient fine-tuning (LoRA) on a custom dataset of instruction-response pairs.

---

## Domain Alignment

**Domain:** Education / Career Guidance

The assistant focuses specifically on Rwanda’s A-Level subject combinations:

- MPC (Mathematics, Physics, Chemistry)
- PCB (Physics, Chemistry, Biology)
- MCE (Mathematics, Computer Science, Economics)
- MEG (Mathematics, Economics, Geography)
- HEG (History, Economics, Geography)
- EGM (Economics, Geography, Mathematics)

The objective is to reduce confusion among O-Level students when choosing subject combinations.

---

## Dataset Creation

A structured dataset of instruction-response pairs was manually created.

Each example follows this format:

Instruction:
Question

Response:
Answer


The dataset includes:

- Combination → Career mapping questions  
- Career → Recommended combination questions  
- Suitability verification (career + combination)

The dataset was converted into a Hugging Face Dataset object and split into:

- 80% Training  
- 20% Evaluation  

---

## Model Selection

**Base Model:**  
TinyLlama/TinyLlama-1.1B-Chat-v1.0

**Reason for selection:**

- Lightweight enough for Google Colab  
- Suitable for instruction-style fine-tuning  
- Compatible with LoRA and 4-bit quantization  

---

## Fine-Tuning Approach

The model was fine-tuned using:

- PEFT (Parameter Efficient Fine-Tuning)  
- LoRA (Low-Rank Adaptation)  
- 4-bit quantization (BitsAndBytes)  

### Hyperparameters

| Parameter | Value |
|------------|--------|
| Learning Rate | 5e-5 |
| Epochs | 3 |
| Batch Size | 2 |
| Gradient Accumulation | 4 |
| Max Length | 256 |

---

## Experiment Tracking

| Experiment | Epochs | Learning Rate | Batch Size | Observed Behavior |
|------------|--------|--------------|------------|-------------------|
| Exp 1 | 2 | 5e-5 | 2 | Generic outputs |
| Exp 2 | 3 | 5e-5 | 2 | Better combination alignment |
| Final | 3 | 5e-5 | 2 | Stable and reliable responses |

---

## Evaluation

Evaluation was performed using ROUGE on a representative subset of samples.

Example ROUGE results:

- ROUGE-1: 0.20  
- ROUGE-2: 0.04  
- ROUGE-L: 0.14  

Because the model is generative, it may use different wording than the reference responses, which lowers lexical overlap. However, qualitative evaluation shows correct domain alignment and structured responses.

---

## Model Stability and Hallucination Reduction

To reduce hallucinations and prevent the model from inventing subject combinations:

- A restricted set of valid combinations was enforced.
- Post-processing cleaning rules were applied.
- Deterministic generation (no sampling) was used.

This improved response stability in the deployed UI.

---

## Deployment (Gradio UI)

A simple interactive web interface was built using Gradio.

The UI allows students to input natural language questions and receive structured responses.

Example usage:

- "Which combination is best for Pharmacy?"
- "Is MEG suitable for Accounting?"
- "What careers are linked to MCE?"

---

## How to Run

1. Install dependencies:

pip install transformers datasets peft accelerate bitsandbytes evaluate rouge_score gradio


2. Run the notebook end-to-end.
3. Launch the Gradio interface:

demo.launch()


---

## Limitations

- Small dataset size  
- Limited training epochs due to compute constraints  
- Moderate ROUGE scores due to generative variation  
- Requires guardrails to control hallucinations  

---

## Future Improvements

- Expand dataset with more careers and combinations  
- Increase training data diversity  
- Perform longer fine-tuning  
- Add continuous feedback learning  
- Improve evaluation with additional metrics  

---

## Demo Video

https://youtu.be/yLNoXxxyO7Y

---

## Example Conversations

**Q:** What careers can I pursue with MPC?  
**A:** MPC (Mathematics, Physics, Chemistry) is commonly linked to engineering, computer science, architecture, physics, and data-related fields.

**Q:** Which combination is best for Medicine?  
**A:** For Medicine, the most common combination(s) are: PCB. Medicine usually requires strong background in biology and chemistry.

**Q:** Is HEG suitable for Law?  
**A:** Yes. HEG is commonly recommended for Law. Law is commonly aligned with humanities and social science combinations.

---

## Colab badge

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tracy8/edu-career-assistant-llm/blob/main/Notebook/fine_tuning_edu_assistant.ipynb)

