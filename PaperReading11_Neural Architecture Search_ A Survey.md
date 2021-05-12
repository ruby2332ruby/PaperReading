# [Paper Reading 11] Neural Architecture Search: A Survey  
paper link: https://arxiv.org/abs/1808.05377  
Elsken et al. JMLR 2019.  

# Poblem Definition
## [1] Assumption and Goal
Deep Learning often progress because someone design a **novel neural architectures**. Currently employed architectures have mostly been developed manually by human experts, which is a time-consuming and error-
prone process. More and more people getting interest in **automated neural architecture search methods**.  

## [2] Novelty and Contribution
* Provide an overview of existing work in this field of research.  
* Categorize them according to three dimensions:  
    1. search space
    2. search strategy
    3. performance estimation strategy

# Technical Summary

## [1] Neural Architecture Search (NAS)
**Neural Architecture Search** is the process
of automating architecture engineering. NAS can be
seen as subfield of AutoML and has significant overlap with hyperparameter optimization and meta-learning.  
NAS methods have outperformed manually designed architectures on some tasks. (image classification, object detection, semantic segmentation)  

## [2] Categorize Methods for NAS
Categorize methods for NAS according to three dimensions:  
1. **Search Space:** The search space defines which architectures can be represented in principle. Prior knowledge can reduce the size of the search space, but introduce a human bias at the same time.  
2. **Search Strategy:** The search strategy details how to explore the search space (which is often exponentially large or even unbounded).  
3. **Performance Estimation Strategy:** Performance Estimation refers to the process of estimating this performance: the simplest option is to perform a standard training and validation of the architecture on data.  
![](https://i.imgur.com/kOomOaE.png)  

## [3] Search Space
The **search space** defines which neural architectures a NAS approach might discover in
principle.  
* **Chain-structured neural networks:** A layer receives input from previous layer and pass its output to the next layer. The search space is then parametrized by:  
    1. the (maximum) number of layers n  
    2. the type of operation every layer executes  
    3. hyperparameters associated with the operation  
![](https://i.imgur.com/35uMDdV.png)  

* **Multi-branch networks:** Incorporates modern design elements known from hand-crafted architectures, such as skip connections.  
    * chain-structured networks
    * Residual Networks
    * DenseNets  
![](https://i.imgur.com/zurg4jd.png)  

* **Cell search space:** Optimize two different kind of cells: **a normal cell** that preserves the dimensionality of the input and **a reduction cell** which reduces the spatial dimension.  
There are three major advantages:  
    1. The size of the search space is drastically reduced since cells usually consist of signif-
icantly less layers than whole architectures.  
    2. Architectures built from cells can more easily be transferred or adapted to other data sets by simply varying the number of cells and filters used within a model.  
    3. Creating architectures by repeating building blocks has proven a useful design principle in general, such as repeating an LSTM block in RNNs or stacking a residual block.  

    A normal cell (top) and a reduction cell (bottom). Right: an architecture built by stacking the cells sequentially.  
![](https://i.imgur.com/dRXOlJH.png)  
But how to choose the macro-architecture: how many
cells shall be used and how should they be connected to build the actual model?

## [4] Search Strategy
* **Random search**
* **Bayesian optimization:** the first automatically-tuned neural networks to win on competition data sets against human experts. But it has not been applied to NAS by many groups since typical BO toolboxes are based on Gaussian processes and focus on low-dimensional continuous optimization problems.  
* **Reinforcement learning (RL):** The generation of a neural architecture can be considered to be the agent's action, with the action space identical to the search space.  
* **Evolutionary methods:** Use evolutionary algorithms for optimizing the neural architecture.  
* **Hill climbing algorithm:** Monte Carlo Tree Search.  
* **Gradient-based methods** Previous methods are in discrete search space, Liu et al. (2019b) propose a continuous relaxation to enable direct gradient-based optimization: compute a convex combination from a set of operations.  

## [5] Performance Estimation Strategy
To guide the search process, these strategies need to estimate the performance of a given architecture.  
* **Simplest way:** To train the given architecture on training data and evaluate its performance on validation data. But demands in the order of thousands of GPU days.  
* **Speed-up methods:**

![](https://i.imgur.com/1bVd8pR.png)  

# Discussion

## [1] Future Directions
* Most existing work has focused on NAS for image classification. On the one hand, this provides a challenging benchmark. On the other hand, it is relatively easy to define a well-suited search space by exploiting knowledge from manual engineering. NAS might not find architectures that substantially outperform existing ones considerably.   
* Notable first steps have been taken in image restoration image restoration, semantic segmentation, transfer learning, machine translation, reinforcement learning, optimizing recurrent neural networks.  
* Further promising application areas for NAS would be generative adversarial networks or sensor fusion.  
* Develop NAS methods for multi-task problems and for multi-objective problems.  
* To extend RL/bandit approaches.
* Defining more general and exible search spaces.  
* Identifying common motifs, providing an understanding why those motifs are important for high performance, and investigating if these motifs generalize over different problems would be desirable.  

## [2] Question
* Is there any way to know that the prior knowledge you add on Search Space is an efficient constrain or a human bias? (ex. observe from Performance Estimation, sample and compare some network architecture)

