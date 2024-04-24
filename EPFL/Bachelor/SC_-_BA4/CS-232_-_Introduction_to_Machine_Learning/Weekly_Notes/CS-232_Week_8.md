---
Week: 8
Themes: 
aliases: 
Lecture1: true
pExercises: false
Exercises: false
---


- Advantages of Kernel Methods
	- Can effectively handle non-linearly-separable data
	- Often have nice solutions (e.g. closed-form, convex)
- Drawbacks of kernel methods
	- Depend on the choice of kernel and hyper-parameter(s)
	- Computational complexity depends on number of samples

## Data representations
**Problem:** 
- Making data homogeneous (example two images with different sizes).
- Data noisiness
- Some representations better suited to certain tasks then others

### Bag of words
**Idea:** For each sample count how many times each word is in dictionary
**Dictionary:** Consider only words in dataset not entire language, consider also common words (to, who, they...)

With $D$ words in the dict each sample can be represented as a vector in $\mathbb{R}^{D}$ 
#### Histograms
For each sample, divide its count by total word count to get frequency rather that occurrences to compensate for non-constant sample lengths

### Data pre-processing
#### Missing data
Potential solutions:
- Ignore corresponding sample
- Fill missing values with global constant
- Fill missing values with attribute average over dataset
- Fill in the missing value with corresponding attribute average over the samples in same class (using labels)
#### Noisy data
- Binning:
	- Sort data
	- Partition into bins
		- ![[Pasted image 20240416093219.png|200]]
	- Smooth data by taking bin means
- Clustering
	- Detecting outliers as the clusters with too few elements and remove these samples
#### Data normalization
(See slides)
#### Imbalanced data
Imbalanced data can give the illusion of good accuracy when classifying due to disproportionate class sizes
**Between-class imbalance:**
![[Pasted image 20240416094426.png|500]]
**Within-class imbalance:**
![[Pasted image 20240416094606.png|500]]
##### Sampling solution
###### Decrease the data: Undersampling
- Naive (random) undersampling
	- Randomly remove samples from largest class until you obtain balanced data
	- Drawback: may remove important samples
- More clever undersampling
	- Remove the redundant samples
	- Good if the class to undersample is densely populated
###### Oversampling
- Naive (random) oversampling
	- Replicate data samples from smallest class until you obtain a balanced data set
	- Drawback: May lead to overfitting
- More clever oversampling:
	- Create new data samples based on neighboring existing ones
		- SMOTE
		- AdaSyn
	- May create noisy samples
##### Cost-Sensitive Solution
Re-weighting samples to put more emphasis on rare cases.
Scale by $\beta =$ 1 - class frequency.

