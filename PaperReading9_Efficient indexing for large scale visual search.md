# [Paper Reading 9] Efficient Indexing for Large Scale Visual Search  
paper link: https://ieeexplore.ieee.org/document/5459354  
Xiao Zhang et al., ICCV 2009.

# Poblem Definition
## [1] Assumption and Goal
**Image query:** Retrieving similar images to a given image(s) from a huge image database.  
Due to a fundamental difference between an image query (e.g. 1500 visual terms) and a text query (e.g. 3-5 terms), the usages of some text indexing techniques, e.g. inverted list, are misleading. In this work, we develop a novel indexing technique for this problem.  
The basic idea is to **decompose** a document-like representation of an image into two components, one for **dimension reduction** and the other for **residual information preservation**.  

## [2] Novelty and Contribution
* Develop a novel indexing technique for this problem.  
* The decomposition has two major merits:  
    * These components have good properties which enable them to be efficiently indexed and retrieved;  
    * The decomposition has better generalization ability than other dimension reduction algorithms.  
* Theoretic analysis and extensive experiments over a 2.3 million image database show that this framework is scalable to index large scale image database.  

## [3] Promising Application
* To support fast and accurate visual search.  

# Overview of Existing Image Retrieval Systems
A typical local feature-based large-scale image retrieval system could be divided into three parts: **feature extraction and quantization**, **indexing** and **postprocessing**.  

## [1] feature extraction and quantization
Feature extraction and quantization is to convert an
image into “bag of visual terms” (BOV) representation. An image is usually represented
by 500-5000 local descriptors.  

* k-means clustering  
* soft assignment  
* hamming embedding  

## [2] indexing
To speed up the process of finding relevant images for a query image, indexing techniques are necessarily adopted to organize images.  

* inverted list structure  

However, a fundamental difference between an image
query and a text query is largely ignored in existing index design. Thus, when inverted list is applied to index images, both its **storage cost** and **time complexity** to respond a query are unacceptably high.  

## [3] postprocessing
After an initial candidate set of images is obtained, many existing computer vision techniques can be applied to rerank images in it.  

* RANSAC-based fast image matching
* query expansion-based high recall retrieval

# High-dimensional indexing techniques
Existing high-dimensional indexing
techniques:  

* Locality Sensitive Hashing (LSH)
* KD-tree

## [1] Dimension reduction algorithms
* By capturing high level semantics, the projected low dimensional space could facilitate information retrieval.  
* High-dimensional indexing technique such as KD-tree or LSH performs better in the projected space, because image feature vectors in this space are compact and dense.  

# Method/Technical Summary
The proposed solution consists of a feature decomposition model and a specifically designed indexing scheme.  

## [1] Image Decomposition
We propose a compound image representation pi which consists of a lowdimensional vector hi and some important residual information lost in dimension reduction.  

![](https://i.imgur.com/rpQ12pK.png)  
![](https://i.imgur.com/C2rpzv5.png)  

A way to implement the proposed decomposition idea is to use an Image Decomposition Model(IDM), which is an extension of the popular LDA model.  

## [2] Indexing and Ranking

**[Indexing]**  

![](https://i.imgur.com/LJoZC5C.png)  

Choosing different approaches to index different kinds of image representations.  

* **Background Visual Terms:** are common for all images in a database, that is, they are discarded from index.
* **Indexing Low-dimensional Representations:** Locality Sensitive Hashing (LSH)  
* **Indexing Image-Specific Words:** preserve only a small amount of visual words of an image as its image-specific words.

**[Ranking]**  
A query image q is processed as follows:  

![](https://i.imgur.com/YkMCupY.png)  

## [3] Dataset
Four datasets (including two benchmark databases) were used in our experiments.  

* Oxford5K
* Paris
* PhotoForum2M
* PhotoForum7K

# Experiment Results
SIFT features were chosen as local features to represent images.  

![](https://i.imgur.com/5XsfzkE.png)  

![](https://i.imgur.com/C5NUwdz.png)  

![](https://i.imgur.com/HHpARnN.png)  

![](https://i.imgur.com/cnBIN7L.png)  



# Discussion

## [1] Conclusion
* Both the computational and memory cost of online search service are significantly reduced, while the search results are almost as good as the accurate algorithm.  

## [2] Question
* Can we take it as a new dimension reduction algorithms, and apply it on other dimension reduction appliactions?

