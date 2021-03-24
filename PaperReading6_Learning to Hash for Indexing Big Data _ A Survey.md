# [Paper Reading 6] Learning to Hash for Indexing Big Data – A Survey  
paper link: https://ieeexplore.ieee.org/document/7360966  
Wang et al. Proceedings of the IEEE, 2016.  
# Poblem Definition
## Novelty and Contribution (Goal)
* Provide readers with systematic understanding of insights, pros and cons of the emerging techniques.  
* Provide a comprehensive survey of the learning to hash framework and representative techniques of various types, including unsupervised, semi-supervised, and supervised.  
* Summarize recent hashing approaches utilizing the deep learning models.  
* Discuss the future direction and trends of research in this area.  

# Technical Summary
Due to the dramatic increase in the size of the data, modern information technology infrastructure has to deal with such gigantic databases. In fact, compared to the cost of storage, searching for relevant content in massive databases turns out to be even a more challenging task. But, the straightforward solution using exhaustive comparison is infeasible due to the prohibitive **computational complexity** and **memory requirement**.  

## Nearest Neighbor Search
給一筆data作為query，要從dataset中找到相似的data example。  
* **Exhaustively Comparing:** Exhaustively comparing a query point q with each sample in a database X. High computational cost, storage constraint originating from loading original data into memory.  
* **Approximate Nearest Neighbors (ANNs):** ANN promises performance in both efficiency and accuracy, and it is often sufficient for many practical applications.  
* **Tree-based Indexing:** Such as KD tree, ball tree, metric tree, and vantage point. However, tree-based approaches require significant storage costs and its performance dramatically degrades when handling high-dimensional data.  
* **Hashing:** Hashing methods repeatedly partition the entire dataset and derive a single hash ’bit’2 from each partitioning.  
    * Binary partitioning based hashing  
    * Locality-Sensitive Hashing (LSH)  

## Pipeline of Hashing-based ANN Search
There are three basic steps in ANN search using hashing techniques:  
1. **Designing Hash Functions:** the most commonly used hash functions are of the form of a generalized linear projection:  
![](https://i.imgur.com/3PZBUAt.png)  
2. **Indexing Using Hash Tables:** the hash codes of the database are organized as an inverse-lookup, resulting in a **hash table** or a **hash map**. A hash table can be seen as an inverse-lookup table, which can return all the database items corresponding to a certain code in constant time.  
3. **Online Querying with Hashing:** The query is first converted into a code using the same hash functions that mapped the database items to codes. One way to find nearest neighbors of the query is by computing the Hamming distance between the query code to all the database codes.  

## Randomized Hashing Methods
* **Random Projection Based Hashing**  
![](https://i.imgur.com/719oV5g.png)  
* **Random Permutation based Hashing**  

## Learning Based Hashing Methods
Design of improved data-dependent hash functions
has been the focus in learning to hash paradigm.  
* **Data-Dependent vs. Data-Independent**  
    * data-independent approaches: random projection, e.g. LSH, SIKH.  
	* data-dependent approaches: 再細分成下一項三種  
* **Unsupervised, Supervised, and Semi-Supervised (Data-dependent)**  
	* **Unsupervised:** unsupervised hashing methods attempt to integrate the data properties, such as distributions and manifold structures to design compact hash codes with improved accuracy.  
		* spectral hashing  
        * graph hashing  
        * manifold hashing  
        * iterative quantization hashing  
        * kernalized locality sensitive hashing  
        * isotropic hashing  
        * angular quantization hashing  
        * spherical hashing  
	* **Supervised:** supervised learning paradigms ranging from kernel learning to metric learning to deep learning have been exploited to learn binary codes.  
	* **Semi-supervised:** semi-supervised learning paradigm was employed to design hash functions by using both labeled and unlabeled data.  
* **Pointwise, Pairwise, Triplet-wise and Listwise**  
	* pairwise labels  
    ![](https://i.imgur.com/kmTmn0c.png)  
	* a triplet  
	![](https://i.imgur.com/OtSmANe.png)  
	* a distance based rank list to a query point  
	![](https://i.imgur.com/VpnH7uf.png)  
* **Linear vs. Nonlinear**  
	* linear:  
        * random projection based LSH  
        * Linear Discriminant Analysis  
	* nonlinear:  
		* Spectral Hashing  
		* Anchor graph hashing  
		* Kernerlized LSH  
* **Single-Shot Learning vs. Multiple-Shot Learning**  
	* **single-shot learning:** the optimal solution is derived by optimizing the objective function in a single-shot.  
	* **multiple-shot learning:** the given label information can be used to assess the quality of the hash functions learned in previous steps.  
* **Non-Weighted vs. Weighted Hashing**  
	* **non-weighted:** traditional way.  
    * **weighted:**  
    ![](https://i.imgur.com/IaExo1s.png)  

    
## Hashing Methods
![](https://i.imgur.com/K5mByfn.png)  

## Deep Learning based Hashing Methods
![](https://i.imgur.com/kvbi3Ac.png)  

1. The majority of those methods did not report the time of hash code generation.  
2. Instead of employing deep neural networks to seek hash codes, another interesting problem is to design a proper hashing technique to accelerate deep neural network training or save memory space.  
## Advanced Methods and Related Applications
* Hyperplane Hashing  
* Subspace Hashing  
* MultiModality Hashing  
* Applications with Hashing  
# Discussion
## Open Issues and Future Directions
* Most of the learning based hashing techniques lack the theoretical guarantees on the quality of returned neighbors.  
* Compact hash codes have been mostly studied for large scale retrieval problems.  
* Most of the current hashing technicals are designed for given feature representations that tend to suffer from the semnatic gap.  
* Since heterogeneity has been an important characteristics of the big data applications, one of the future trends will be to design efficient hashing approaches that can leverage heterogeneous features and multi-modal data to improve the overall indexing quality.  
## Question
* Is there any big principle to adjust hashing method according to domain knowledge?   


