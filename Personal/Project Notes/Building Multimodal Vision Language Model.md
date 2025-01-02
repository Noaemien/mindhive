Source video: https://www.youtube.com/watch?v=vAmKB7iPkWw


## Contrastive vision encoder
Converts image into series of embeddings.
Image is split into blocks and blocks are converted to embeddings

### Contrastive learning
A way to train Text encoder and Image encoder.
Images with corresponding text descriptions are encoded using their corresponding encoder. 
We then perform a dot product of the embedding. Embeddings of corresponding inputs are expected to return high values whilst low values are expected for non corresponding input values.

Uses cross entropy loss
![[Contrastive Learning.png|400]]
Cross entropy uses the soft-max function to convert rows into probabilities.
To prevent explosion due to exponential function softmax is rewritten as: 
$$softmax_{i} = \frac{e^{a_{i}}}{\sum_{j}^{n}e^{a_{ij}}} = \frac{ce^{a_{i}}}{c\sum_{j}^{n}e^{a_{ij}}} = \frac{e^{\log(c)}e^{a_{i}}}{e^{\log(c)}\sum_{j}^{n}e^{a_{ij}}} = \frac{e^{a_{i} + \log(c)}}{\sum_{j}^{n}e^{a_{ij} + \log(c)}} \\ \text{ where } c = -\underset{i}{\max}(a_{i})$$
Using cross-entropy loss is therefore not very parallelisable and quite computationally heavy (~20 min maybe watch again for more intuition).


Cross entropy loss can be replaced with sigmoid loss.
-> Each dot product between image embedding and text embedding is considered as an individual binary classification task where 1 is a match and 0 is a mismatch. 
This is highly parallelisable.


