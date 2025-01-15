---
id: 6
Title: Transformer^2
published-date: 2025-01-14
Author:
  - Qi Sun
  - Edoardo Cetin
  - Yujin Tang
lecture-date: 2025-01-15
rating: -1
Link: https://arxiv.org/pdf/2501.06252
---
## Summary:

Self-adaptive llm that adapts in real-time to unseen tasks.
Inference uses a two pass system, with a first path to identify task properties and a second pass which uses task specific weights.
Outperforms approaches like LoRA with fewer parameters and better efficiency.


## Introduction
Traditional post-training approach: optimize model for wide range of capabilities in a single training session. -> difficult to achieve. Performance trade-offs when adding new data, overfitting and task interference.

Self-adaptive models:
Develop "expert" modules offline and augmented to base LLM on-demand. -> Behavior based on task. Enables continual learning, allowing model to learn new tasks without forgetting old ones.
Similar to how the actual brain works. Brain activates specific regions depending on task and dynamically optimizes neuronal connections based on task demands.

Previous limitations:
- Fine-tuning LLM to create expert modules results in big increase in parameters.
	- Even with efficient methods like LoRA, cumulative size of expert modules scales rapidly.
- Expert modules are likely to overfit, especially on smaller datasets or small task domains.
- Flexible composition of expert modules is an unresolved challenge in current research.

Suggestions:
- Singular Value Fine-tuning (SVF)
	- a parameter-efficient fine-tuning (PEFT) method
	- Extract and tune only singular values within weight matrices. 
	- Reduces risk of overfitting.
	- Reduces computational demands.
	- Allows for compositionally.

![[Pasted image 20250115114613.png|500]]


Transformer$^2$ framework:
- Two pass inference mechanism:
	- Execute the model and observe its behavior, gather information to understand necessary skills for current problem.
	- Use this information to combine expert vectors and provide new modification to base weights based on previous observations.
- Trainable with RL on small datasets

## Methods
#### Preliminaries:
- SVD
- Cross-entropy method (CEM)

