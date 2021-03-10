# [Paper Reading 3] Deep Learning for Understanding Faces: Machines May Be Just as Good, or Better, than Humans
paper link: https://ieeexplore.ieee.org/document/8253595  
IEEE Signal Processing Magazine 2018

# Poblem Definition
## Assumption and Goal
Provide an overview of deep-learning methods used
for face recognition. Discuss different modules involved in designing an automatic face recognition system and the role of deep learning for each of them. Some open issues regarding DCNNs for face recognition problems are then discussed.  

## Novelty and Contribution
* 同上

## Promising Application
This article should prove valuable to scientists, engineers, and end users working in the fields of face recognition, security, visual surveillance, and biometrics.  

# Method/Technical Summary

## Face detection in unconstrained images
Given an input image, a face detector needs to detect all of the faces in the image and return bounding-box coordinates for each of them.  
* **Traditional:** Haar wavelets and histogram of oriented gradient (HOG) do not capture salient facial information at different resolution, viewpoint, illumination, expression, skin color, occlusions, and cosmetics conditions.
* **DCNN-based:** 
    * **Region based:** The region-based approach generates a pool of generic objectproposals (approximately 2,000 per image), and a DCNN is used to classify whether or not a given proposal contains a face.
    * **Faster R-CNN:** A more recent face detector using faster R-CNN uses a common DCNN to generate the proposals and classify faces. It can also simultaneously regress the bounding-box coordinates for each face proposal.
    * **Sliding-window based:** The sliding-window-based method computes a face detection score and bounding-box coordinates at every location in a feature map at a given scale. Detections at different scales are typically carried out by creating an image pyramid at multiple scales.  
![](https://i.imgur.com/702BMRZ.png)  
    * **Single-shot detector:** The SSD is a sliding-window-based detector, but instead of creating image pyramids at different scales, it utilizes the inbuilt pyramid structure present in
DCNNs. The overall computation time of SSD is lower than faster R-CNN.  

* **Face detection data sets**
    * **FDDB:** FDDB is the most widely used benchmark for unconstrained face detection. It consists of 2,845 images containing a total of 5,171 faces collected from news articles on yahoo.com.
    * **MALF:** MALF consists of 5,250 high-resolution images containing a total of 11,931 faces.
    * **WIDER Face:** WIDER Face contains a total of 32,203 images, with 50% samples for training and 10% for validation. This data set contains faces with large variations in pose, illumination, occlusion, and scale.  

## Finding crucial facial keypoints and head orientation
Facial keypoints such as eye centers, nose tip, mouth corners, etc. can be used to align the face into canonical coordinates, and such face normalization helps face recognition and attribute
detection.  
Head-pose estimation is required for pose-based face analysis.  
![](https://i.imgur.com/DG0qy8i.png)  
* **Model based:** Model-based approaches, such as AAM, ASM, and CLM, learn a shape model during training and use it to fit new faces aduring testing. However, the learned models lack the power to capture complex face image variations in pose, expression, and illumination. Also, they are sensitive to the initialization in gradient descent optimization.  
* **Cascaded regression based:** In general, these methods learn a model that directly maps the image appearance to the target output. Nevertheless, the performance of these methods depends on the robustness of local descriptors.

## Face identification and verification
There are two major components for face identification/verification:  
1. robust face representation
2. a discriminative classification model (face identification) or similarity measure (face verification)  
* **Face identification data set**  
![](https://i.imgur.com/HsUkZPx.png)

## Facial attributes
Facial attributes: gender, expression, age, skin tone, etc.  
In biometrics literature, facial attributes are also referred to as soft biometrics

## MTL(multitask learning) for facial analysis

# Discussion
## Open issues
* Face detection chellange by variations, ex. illumination, facial expression,viewpoints, occlusions, blur, low resolution, etc.
* Fiducial detection data sets most contain only a few thousand images.
* Memory constraints limited by graphics cards, while training face identification/verification.
## Question
* How to deal with privacy issue in face task in the future?

