---
layout: post
title:  "Yolo性能展示 YOLO Explore Object Detection PERFORMANCES In Relation To INPUT IMAGE DETERIORATION
"
date:   2021-04-15 22:34:18
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---


**Experiment**

**Baseline Model and Tuning**

We consider having the model trained based on the original dataset without processing it with distorted quality and treat it as the baseline model. A fine-tune process is also necessary to conduct in order to get the optimal performance for object detection. This allows us to fit the same dataset processed in distorted methods in further step and make it possible for us to compare how the dataset in the variable of distortion types would impact the performance of the baseline model.

**Learning Rate Annealing**

During the training process, the adjustment of the learning rate is best have. The learning rate represents the speed at which information accumulates in the neural network over time. In an ideal situation, we would start with a large learning rate and gradually reduce the speed until the loss value no longer diverges.
The technique we use is learning rate annealing, which starts with a relatively high learning rate and then slowly reduces the learning rate during training. The idea behind this method is to move from the initial parameter to a "good" range of parameter values. We use the **ReduceLROnPlateau** callback method of the TensorFlow API to implement this step. This callback monitors a quantity and if no improvement is seen for a 'patience' number of epochs, the learning rate is reduced.

**Early Stopping**

In order to get better generalization performance to prevent the model from overfitting, we use the early stopping method. It is implemented by the TensorFlow EarlyStopping callback function. It stops training when a monitored metric has stopped improving based on the ‘patience’ - number of epochs with no improvement after which training will be stopped. We set it 3 for giving the 3 epochs training threshold times.

**NMS Parameters Tuning: Score and IoU**

Once the YOLO model has finished the training process, it is necessary to fine-tune the IoU threshold and Score threshold to improve the overall performance in mAP metric. We choose the strategy by individually incrementing the values of each parameter in the stride of 0.1 and evaluate its model performance. During the individual tuning of each parameter, we select the current parameter value that can make mAP reach the highest value as the baseline setting. Therefore, for the final tuning result, we expect to get IoU and Score setting as in the format of values of pairs.

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/01-finetune-score.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/01-finetune-score.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Score Threshold Tuning Results</figcaption>
</div>

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/02-finetune-score.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/02-finetune-score.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Score Threshold Tuning Line Graph</figcaption>
</div>

It can be seen from Figure 12 that the overall line graph follows a monotonous downward trend which also applies to both categories of the Traffic sign and Stop sign. 

The Step sign category has the lowest degree of decline in AP value, and the decline rate is slower, so the curve looks smoother. From table 1, the results show Stop sign performs consistently steady until the score threshold reaches the values that close to the end.

Traffic sign behaves in an opposite way, the AP value is decreasing from the beginning, as the score threshold increases linearly, the AP value decreases more rapidly.

When the score value is 0.1, the mAP value of the YOLO model is the highest and performs best. Therefore 0.1 is selected for the score threshold value for the baseline YOLO model.

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/03-finetune-iou.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/03-finetune-iou.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>IoU Threshold Tuning Results</figcaption>
</div>

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/04-finetune-iou.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/04-finetune-iou.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>IoU Threshold Tuning</figcaption>
</div>

It can be seen from Figure 13 all three lines display a two-phased behavior. In the first phase, they both demonstrate a smooth upwards trend in values. The increasing rate is too small to be noticed in the figure, but the performance improves from the results table.  

In the second phase, the overall mAP and AP of all categories drop rapidly. The starting point for having such a situation is when IoU threshold equals around 0.6. 

When the IoU value is 0.3, the mAP value of the YOLO model is the highest and performs best. Therefore, the IoU threshold setting for the baseline YOLO model is 0.3.

**Fitting Dataset in Distortion**

As it is mentioned in part 2, the dataset that employed for the experiment contains 3200 images, in which 10 percent of them are 320 images that are selected as the test set. The images in the test set will first be processed by different distortion methods, and then the test images of different types and levels of distortion will be input into the neural network for prediction, and the results will be used to measure the performance of the object detector using the final statistics of mAP.

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/05-distortion-types.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/05-distortion-types.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Distortion Image Comparison Results</figcaption>
</div>

Noted that all 320 test images serve only for testing the performance of the YOLO model. The model is trained and validated by the other 90 percent of the dataset.

**Results**

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/06-map_results.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/06-map_results.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Distortion Image Comparison Results</figcaption>
</div>

<div style="text-align: center;">
    <a href ="{{site.url}}/assets/2021-04-15-YOLO-Experiment/07-map_comparison.png">
   <img src="{{site.url}}/assets/2021-04-15-YOLO-Experiment/07-map_comparison.png" alt="drawing" style="width: 100%;"/>
    </a>
   <figcaption>Distortion Image Comparison Bar Graph</figcaption>
</div>

From the experiment results, it is interesting to observe that the object detector is remarkably affected by the images of different distorted types in various behaviors.

There are 3 distortion types that affect the detector in a positive way by increasing the mAP values. These three distortion types are Noise100, Grayscale, and Contrast. However, the increase of values is not significant therefore the impact levels are considered to be limited and weakly.

The other three distortion types of images – Noise200, 2percentPackLost, 5percentPackLost – suffer extreme decrease. The 2percentPackLost and 5percentPackLost are even close to half and one-third of the performance with original images. 

Horizontally comparing detector’s performances with Stop signs(SS) and Traffic signs(TS), one using SS for all distortion types and levels beats what use TS. Surprisingly to observe that the 4 distortion types of images in Noise100, Noise200, Grayscale, and Contrast boosts the performances using SS. In other words, these 4 distortion types improved the detector’s ability to predict. However, it works in the opposite way when employs TS and exhibits a similar decrease in performance of overall mAP.


---
Copyright @ 2021 Zizhun Guo. All Rights Reserved.

