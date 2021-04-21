# [Paper Reading 8] Image Denoising Via Sparse and Redundant Representations Over Learned Dictionaries  
paper link: https://ieeexplore.ieee.org/document/4011956  
Elad and Aharon. Image Processing, IEEE Transactions on Image Processing (2006) vol. 15 (12) pp. 3736 – 3745.
# Poblem Definition
## [1] Assumption and Goal
The **image denoising problem**: where
zero-mean white and homogeneous Gaussian additive noise is to be removed from a given image.  

## [2] Novelty and Contribution
* The approach taken is based on **sparse and redundant representations** over **trained dictionaries**. Using the **K-SVD algorithm**, we obtain a dictionary that describes the image content effectively.  
* we extend **K-SVD algorithm**'s deployment to arbitrary image sizes by defining a global image prior that forces sparsity over patches in every location in the image.  
* **Bayesian treatment** leads to a simple and effective denoising algorithm.  

## [3] Promising Application
This leads to a state-of-the-art denoising performance, equivalent and sometimes surpassing recently published leading alternative denoising methods.

# Method/Technical Summary

## [1] Sparseland Model
For the construction of the Sparseland model, we need to define a dictionary (matrix) of size:![](https://i.imgur.com/ikDDuI9.png) (with k > n, implying that it is redundant).  
Every image patch, could be represented sparsely over this dictionary:  
![](https://i.imgur.com/GgfP22a.png)


## Architecture

## Dataset

# Experiment Results

# Discussion

## Question
* 