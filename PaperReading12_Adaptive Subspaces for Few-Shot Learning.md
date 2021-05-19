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
* Data Labeling Issue:
    * few labeled data
    * precise annotation ex.annotating bounding boxes of object detection
    * labeling with expert knowledge ex. sign language recognition

* Humans can learn new objects from only a few examples. This in turn provides humans with lifelong learning abilities.  

* Some exist method:
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

* **Pair-Wise Classifier:** To build a classifier directly from samples by calculating the similarity between them. It does not poses an invariance w.r.t. the order of input images which affects the accuracy.  
* **Prototype Classifier:** The parameters from the last fullyconnected layer and prototypes correlate. Thus, the classifier is generated from the prototypes. This approach preserves the symmetric property (invariance to order of images) because the average operation is performed to generate the classifier.  
* **Non-Linear Binary Classifier:** This approach exploits the non-linearity of the decision boundaries. Relational networks use a non-linear binary classifier to calculate the similarity. Even though this classifier does not use a softmax function, it follows the principle of a generating classifier that learns a comparison of datapoint pairs.  
* **Subspace Classifiers:** See under.

![](https://i.imgur.com/ZG2sPff.png)  

## [4] Subspace Classifiers
* **Subspace Classifiers:** One of the classification methods on a subspace is to find the closest distance between the datapoint to its projection onto the subspace. We define the probability of the query assigned to class cusing a softmax function as:  
![](https://i.imgur.com/aswvI2v.png)  
![](https://i.imgur.com/qWwLWPD.png)  

* **Discriminative Deep Subspace Networks:** Make use of the Grassmannian geometry and propose to maximize the distance between subspaces during training.  

* The steps of training DSN(Deep Subspace Networks):  
![](https://i.imgur.com/1wPQOHY.png)  
![](https://i.imgur.com/LxuNBVo.png)  

* **DSN for SemiSupervised FewShot Learning:** We need to take advantage of the unlabeled data to fit better subspaces to our data. We achieve this by refining the center of each class (mean-refinement) according to:  
![](https://i.imgur.com/bwvwV4R.png)  
![](https://i.imgur.com/tCa2cc7.png)  
To work at the presence of distractors, we use a fake class with zero mean.  

## [5] Dataset
* CUB dataset
* ImageNet (universal feature extraction)
    * mini-ImageNet
    * tiered-ImageNet
* CIFAR-100
* Open MIC

# Experiment Results
![](https://i.imgur.com/FT4sjCL.png)  
![](https://i.imgur.com/I5ON2t3.png)  

![](https://i.imgur.com/B06btpI.png)  

![](https://i.imgur.com/WlAh8TL.png)  

![](https://i.imgur.com/1ITGypy.png)  

# Discussion
## [1] Ablation Study
* **Discriminative Term:** The discriminative term in Eq. 4 encourages the orthogonality between subspaces of different classes. The discriminative term gives a performance boost and results in more discriminative subspaces for classification.  
* **Subspace Dimensionality:** We observe that the choice of n from 2 to K − 1 does not affect the performance significantly (± 0.5%) on mini-ImageNet using Conv-4 backbone.  

## [2] Discussion
* **Robustness to Perturbations:** We observed in our experiments that the performance for standard methods degrades significantly with a small degree of perturbations added to signal. However, our subspace-based model handles such a noise well.  
* **Computational Complexity:** Compare to prototype method, our method is somewhat slower due to the use of the SVD step. However, to address the complexity of SVD, fast approximate SVD algorithms can be used.  

## [1] Question
* Can model extract the feature from the sample and do some denoise based on the feature at the same time?  
