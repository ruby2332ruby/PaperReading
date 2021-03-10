# [Paper Reading 4] SphereFace: Deep Hypersphere Embedding for Face Recognition  
paper link: https://openaccess.thecvf.com/content_cvpr_2017/html/Liu_SphereFace_Deep_Hypersphere_CVPR_2017_paper.html  
Liu et al. CVPR 2017
# Poblem Definition
## Assumption and Goal
This paper addresses deep face recognition (FR) problem under open-set protocol, where ideal face features are expected to have smaller maximal intra-class distance than minimal inter-class distance under a suitably chosen metric space.
* **met-ric space (賦距空間) :** 在數學中，賦距空間是一個集合及其度量，該度量是一個函數，定義了集合中任兩個成員（通常我們稱為"點"）間的距離這一概念。賦距空間中最符合人們對於現實直觀理解的為三維歐幾里得空間。  
    * 每個點和自己的距離為0，
    * 任兩點間的距離為正數，
    * 從A到B的距離，等同於從B到A的距離，
    * 從A到B的距離小於等於從A先經過C再到B的距離。  
(資料來源: 維基百科 https://zh.wikipedia.org/wiki/%E5%BA%A6%E9%87%8F%E7%A9%BA%E9%97%B4)  

However, few existing algorithms can effectively
achieve this criterion. To this end, we propose the angular softmax (A-Softmax) loss that enables convolutional neural networks (CNNs) to learn angularly discriminative features.  

## Novelty and Contribution
* Propose A-Softmax loss for CNNs to learn discriminative face features with clear and novel geometric interpretation. The learned features discriminatively span on a hypersphere manifold, which intrinsically matches the prior that faces also lie on a manifold.
* Derive lower bounds for m such that A-Softmax loss can approximate the learning task that minimal interclass distance is larger than maximal intra-class distance.
* We are the very first to show the effectiveness of angular margin in FR. Trained on publicly available CASIA dataset , SphereFace achieves competitive results on several benchmarks, including Labeled Face in the Wild (LFW), Youtube Faces (YTF) and MegaFace Challenge 1.

## Promising Application
This connection makes A-Softmax very effective for learning face representation. Competitive results on several popular face benchmarks demonstrate the superiority and great potentials of this approach.

# Method/Technical Summary

## Face Recognition
Face recognition can be categorized as face identification and face verification. The former classifies a face to a specific identity, while the latter determines whether a pair of faces belongs to the same identity.  
In terms of testing protocol, face recognition can be evaluated under closed-set or open-set settings, as illustrated .
![](https://i.imgur.com/UDyGp3D.png)
* **Closed-set Protocol**  
All testing identities are predefined in training set. In this scenario, face verification is equivalent to performing identification for a pair of faces respectively.Therefore, closed-set FR can be well addressed as a classification problem, where features are expected to be separable.
* **Open-set Protocol**  
The testing identities are usually disjoint from the training set, which makes FR more challenging yet close to practice. Since it is impossible to classify faces to known identities in training set, we need to map faces to a discriminative feature space.  
**Desired features for open-set FR are expected to satisfy the criterion that the maximal intra-class distance is smaller than the minimal inter-class distance under a certain metric space.**

## Formulate the Criterion(Open-set Protocol) in Loss Functions
* **Softmax Loss**  
But softmax loss only learns separable features that are not discriminative enough.
* **Softmax Loss + Contrastive Loss/Center Loss**  
To enhance the discrimination power of features. However, center loss only explicitly encourages intra-class compactness. Both contrastive loss and triplet loss can not constrain on each individual sample, and thus require carefully designed pair/triplet mining procedure, which is both time-consuming and performance-sensitive.
* **Euclidean Margin Based Loss**  
In some sense, Euclidean margin based losses are incompatible with softmax loss, so it is not well motivated to combine these two type of losses.
*  **A-Softmax loss**  
A-Softmax loss that enables convolutional neural networks (CNNs) to learn angularly discriminative features.

## A-Softmax loss
A-Softmax loss can be viewed as imposing discriminative constraints on a hypersphere manifold, which intrinsically matches the prior that faces also lie on a mani-fold.  
* A-Softmax loss defines a large angular margin learning task with adjustable difficulty(m).
* Lower bound of mmin in binary-class case is 2+√3.
* Lower bound of mmin in multi-class case is 3.  
![](https://i.imgur.com/7CmRzYd.png)


# Experiment Results
![](https://i.imgur.com/1aweWPL.png)  
![](https://i.imgur.com/eYogtx4.png)


# Discussion
## Why angular margin.
Incorporating angular margin to softmax loss is actually a more natural choice
## Comparison with existing losses.
A-Softmax loss requires no sample mining and imposes discriminative constraints to the entire mini-batches.

## Question
* Is it posible for a feature set on the center of the sphere?
