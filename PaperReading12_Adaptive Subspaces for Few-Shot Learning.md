# [Paper Reading 12] Adaptive Subspaces for Few-Shot Learning  
paper link: https://openaccess.thecvf.com/content_CVPR_2020/papers/Simon_Adaptive_Subspaces_for_Few-Shot_Learning_CVPR_2020_paper.pdf  
Simon et al. CVPR 2020.
# Poblem Definition
## [1] Assumption and Goal
Object recognition **with extremely few samples** requires a generalization capability to **avoid overfitting**. Usually studied under the umbrella of meta-learning.  
In this paper, it provided a framework for few-shot
learning by introducing dynamic classifiers that are constructed from few samples. A subspace method is exploited as the central block of a dynamic classifier.  


## [2] Novelty and Contribution
* Few-shot learning solutions are formulated within a framework of generating dynamic classifiers.
* Propose an extension of existing dynamic classifiers by using subspaces. We rely on a wellestablished concept stating that a second-order method generalizes better for classification tasks.
* Also introduce a discriminative formulation where maximum discrimination between subspaces is encouraged during training. This solution boosts the performance even further.
* We show that our method can make use of unlabeled data and hence it lends itself to the problem of semisupervised few-shot learning and transductive setting. The robustness of such a variant is assessed in our experiments.
* Code is available at https://github.com/chrysts/dsn_fewshot  

## [3] Promising Application
* A robust method againsts perturbations (e.g., outliers) and yields competitive results on the task of supervised and semi-supervised few-shot classification.  
* Also develop a discriminative form which can boost the accuracy even further.  

# Method/Technical Summary

## [1] Few-Shot Learning (FSL)
* **Data Labeling Issue:**
    * few labeled data
    * precise annotation ex.annotating bounding boxes of object detection
    * labeling with expert knowledge ex. sign language recognition

* **Humans can learn new objects from only a few examples. This in turn provides humans with lifelong learning abilities.**  

* **Some exist method:**
    * embedding learning
    * adaptation techniques
    * **generative models and similarity learning:** capture the variation within parts and geometric configurations of objects.(hand-crafted features)
    * **constellation model:** takes into account the object parts for inference.
    * **Meta-learning:** Used to obtain fast adaptive networks. To learn initial values for the parameters (weights) of the neural network, we can expect the network to adapt to different tasks using backpropagation from limited samples.  
        * LSTM
        * MAMLm MAML++
        * MetaNets
    * **metric-learning**
    * **few-shot semi-supervised learning (FS-SSL):** unlabeled images help samples from the support set to increase the performance of few-shot classification.

* **Symmetric Function:** a requirement to learn the classifier from high-dimensional data.  
    * **Prototype**: (old way) Averaging all the samples within each class.
    * **Subspace**: (proposed way)

## [2] Terminology
* **episodes:** consists of two sets, the support set S and the query set Q. (semisupervised setting: +unlabeled set R)  
* **N-way K-shot classification:** the deep embeddings learn with limited amount of labels and inputs per episode. (e.g., 20-way 1-shot and 5-way 5-shot)  
* **distractor:** samples from distractor classes are irrelevant to the classification task and represent classes outside the support set.  

## [3] FSL Learning Paradigm
Many state-of-the-art FSL techniques fit nicely into such a learning paradigm.  
1. **feature extractor:** learning a universal feature extractor
2. **dynamic classifier:** learning to generate a classifier dynamically from limited data

## [1] Architecture

## [2] Dataset
* CUB dataset
* ImageNet (universal feature extraction)

# Experiment Results

# Discussion

## [1] Question
* 
