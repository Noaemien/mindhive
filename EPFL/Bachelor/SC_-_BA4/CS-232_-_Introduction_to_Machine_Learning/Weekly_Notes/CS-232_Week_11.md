---
Week: 11
Themes: 
aliases: 
Lecture1: true
pExercises: false
Exercises: false
---

# NLP's

## Input representation
- Bag of words: 
	- Not very precise
	- Okay for sentiment analysis
- Naive approach:
	- Each word is represented by a sparse vector in dict
	- Limitations:
		- No notion of similarity between words
- Word embeddings
	- Represent each word as a high dimentional, real valued vector
	- Similarity of vector represents similarity of meaning in words
	- Representation is learned
- Sequence to sequence model
	- Instead of modeling words, model sequence or document as a sequence
		- each individual word still represented as vector

### How to train semantics-encoding embedding of words
- Supervised
	- Optimise embedings for specific task
	- Requires supervised data, may not generalize to other tasks
- Unsupervised
	- "you shall know a word by the company it keeps" 

# RNN

![[Pasted image 20240520154235.png|500]]

![[Pasted image 20240520154434.png|500]]
# Transformers
