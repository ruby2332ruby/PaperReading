# [Paper Reading 14] Fast and Furious: Real Time End-to-End 3D Detection, Tracking and Motion Forecasting with a Single Convolutional Net  
paper link: https://openaccess.thecvf.com/content_cvpr_2018/html/Luo_Fast_and_Furious_CVPR_2018_paper.html  
Luo et al. CVPR 2018.
# Poblem Definition
## [1] Assumption and Goal
Modern approaches to self-driving divide the problem
into four steps: detection, object tracking, motion forecasting and motion planning.
* **Detection:** Object detection is a computer technology related to computer vision and image processing that deals with detecting instances of semantic objects of a certain class (such as humans, buildings, or cars) in digital images and videos. (or any data get from sensor) (from wiki)  
* **Object Tracking:** Object tracking is the task of taking an initial set of object detections, creating a unique ID for each of the initial detections, and then tracking each of the objects as they move around frames in a video, maintaining the ID assignment. (from https://paperswithcode.com/task/object-tracking)  
* **Motion Forecasting:** Estimates where traffic participants are going to move in the next few seconds. (from paper)  
* **Motion Planning:** Estimates the final trajectory of the ego-car. (from paper)  

These modules are usually learned independently, and then cascade together. Uncertainty is usually rarely propagated. This can result in catastrophic failures as downstream processes cannot recover from errors that appear at the beginning of the pipeline.  

We propose an end-to-end fully convolutional approach that performs simultaneous 3D detection, tracking and motion forecasting by exploiting spatio-temporal information captured by a 3D sensor.  


## [2] Novelty and Contribution
* Real time: by sharing computation it can perform all tasks in as little as 30 ms.  
* Jointly reason about 3D detection, tracking and motion forecasting given data captured by a 3D sensor. By jointly reasoning about these tasks, our holistic approach is more robust to occlusion as well as sparse data at range.  

## [3] Promising Application
* Apply on self-driving car.  

# Method(Technical Summary)

## [1] 2D Object Detection
CNN 2D object detections from a single image.  
* **Two-stage Detectors:** Utilize region proposal networks (RPN) to learn the region of interest (RoI) where potential objects are located. In a second stage the final bounding box locations are predicted from features that are average-pooled over the proposal RoI.  
    * Mask-RCNN
* **One-stage Detectors:** Skip the proposal generation step, and instead learn a network that directly produces object bounding boxes. One-stage detectors are computationally very appealing and are typically real-time.  
    * YOLO
    * SSD
    * RetinaNet
    * MobineNet
    * SqueezeNet

## [2] 3D Object Detection
These approach are very slow on per frame computation.  
* Used stereo images.  
* Used 3D point cloud data and proposed to use 3D convolutions on a voxelized representation of point clouds.  
* Fusion network: image + 3D point clouds  

## [3] Object Tracking
* Pretrained CNNs were used to extract features and perform tracking with correlation or regression.  
* Used autoencoder instead of CNN.  
* Used siamese matching networks.  
* Finetuned a CNN at inference time to track object within the same video.  

## [4] Motion Forecasting
Predicting where each object will be in the future given multiple past frames.  
* RNN, LSTM  
* game theory  

Some work has also focussed on short term prediction of dynamic objects.  
* VAE  

## [5] Multi-task Approaches
Existed multi-task:
* **Detection + Tracking:** Model the displacement of corresponding objects between two input images during training and decode them into object tubes during inference time.  

## [6] Architecture
Our input representation is a 4D tensor encoding an
occupancy grid of the 3D space over several time frames.  
We name our approach **Fast and Furious (FaF)**, as
it is able to create very accurate estimates in as little as 30 ms.  

* **Data Parameterization**  
    * **Voxel Representation:** (CNN) We quantize the 3D world to form a 3D voxel grid. We then assign a binary indicator for each voxel encoding whether the voxel is occupied. Since the grid is very sparse, we performed 2D convolutions and treat the **height dimension** as the channel dimension.  
    ![](https://i.imgur.com/fhYEJSk.png)  
    * **Adding Temporal Information:** we take all the 3D points from the past n frames and perform a change of coordinates to represent then in the **current vehicle coordinate system**. To undo the ego-motion of the vehicle where the sensor is mounted. After performing this transformation, we compute the voxel representation for each frame. (4D tensor)  
    ![](https://i.imgur.com/cOOfTrq.png)

* **Model Formulation:** We investigate two different ways to exploit the temporal dimension on our 4D tensor: **early fusion** and **late fusion**. They represent a tradeoff between accuracy and efficiency, and they differ on at which level the temporal dimension is aggregated.  
![](https://i.imgur.com/IsIuLkh.png)  
    * **Early Fusion:** Aggregates temporal information at the very first layer. It runs as
fast as using the single frame detector. However, it might lack the ability to capture complex temporal features as this is equivalent to producing a single point cloud from all frames, but weighting the contribution of the different timestamps differently.  
    * **Late Fusion:** we gradually merge the temporal information. This allows the model to capture high level motion features.  

We then add two branches of convolution layers. The first one performs binary classification to predict the probability of being a vehicle. The second one
predicts the bounding box over the current frame as well as n−1 frames into the future.  
![](https://i.imgur.com/tNCVvm2.png)  

Following SSD , we use multiple predefined boxes
for each feature map location. As we utilize a BEV (bird’s eye view) representation which is metric, our network can exploit priors about physical sizes of objects.  

* **Decoding Tracklets:** Each timestamp will have current detections as well as n − 1 past predictions. Thus we can aggregate the information for the past to produce accurate tracklets without solving any trajectory based optimization problem. We use average as aggregation function.  

## [7] Loss Function and Training
* **Regression:** we include both the current frame as well as our n frames forecasting into the future.  
![](https://i.imgur.com/bGLNqwp.png)  
* **Classification:** binary cross-entropy computed over all locations and predefined boxes.  
![](https://i.imgur.com/cDV5LGz.png)  
* **Ground Truth:** we first find the ground truth box with biggest overlap in terms of intersection over union (IoU). If the IoU is bigger than a fixed threshold, we assign this ground truth box to this boundung box and its corresponding label. If there exist a ground truth box not assigned to any predefined box, we will assign it to its highest overlapping predefined box ignoring the fixed threshold.  
* **Hard Data Mining:** Due to the imbalance of positive and negative samples, we use hard negative mining during training.  

## [8] Dataset
We collected a very large scale dataset in order to
benchmark our approach. It is 2 orders of magnitude bigger than datasets such as KITTI.  
Our dataset is collected by a roof-mounted LiDAR on top of a vehicle driving around several North-American cities.  

# Experiment Results
![](https://i.imgur.com/PQPqRru.png)  
* Our method is able to outperform all other methods. Particularly at IoU 0.7, we achieve 4.7% higher mAP than MobileNet while being twice faster, and 5.2% better than SSD with similar running time.  

![](https://i.imgur.com/nbo9cOv.png)  
* Ablation Study, We fixed the training setup for all experiments.   

![](https://i.imgur.com/ttUHyon.png)  
* Our model is able to output detections with track ids directly. We can see that our final output achieves 80.9% in MOTA, 7.8% better than Hungarian, as well as 20% better on MT, 10% lower on ML, while still having similar MOTP.  

![](https://i.imgur.com/ZpfX1eW.png)  
* We are able to predict 10 frames into the future with L2 distance only less than 0.33 meter. Note that due to the nature of the problem, we can only evaluate on true positives, which in our case has a corresponding recall of 92.5%.  

* **Qualitative Results:**  

![](https://i.imgur.com/Jy8ZGnm.png)  
![](https://i.imgur.com/aXqFl1z.png)  




# Discussion

## [1] Conclusion
Jointly detection, prediction and tracking in the scenario of autonomous driving. It runs real-time and achieves very good accuracy on all tasks.  
In the future, we plan to incorporate RoI align or plan to test other categories such as pedestrians and produce longer term predictions.

## [2] Question
* If using data from multiple cars, how to consider data transmission delay? Or how to select data when the number of cars arround is too large?  
