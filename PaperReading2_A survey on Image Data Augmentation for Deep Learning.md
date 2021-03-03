# [Paper Reading 2] A survey on Image Data Augmentation for Deep Learning
paper link:  
https://journalofbigdata.springeropen.com/articles/10.1186/s40537-019-0197-0  
Shorten, C., Khoshgoftaar, T.M. J Big Data6, 60 (2019)
# Poblem Definition
## Assumption and Goal(假設與目標)
This survey will present existing methods for Data Augmentation, promising developments, and meta-level decisions for implementing Data Augmentation. Readers will understand how Data Augmentation can improve the performance of their models and expand limited datasets to take advantage of the capabilities of big data.
## Novelty and Contribution(創新與貢獻)
* Summarized Image Data Augmentation for Deep Learning
* Analyzed Design considerations for image Data Augmentation” section.
## Promising Application(未來應用)
Data Augmentation, a data-space solution to the problem of limited data. Data Augmentation encompasses a suite of techniques that enhance the size and quality of training datasets such that better Deep Learning models can be built using them.
## Overfitting
Overfitting refers to the phenomenon when a network learns a function with very high variance such as to perfectly model the training data.  
On the plot: the validation error starts to increase as the training rate continues to decrease.
![](https://i.imgur.com/AiW2ko6.png)

## Generalizability
Generalizability refers to the performance difference of a model when evaluated on previously seen data (training data) versus data it has never seen before (testing data).  
### To Improve Generalizability (To Avoid Overfitting)
* **Data Augmentation:** Data Augmentation is a very powerful method of achieving this. The augmented data will represent a more comprehensive set of possible data points, thus minimizing the distance between the training and validation set, as well as any future testing sets.
* **Change Model’s Architecture:** This has led to a sequence of progressively more complex architectures from AlexNet to VGG-16, ResNet, Inception-V3, and DenseNet.
* **Dropout:** a regularization technique that zeros out the activation values of randomly chosen neurons during training.
* **Batch Normalization:** a regularization technique that normalizes the set of activations in a layer.
* **Transfer Learning:** training a network on a big dataset such as ImageNet and then using those weights as the initial weights in a new classification task.  
#Many aspects of Deep Learning and neural network models draw comparisons with human intelligence. For example, a human intelligence anecdote of transfer learning is illustrated in learning music. If two people are trying to learn how to play the guitar, and one already knows how to play the piano, it seems likely that the piano-player will learn to play the guitar faster. Analogous to learning music, a model that can classify ImageNet images will likely perform better on CIFAR-10 images than a model with random weights. ***(來自管樂的音樂室友表示並沒有這回事。)***
* **Pretraining:** Pretraining is conceptually very similar to transfer learning. Pretraining enables the initialization of weights using big datasets, while still enabling flexibility in network architecture design.
* **One-shot and Zero-shot Learning:** An approach to one-shot learning is the use of siamese networks that learn a distance function such that image classification is possible even if the network has only been trained on one or a few instances. Zero-shot learning uses input and output vector embeddings such as Word2Vec or GloVe to classify images based on descriptive attributes.

# Technical Summary
## Data Augmentation
Data Augmentation artificially inflate the training dataset size by either **data warping** or **oversampling**.  
![](https://i.imgur.com/qn32eqC.png)

### Data Warping
Data warping augmentations transform existing images such that their label is preserved.  

1. **Geometric transformations**
    * **Flipping** 
    * **Color space**
    * **Cropping**
    * **Rotation**
    * **Translation**
    * **Noise injection**
    * **Color space transformations**   
  
2. **Geometric versus photometric transformations**
    * **Kernel filters**
    * **Mixing images**
    * **Random erasing** 

### Oversampling
Oversampling augmentations create synthetic instances and add them to the training set.  

1. **Data Augmentations based on Deep Learning**
    * **Feature space augmentation**
    * **Adversarial training**
    * **GAN‑based Data Augmentation**
    * **Neural Style Transfer**
    * **Meta learning Data Augmentations**
        * Neural augmentation
        * Smart Augmentation
        * AutoAugment

## Dataset
Include MNIST hand written digit recognition, CIFAR-10/100, ImageNet, tiny-imagenet-200, SVHN (street view house numbers), Caltech-101/256, MIT places, MIT-Adobe 5K dataset, Pascal VOC, and Stanford Cars. The datasets most frequently discussed are CIFAR-10, CIFAR-100, and ImageNet.  





# Experiment Results
![](https://i.imgur.com/TUyDlT8.png)
![](https://i.imgur.com/rVdEncs.png)
![](https://i.imgur.com/adzqLDq.png)
![](https://i.imgur.com/Ta1prSB.png)
![](https://i.imgur.com/KCwPPUH.png)
![](https://i.imgur.com/otB9DCe.png)
![](https://i.imgur.com/ohiLz7K.png)
![](https://i.imgur.com/MvvIoTk.png)
![](https://i.imgur.com/w7IcwJ9.png)
![](https://i.imgur.com/HVmn3IP.png)
![](https://i.imgur.com/zy2S9yq.png)


# Discussion
## Design considerations for image Data Augmentation
1. **Test‑time augmentation**
2. **Curriculum learning**
3. **Resolution impact**
4. **Final dataset size**
5. **Alleviating class imbalance with Data Augmentation**
## Question
* How to choose a Data Augmentation based on domain knowledge?