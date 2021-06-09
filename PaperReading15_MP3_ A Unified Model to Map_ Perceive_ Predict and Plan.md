# [Paper Reading 15] MP3: A Unified Model to Map, Perceive, Predict and Plan  
paper link: https://arxiv.org/abs/2101.06806  
Ren et al. CVPR 2021.
# Poblem Definition
## [1] Assumption and Goal
**High-definition (HD) Map:** maps that contain rich semantic information necessary for driving such as the topology and location of the lanes, crosswalks, traffic lights, intersections as well as the traffic rules for each lane (e.g., unprotected left, right turn on red, maximum speed).   
**MP3**, an end-to-end approach to mapless driving where the input is raw sensor data and a high-level command (e.g., turn left at the intersection). MP3 predicts intermediate representations in the form of an online map and the current and future state of dynamic agents, and exploits them in a novel neural motion planner to make interpretable decisions taking into account uncertainty.  

## [2] Novelty and Contribution
* our approach is significantly safer, more comfortable, and can follow commands better than the baselines in challenging long-term closed-loop simulations.  
* Perform better when compared to an expert driver in a large-scale real-world dataset.  

## [3] Promising Application
* self-driving car

# Method(Technical Summary)

## [1] Driving with HD Maps
HD maps contain rich semantic information necessary for driving. These maps are a great source of knowledge that simplify the perception and motion forecasting tasks, as the online inference process has to mainly focus
on dynamic objects. And significantly increases the safety of motion planning as knowing the lane topology and geometry.
However, building HD maps is hard to scale due to the complexity and cost of generating the maps and maintaining them. Furthermore, it heavily relys on centimeter-level accuracy localization system.  

![](https://i.imgur.com/sI9VdVq.png)  

## [2] Driving without HD Maps
Mapless technology can tolerace localization failures or outdated maps. However, the search space to plan a safe maneuver for the SDV goes from narrow envelopes around the lane center
lines to the full set of dynamically feasible
trajectories, and the goal that the SDV is trying to reach needs to be abstracted into
high-level behaviors such as going straight at an intersection, turning left or turning right.  

## [3] Online Mapping
* **offline mapping approaches:** these rely on satellite imagery or multiple passes through the same scene with a data collection vehicle to gather dense information, and often involve a human-in-the-loop.  
* **online mapping approaches:** predicting map elements online.  
    * directly predict from a single image  
    * use LiDAR and camera to predict estimates of ground height and lanes  
    * hierarchical recurrent neural network  

## [4] Perception and Prediction
Most previous works perform object detection and actor-based prediction to reason about the current and future state of a driving scene.  
* generate a fixed set of trajectories  
* draw samples to characterize the distribution  
* predict temporal occupancy maps  
* estimate occupancy probability of each grid-cell independently using range sensor data  
* directly predicts an occupancy grid to replace object detection  

## [5] Motion Planning
* Pioneering: use a single neural network that directly outputs a driving control command.  
*  Use DNN to directly generating control command from sensor data.  
*  cost map-based approaches  
    *  simple linear combination of hand crafted costs  
    *  a general non-parametric form  
* output a navigation cost map without localization under a weakly supervised learning environment  
* improves sampling in complex driving environments without the consideration of dynamic objects  

## [6] Interpretable Mapless Driving
Interpretable representations estimate the current and future state of the world around the SDV, including the unknown map as well as the current and future location and velocity of dynamic objects.  

* **Extracting Geometric and Semantic Features (Backbone Network):** Our model exploits a history of LiDAR point clouds to extract rich geometric and semantic features from the scene over time.  
![](https://i.imgur.com/LZ1b5Ly.png)  

* **Interpretable Scene Representations:** to predict intelligible representations of the static environment, which we refer here as an **online map**, as well as the dynamic objects position and velocity into the future, captured in our **dynamic occupancy field**.  
    * **Online map representation:** In order to drive safely it is useful to reason the following elements in BEV:  
        * Drivable area: Road surface (or pavement) where vehicles are allowed to drive, bounded by the curb.  
        * Reachable lanes: Lane center lines (or motion paths) are defined as the canonical paths vehicles travel on, typically in the middle of 2 lane markers.  
        * Intersection: Drivable area portion where traffic is controlled via traffic lights or traffic signs.  
        ![](https://i.imgur.com/JP2sl0X.png)  
    * **Dynamic occupancy field:** to understand which space is occupied by dynamic objects and how do these move over time. we propose an occupancy flow parameterized by the occupancy of the dynamic objects at the current state of the world and a temporal motion field into the future that describes how objects move (and in turn their future occupancies).  
        * Initial Occupancy: a BEV grid cell is active (occupied) if its center falls in the interior of a polygon given by an object shape and its current pose.  
        * Temporal Motion Field: defined for the occupied pixels at a particular time into the future. Each occupied pixel motion is represented with a 2D BEV velocity vector (in m/s).  
        ![](https://i.imgur.com/KuB4VhC.png)  

    * **Probabilistic Model:** To reason about uncertainty in our online map and dynamic occupancy field, we model each semantic channel of the online mapMas a collection of independent variables per BEV grid cell.  
        * we first define the probability of occupancy flowing from location i1 to location i2 between two consecutive time steps t and t + 1 as follows:  
        ![](https://i.imgur.com/HUg1lST.png)  
        * then simply compute the probability that no occupancy flow event occurs, and take its complement  
        ![](https://i.imgur.com/Y95Hyta.png)  

## [7] Motion Planning
Motion planner is to generate trajectories that are safe, comfortable and progressing towards the goal. We design a sample-based motion-planner in which a set of kinematically-feasible trajectories are generated and then evaluated using a learned scoring function.  
* minimum cost:   
![](https://i.imgur.com/9OCc0tD.png)
* **Trajectory Sampling:** 
* **Route Prediction:** 

## [1] Architecture

## [2] Dataset

# Experiment Results

# Discussion

## [1] Question
* 
