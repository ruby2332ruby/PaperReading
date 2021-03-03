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
Geometric transformations are very good solutions for positional biases present in the training data.
    * **Flipping:** Horizontal axis flipping is much more common than flipping the vertical axis.On datasets involving text recognition such as MNIST or SVHN, this is not a label-preserving transformation.
    * **Color space:** Digital image data is usually encoded as a tensor of the dimension (height × width × color channels). Performing augmentations in the color channels space is another strategy that is very practical to implement.
    * **Cropping:** Cropping a central patch of each image. Additionally, random cropping can also be used to provide an effect very similar to translations.
    * **Rotation:** rotating the image right or left on an axis between 1° and 359°. The safety of rotation augmentations is heavily determined by the rotation degree parameter.
    * **Translation:** Shifting images left, right, up, or down can be a very useful transformation to avoid positional bias in the data.
    * **Noise injection:** Noise injection consists of injecting a matrix of random values usually drawn from a Gaussian distribution.
2. **Color space transformations** Image data is encoded into 3 stacked matrices, each of size height × width. These matrices represent pixel values for an individual RGB color value. Lighting biases are amongst the most frequently occurring challenges to image recognition problems. Color space transformations can also be derived from image-editing apps.  
Similar to geometric transformations, a disadvantage of color space transformations
is increased memory, transformation costs, and training time. Additionally,
color transformations may discard important color information and thus are not
always a label-preserving transformation.
  

3. **Kernel filters** These Kernel filters work by sliding an n × n matrix across an image with either a Gaussian blur filter, which will result in a blurrier  image, or a high contrast vertical or horizontal edge filter which will result in a sharper image along edges.
4. **Mixing images** Mixing images together by averaging their pixel values is a very counterintuitive approach to Data Augmentation.
5. **Random erasing** the mechanisms of dropout regularization, random erasing can be seen as analogous to dropout except in the input data space rather than embedded into the network architecture. 

### Oversampling
Oversampling augmentations create synthetic instances and add them to the training set.  

1. **Data Augmentations based on Deep Learning**
    * **Feature space augmentation:** These networks can map images to binary classes or to n × 1 vectors in flattened layers. The sequential processing of neural networks can be manipulated such that the intermediate representations can be separated from the network as a whole.
        * **SMOTE** used to alleviate problems with class imbalance.This technique is applied to the feature space by joining the k nearest neighbors to form new instances.
    * **Adversarial training:** Adversarial training is a framework for using two or more networks with contrasting objectives encoded in their loss functions.
        * **Adversarial attacking** consists of a rival network that learns augmentations to images that result in misclassifications in its rival classification network.
    * **GAN‑based Data Augmentation:** Generative modeling refers to the practice of creating artificial instances from a dataset such that they retain similar characteristics to the original set.
        * **Variational auto-encoders** learn a lowdimensional representation of data points. It is another useful strategy for generative modeling worth mentioning.
        * **Vanilla GAN** uses multilayer perceptron networks in the generator and discriminator networks.
        * **DCGAN** was proposed to expand on the internal complexity of the generator and discriminator networks.
        * **Progressively Growing GANs** trains a series of networks with progressive resolution complexity.
        * **CycleGAN** introduces an additional Cycle-Consistency loss function to help stabilize GAN training. This is applied to image-to-image translation.
        * **Conditional GANs** add a conditional vector to both the generator and the discriminator in order to alleviate problems with mode collapse.
    * **Neural Style Transfer:** The general idea is to manipulate the representations of images created in CNNs. Neural Style Transfer is probably best known for its artistic applications, but it also serves as a great tool for Data Augmentation.
2. **Meta learning Data Augmentations:** The concept of optimizing neural networks with neural networks.  
    * **Neural augmentation** takes in two random images from the same class. The prepended augmentation net maps them into a new image. The image outputted from the augmentation is then transformed with another random image via Neural Style Transfer.These images are then fed into a classification model and the error from the classification model is backpropagated to update the Neural Augmentation net.
    * **Smart Augmentation** utilizes a similar concept as the Neural Augmentation technique presented above. However, the combination of images is derived exclusively from the learned parameters of a prepended CNN, rather than using the Neural Style Transfer algorithm.
    * **AutoAugment** is a Reinforcement Learning algorithm that searches for an optimal augmentation policy amongst a constrained set of geometric transformations with miscellaneous levels of distortions.

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