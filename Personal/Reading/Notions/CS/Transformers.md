
Follows an encoder/decoder architecture.

## Encoder
![[Transformer_Encoder.png|200]]

### Input Processing
#### Embeddings
Input sequence is split into tokens.

$$Input = \text{The quick brown fox}$$
In common practice, subword tokenization is used as it allows for smaller vocabulary size but maintains a lot of word information. 
For illustrative purposes it is easier to imagine them as words.
$$\text{Tokenized Input} = \begin{cases}
t_{1}= \text{The} \\
t_{2}= \text{quick} \\
t_{3}= \text{brown} \\
t_{4}= \text{fox} \\
\end{cases}$$
Each Token is mapped to id / position in vocabulary.
ID is mapped to vector embedding of size $d_{model}$.
$$ t_{i}\longrightarrow id_{t_{i}} \longrightarrow e_{id_{t_{i}}}$$
$$\text{where }e = \begin{bmatrix}
e_{0} & e_{1} & \dots & e_{d_{model} - 1}
\end{bmatrix}\in \mathbb{R}^{d_{model}}$$
Embeddings are the same for identical tokens but may change over time as they are optimized by transformer.
They can be imagined as encoding meaning in a continuous, dimensional space.
#### Positional encodings

A way to let tokens carry positional information from sequence.
This way model can treat token that are close as "close" and tokens that are far from each other as "far"
In the source paper, positional encodings are computed as follows:
$$PE(pos, 2i) = \sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}})$$
$$PE(pos, 2i+1) = \cos(\frac{pos}{10000^{\frac{2i}{d_{model}}}})$$
Meaning that for our first token $t_{0}$ we would get:
$$pe_{1} = \begin{bmatrix}
PE(1, 1)  = 0.0001 \\ PE(1, 2)\\ \dots \\ PE(1, d_{model})
\end{bmatrix} \in \mathbb{R}^{d_{model}}$$
These values are fixed and do not change throughout training.

Positional encodings are summed to embeddings giving final input to model.
### Attention
#### Self-Attention
Self-attention is a way for the model to assign relations between queries $Q$ and keys $K$ and then maps it to values $V$.
$$Attention(Q, K, V) = \text{softmax}(\frac{QK^{T}}{\sqrt{d_{k}}})V$$
