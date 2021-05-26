# [Paper Reading 13] Importance Estimation for Neural Network Pruning  
paper link: https://openaccess.thecvf.com/content_CVPR_2019/html/Molchanov_Importance_Estimation_for_Neural_Network_Pruning_CVPR_2019_paper.html  
Molchanov et al. CVPR 2019
# Poblem Definition
## [1] Assumption and Goal
* **Structural Pruning:** Pruning is a common method to derive a compact network – after training, some structural portion of the parameters is removed, along with its associated computations. So as to reduces computation, energy, and memory transfer costs during inference.  

We propose a novel method that **estimates the
contribution of a neuron (filter) to the final loss** and iteratively removes those with smaller scores.  
Both methods scale consistently across any network
layer **without requiring per-layer sensitivity analysis** and can be applied to any kind of layer, including skip connections.  

## [2] Novelty and Contribution
* We propose a new method for estimating, with a little computational overhead over training, the contribution of a neuron (filter) to the final loss.  
* We compare two variants of our method using the first and second-order Taylor expansions, respectively, against a greedy search (“oracle”), and show that both variants achieve state-of-the-art results.  
* We also find that using a squared loss as a measure for contribution leads to better correlations with the oracle and better accuracy when compared to signed difference.  
* Pruning results on a wide variety of networks trained on CIFAR-10 and ImageNet, including those with skip connections, show improvement over state-of-the-art.  


## [3] Promising Application
* Our method is easy to implement in existing frameworks with minimal overhead.  
* Pruning with the proposed methods leads to an improvement over state-of-the-art in terms of accuracy, FLOPs, and parameter reduction.  

**Benefits of our method:**  
1. no hyperparameters to set, other than providing the desired number of neurons to prune  
2. globally consistent scale of our criterion across network layers without the need for per-layer sensitivity analysis  
3. a simple way of computing the criterion in parallel for all neurons without greedy layerbylayer computation  
4. the ability to apply the method to any layer in the network, including skip connections  

# Method/Technical Summary

## [1] Why Structural Pruning?
While larger networks have exhibited better performance, possibly due to the lottery ticket hypothesis , they have also been known to be **heavily over-parameterized**.  

## [2] Reduce the Computational Complexity
* **Network Distillation:** to train a smaller model that can mimic the output of the larger model. But need to define the architecture of the smaller distilled model beforehand.  
* **Pruning:** to remove entire filters, or neurons, that make little or no contribution to the output of a trained network.  
    * with a predefined per-layer pruning ratio  
    * simultaneously over all layers  

## [2] Exsiting Pruning Method
**The method belowed rely on the belief that the magnitude of a weight and its importance are strongly correlated.**  
* greedy algorithms: a significant computational cost
* sparse regularization
* reinforcement learning

Exact solution for Pruning:  
* Minimize the L0 norm of all neurons and remove those that are zeroed-out
    * Bayesian methods
    * Regularization terms
* Relax L0 minimization with L1 or L2 regularization, followed by soft thresholding of parameters with a predefined threshold.
    * Iterative Shrinkage and Thresholding Algorithms (ISTA)
* Regularize the scaling term (γ) of batch-norm layers and apply soft thresholding when value fell below a predefined threshold.
* Utilize pruning as a network training regularizer

## [3] Method
Include a sparsification term in the cost function to minimize the size of the model.  
![](https://i.imgur.com/HNFz6pm.png)  
![](https://i.imgur.com/DHF44gc.png)  
Unfortunately there is no efficient way to minimize the ℓ0 norm as it is non-convex, NP-hard, and requires combinatorial search.  

An alternative approach starts with the full set of parameters W upon convergence of the original optimization and gradually reduces this set by a few parameters at a time. (**greedy first-order search**)  

Under an i.i.d. assumption, this induced error can be measured as a squared difference of prediction errors with and without the parameter (wm):  
![](https://i.imgur.com/l8FWdAU.png)  

Computing Im is computationally expensive since it requires evaluating M versions of the network. So we approximating Im in the vicinity of W by its **second-order Taylor expansion**:
![](https://i.imgur.com/Gh6S84D.png)  

An even more compact approximation is computed
using the **first-order expansion**. It is easily computed since the **gradient g** is already available from backpropagation.  
![](https://i.imgur.com/znZvU76.png)  

To approximate the joint importance of a structural set of parameters Ws, e.g. a convolutional filter:  
1. as a group contribution:  
![](https://i.imgur.com/XP165QW.png)  
2. sum the importance of the individual parameters in the set:  
![](https://i.imgur.com/4hfG31n.png)  

The proposed metric, I(1), can be interpreted as the variance estimate and as the diagonal of the Fisher information matrix.  

## [4] Algorithm
During each epoch, the following steps are
repeated:  
1. For each minibatch, we compute parameter gradients and update network weights by gradient descent. We also compute the importance of each neuron (or filter) using the gradient averaged over the minibatch, as described in (7) or (8). (Or, the second-order importance estimate may be computed if the Hessian is available.)  
2. After a predefined number of minibatches, we average the importance score of each neuron (or filter) over the of minibatches, and remove the N neurons with the smallest importance scores.  
Fine-tuning and pruning continue until the target number of neurons is pruned, or the maximum tolerable loss can no longer be achieved.  

## [5] Implementation Details
You can read these details in the original paper.  
* Hessian computation
* Importance score accumulation
* Importance score aggregation
* Gate placement
* Averaging importance scores over pruning iterations
* Pruning strategy
* Number of minibatches
* Number of neurons

## [6] Dataset
* CIFAR-10
* ImageNet

# Experiment Results
![](https://i.imgur.com/i1P19c7.png)  

![](https://i.imgur.com/vh7nJcx.png)  

![](https://i.imgur.com/k22pCVU.png)  

![](https://i.imgur.com/ZbVFKI1.png)  
![](https://i.imgur.com/ivKePYN.png)  
![](https://i.imgur.com/jKJgSl6.png)  

# Discussion
We demonstrated that even the first-order approximation shows significant agreement with true importance, and outperforms prior work on a range of deep networks.  
We showed that applying the first-order criterion after batch-norms yields the best results, under practical computational and memory constraints.  

## [1] Question
* Can this method be expand to use on GAN or RL network? (since it is very hard to train)

