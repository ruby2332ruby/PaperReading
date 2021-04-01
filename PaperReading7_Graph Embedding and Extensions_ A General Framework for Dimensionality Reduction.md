# [Paper Reading 7] Graph Embedding and Extensions: A General Framework for Dimensionality Reduction  
paper link: https://ieeexplore.ieee.org/document/4016549  
Shuicheng Yan et al., PAMI 2007
# Poblem Definition
## [1] Assumption and Goal
In **graph embedding**, each **dimensionality reduction** algorithm can be considered as the direct graph embedding or its **linear/kernel/tensor** extension of a specific intrinsic graph that describes certain desired statistical or geometric properties of a data set, with constraints from scale normalization or a penalty graph that characterizes a statistical or geometric property that should be avoided.  

Furthermore, the graph embedding framework can be used as a general platform for developing new dimensionality reduction algorithms.  

## [2] Novelty and Contribution
* Present in this paper a general formulation known as **graph embedding** to unify them within a common framework.  
* Propose a new supervised dimensionality reduction algorithm called **Marginal Fisher Analysis**.  
* **MFA** effectively overcomes the limitations of the traditional **Linear Discriminant Analysis** algorithm due to data distribution assumptions and available projection directions.  

## [3] Promising Application
Marginal Fisher Analysis in which the **intrinsic graph** characterizes the intraclass compactness and connects each data point with its neighboring points of the same class, while the **penalty graph** connects the marginal points and characterizes the interclass separability.

# Method(Technical Summary)

## [1] Dimensionality Reduction
使用低維的特徵值去代表高為的原始資料內容(高維資料)。目前有supervised與unsupervised learning方式。
* **linear algorithm:** 
    * Principal Component Analysis (PCA)
    * Linear Discriminant Analysis (LDA)
    * Locality Preserving Projections (LPP): preserves local relationships
* **nonlinear algorithm:**
    * ISOMAP
    * LLE
    * Laplacian Eigenmap
    * Kernel trick
    * Objects encoded as matrices or tensors of arbitrary order

![](https://i.imgur.com/nEQflR6.png)  
* the third-order tensor data are the Gabor filtered images  

## [2] Graph Embedding Architecture
* **Classification Problem to Graph:** an undirected weighted graph with vertex set **X** (training dataset) and similarity matrix **W**, where W (symmetric matrix) measures, for a pair of vertices, its similarity, which may be negative.  
* **Intrinsic Graph:** the graph G itself and a penalty graph **Gp:** Wpg as a graph whose vertices **X** are the same as those of G, but whose edge weight matrix **Wp** corresponds to the similarity characteristics that are to be suppressed in the dimension-reduced feature space.  
* **Graph Embedding:** an algorithm to find the desired low-dimensional vector representations relationships among the vertices of G that best characterize the **similarity relationship** between the vertex pairs in G.  
![](https://i.imgur.com/5Qy6FIz.png)
* **Graph-preserving Criterion:** For larger (positive) similarity between samples xi and xj, the distance between yi and yj should be smaller to minimize the objective function. Likewise, smaller (negative) similarity between xi and xj should lead to larger distances between yi and yj for minimization.  
![](https://i.imgur.com/7MttTmX.png)
* **Approach:**
    * **Linearization:** do linear projection: ![](https://i.imgur.com/xD4GQey.png)  
*Graph-preserving Criterion:*  
![](https://i.imgur.com/myta7wq.png)  
    * **Kernelization:** to map the data from the original input space to another higher dimensional Hilbert space and then perform the linear algorithm in this new feature space.  
    *Graph-preserving Criterion:*  
    ![](https://i.imgur.com/mwvjsNk.png)  
    * **Tensorization:** to conduct dimensionality reduction with vertices encoded as general tensors of an arbitrary order.  
    *Graph-preserving Criterion:*  
    ![](https://i.imgur.com/QkSgkAY.png)  

## [3] General Framework for Dimensionality Reduction
The previously mentioned dimensionality reduction algorithms can be reformulated within the presented graph embedding framework.  
The differences between these algorithms lie in the computation of the similarity matrix of the graph and the selection of the constraint matrix.  
![](https://i.imgur.com/cdjYUFK.png)
* The top row is the graph embedding type, the middle row is the corresponding objective function, and the third row lists the sample algorithms.  

![](https://i.imgur.com/6oXQ4Qh.png)

## [4] Marginal Fisher Analysis
* **Limitation of LDA:** LDA is developed with the assumption that the data of each class is of a **Gaussian distribution**, a property that often does not exist in real-world problems. Without this property, separability of the different classes cannot be well characterized by interclass scatter.  
* **Marginal Fisher Analysis(MFA):** an **intrinsic graph** that characterizes the intraclass compactness and another **penalty graph** which characterizes the interclass separability.  
![](https://i.imgur.com/CHp980m.png)  
* **The algorithmic procedure of MFA:**
    1. PCA projection.
    2. Constructing the intraclass compactness and interclass separability graphs.
    3. Marginal Fisher Criterion.![](https://i.imgur.com/jV40BnE.png)
    4. Output the final linear projection direction![](https://i.imgur.com/NlHzgX3.png)
* **The Advantages of MFA:**
    1. The available projection directions are much greater than that of LDA and the dimension size is determined by k2, the selected number of shortest pairs of in-class and out-of-class sample pairs.
    2. There is no assumption on the data distribution of each class and the intraclass compactness is characterized by the sum of the distances between each data and its k1-nearest neighbors of the same class. Thus, it is more general for discriminant analysis.
    3. Without prior information on data distributions, the interclass margin can better characterize the separability of different classes than the interclass variance in LDA.

* **Kernel Marginal Fisher Analysis:** The kernel trick is widely used to enhance the separation ability. Marginal Fisher Analysis can be further improved by using the kernel trick.  
projection direction be: ![](https://i.imgur.com/pVldCxD.png)  
optimal: ![](https://i.imgur.com/Gx49rF5.png)

* **Tensor Marginal Fisher Analysis:** with the objects are represented as tensors of arbitrary order.  
![](https://i.imgur.com/SfGWM7j.png)


## [3] Dataset

# Experiment Results

# Discussion

## [1] Conclusion

## [2] Future Works
* Systematically compare all possible extensions of thealgorithms mentioned in this paper.  
* The combination of the kernel trick and tensorization.  
* An open problem in MFA is the selection of parameters k1 and k2, which is also an unsolved problem in algorithms such as ISOMAP, LLE, Laplacian Eigenmap, and LPP.  
* There are also cases under whichMFAmay fail. For example, when the value of k2 is insufficiently large, a nonmarginal pair from different classes in the original feature space may be very close in the dimension-reduced feature space and degrade classification accuracy.  
* This framework only considers the L2 distance as a similarity measure, which means that it can only take into account the second-order statistics of the data set. How to utilize higher order statistics of the data set in the graph embedding framework.  

## [3] Question
* 
