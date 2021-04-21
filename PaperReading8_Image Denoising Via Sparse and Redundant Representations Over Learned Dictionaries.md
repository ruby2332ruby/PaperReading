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

## #FROM LOCAL TO GLOBAL BAYESIAN RECONSTRUCTION

## [1] Sparseland Model
For the construction of the Sparseland model, we need to define a dictionary (matrix) of size:  
![](https://i.imgur.com/x8uz2VI.png)  
 (with k > n, implying that it is redundant).  
 
Every image patch, could be represented sparsely over this dictionary:  
![](https://i.imgur.com/OI4n5QK.png)  

Adding more:
* bounded representation error: ![](https://i.imgur.com/jqo00yx.png)  
* define how deep is the required sparsity: ![](https://i.imgur.com/Q9tSeNh.png)  

The MAP estimator for denoising this image patch is
built by solving:  
![](https://i.imgur.com/khZZJdi.png)  
or  
![](https://i.imgur.com/MrfxiXK.png)  

## [2] From Local Analysis to a Global Prior
* question arises concerning the use of such a **small dictionary**:  
    1. When training takes place (as we will show in the next section), only small dictionaries can be composed.  
    2. A small dictionary implies a locality of the resulting algorithms, which simplifies the overall image treatment.  

If our knowledge on the unknown large image is fully expressed in the fact that every patch in it belongs to the Sparseland model, then the natural generalization of the above MAP estimator is the replacement of with:  

![](https://i.imgur.com/yB6K0qY.png)  

## [3] Numerical Solution
We propose a block-coordinate minimization algorithm that starts with an initialization  X = Y, and then seeks the optimal aij.  
![](https://i.imgur.com/h5OsaX5.png) 

Given all aij, we can now fix those and turn to update X. We need to solve:  
![](https://i.imgur.com/Ifye2ow.png)  

## #EXAMPLE-BASED SPARSITY AND REDUNDANCY

## [1] Training on the Corpus of Image Patches
Given a set of image patches, and assuming that they emerge from a specific Sparseland model, we would like to estimate this model parameters.  
![](https://i.imgur.com/DZrfHz9.png)  

The K-SVD proposes an iterative algorithm designed to handle the above task effectively.

## [2] Training on the Corrupted Image
![](https://i.imgur.com/nkeUeKz.png)  

## #ARCHITECTURE
Denoising procedure using a dictionary trained on patches from the corrupted image.  

![](https://i.imgur.com/So1SVrO.png)  
![](https://i.imgur.com/3x1X0Ds.png)  
![](https://i.imgur.com/c4gA1h5.png)  

# Experiment Results

## [1] Denoising Results for the DCT Dictionary
The DCT dictionary, the globally trained dictionary, and training on the corrupted images directly (referred to hereafter as the adaptive dictionary).  
![](https://i.imgur.com/luiOhVL.png)  

## Dictionary
![](https://i.imgur.com/hsI3RNR.png)  

# Discussion
* The content of the dictionary is of prime importance for the denoising process—we have shown that a dictionary trained for natural real images, as well as an adaptive dictionary trained on patches of the noisy image itself, both perform very well.  
* **Future work**:
    * using several dictionaries and switching between them by content
    * optimizing the parameters
    * replacing the OMP by a better pursuit technique
    * to be promising is a generalization to multiscale dictionaries
    * extend this work to multiscale dictionaries, as it is clear that K-SVD cannot be directly deployed on larger blocks.

## Question
* Can noise patern also be written in dictionary?