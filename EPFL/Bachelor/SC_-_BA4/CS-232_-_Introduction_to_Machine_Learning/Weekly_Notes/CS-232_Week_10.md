---
Week: 10
Themes: 
aliases: 
Lecture1: true
pExercises: false
Exercises: false
---


# Convolutional neural networks !!! (Finally !!!)


Treating an image as a huge vector seems counter intuitive
- An image of reasonable size yields a huge vector, thus a huge amount of parameters in mlp
- Small image regions have similar properties and should not be processed independently
- A translation should not affect the results

## Filters

CNN's use NxN filters as learnable parameters and convolve the with original signal.
Multiple channels can be used on a single layer:
![[Pasted image 20240520125356.png|400]]
When dealing with multiple input channels, the convolutions are done in 3D
![[Pasted image 20240520130130.png|400]]
### Padding

To avoid dimentionality reduction when applying filters, we can add padding to our value matrix. This can be done by mirroring values or simply copying outermost value.

### Stride
In standard convolution the filter is applied pixel by pixel.
The shifts can be made larger by increasing stride
This "compresses" information
Drawback: using a stride too large can skip pixels / miss important information

## Pooling
When using small filters (3x3, 5x5, 7x7), to go from local representation to a more global representation, we use pooling layers.

### Different pooling strategies
- Max pooling: take max value in nxn neighborhood
- Average pooling: take average value in nxn neighborhood


![[Pasted image 20240520125755.png|400]]Operations are stackable

## Normalization layers

Inspired by the fact that data normalization can help. Several layers developed to normalize feature maps.
- Batch normalization
- Layer normalization
- Instance normalization
- Group normalization

## Finality

![[Pasted image 20240520133131.png]]

## BACKPROP

Done in same way as for mlp's, using stockastic grad desc. with mini-batches.
The gradient of pooling layers depends on strategy:
- for average pooling, gradient is propagated to all neurons in pooling
- for max pooling, gradient is propagated only to neuron that has max value