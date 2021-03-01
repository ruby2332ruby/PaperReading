# [Paper Reading] Self-training with Noisy Student improves ImageNet classification

due day: 3/4\
paper link: https://arxiv.org/abs/1911.04252\
Xie et al. CVPR 2020

# Poblem Definition
## Question Assumption and Goal(假設與目標)
State-of-the-art (SOTA) vision models are trained with supervised learning which requires a large corpus of labeled images to work well. By showing the models only labeled images, this paper limit itself from **making use of unlabeled images available in much larger quantities to improve accuracy and robustness of SOTA models**.\
**Noisy Student** Training, a **semi-supervised** learning approach that works well even when labeled data is abundant.
## Novelty and Contribution(創新與貢獻)
* The key improvements lie in adding noise to the student and using student models that are not smaller than the teacher. This makes Noisy Student different from Knowledge Distillation.
* Noisy Student Training achieves 88.4% top-1 accuracy on ImageNet, which is 2.0% better than the state-of-the-art model that requires 3.5B weakly labeled Instagram images.
* On robustness test sets, it improves ImageNet-A top-1 accuracy from 61.0% to 83.7%, reduces ImageNet-C mean corruption error from 45.7 to 28.3, and reduces ImageNet-P mean flip rate from 27.8 to 12.2.
![](https://i.imgur.com/DY2tzkB.png)

## Promising Application(保證可以做的未來應用?)
This method not only improves standard ImageNet accuracy, it also improves classification robustness on much harder test sets by large margins.

# Method
## Noisy Student
1. train a teacher model on labeled images
2. use the teacher to generate pseudo labels on unlabeled images
3. train a student model on the combination of labeled images and pseudo labeled images
4. iterate 1.~3. a few times by treating the student as a teacher to relabel the unlabeled data and training a new student
5. to noise the student, use input noise such as RandAugment data augmentation and model noise such as dropout and stochastic depth during training.

![](https://i.imgur.com/jctLQzX.png)

![](https://i.imgur.com/N3tQrJ7.png)

* Noisy Student Training also works better with an additional trick: data filtering and balancing.
## Architecture
Use EfficientNets as baseline.
## Dataset
* **Labeled dataset:** ImageNet 2012 ILSVRC challenge prediction task
* **Unlabeled dataset:** JFT dataset (filter the ImageNet validation set images from the dataset)

# Experiment and Discussion
## Result
### Accuracy
![](https://i.imgur.com/hGWK7Rf.png)
### Robustness
![](https://i.imgur.com/SKh8CSC.png)
### Adversarial Robustness
![](https://i.imgur.com/u8aTaVU.png)


## Discussion
### Comparisons with Existing SSL Methods
In the early phase of semi-supervised training, the model being trained has low accuracy and high entropy, hence consistency training regularizes the model towards high entropy predictions, and prevents it from achieving good accuracy. While Noisy Student has a separate teacher model to generate pseudo-labels.

### Model size study: Noisy Student Training for Efficient-Net B0-B7 without Iterative Training
![](https://i.imgur.com/BrhOEF3.png)

### Additional Ablation Study Summarization
1. Using a large teacher model with better performance leads to better results.
2. A large amount of unlabeled data is necessary for better performance.
3. Soft pseudo labels work better than hard pseudo labels for out-of-domain data in certain cases.
4. A large student model is important to enable the student to learn a more powerful model.
5. Data balancing is useful for small models.
6. Joint training on labeled data and unlabeled data outperforms the pipeline that first pretrains with unlabeled data and then finetunes on labeled data.
7. Using a large ratio between unlabeled batch size and labeled batch size enables models to train longer on unlabeled data to achieve a higher accuracy.
8. Training the student from scratch is sometimes better than initializing the student with the teacher and the student initialized with the teacher still requires a large number of training epochs to perform well.
