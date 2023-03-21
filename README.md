<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/banner.png" width="100%">
</p>

<p align="center">
<a href="https://colab.research.google.com/drive/1mhovAKi8Qs7FTWp-0efa1kfHyOFvR2c1?usp=sharing"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://github.com/ultralytics/ultralytics"><img src="https://img.shields.io/badge/Ultralytics-Open-brightgreen" alt="Ultralytics"></a> <a href="https://github.com/xN1ckuz/Crosswalks-Detection-using-YOLO/blob/main/LICENSE.md"><img src="https://img.shields.io/badge/license-GPL-blue" alt="License"></a>
</p>

<p align="center"> 
We introduce a supervised method for detecting pedestrian crosswalks with DCNNs and Python. The project is based on YOLO framework, developed by <a href="https://github.com/ultralytics/ultralytics">Ultralytics</a>. The code is written on Google Colab, a platform to execute code on the Cloud, in the form of Jupyter Notebook. It provides also a GPU, needed for the project. The v1.2.0 of the detector uses YOLOv5, then we updated it to state-of-art model YOLOv8 in the version v2.0.0.
</p>

<h2 align="center"> Project Purpose</h2>
<p align="justify"> 
We have created a detector for pedestrian crossings, as more and more often we hear about road accidents with the involvement of pedestrians. Every 32 hours, a pedestrian is run over and killed, a staggering number. This explains the reason for our research. In addition, this detector can also be used by blind people, ensuring greater safety on the road.
</p>

<h2 align="center"> Creating the dataset </h2>
<p align="justify"> 
In this phase we took the photos, at different times of the day (morning, afternoon, evening), in different light conditions, with different cameras, in different weather conditions and different perspectives. To label the images we used “LabelImg”, completely free tool. LabelImg generates a YOLO .txt file for each image. It has coordinates in YOLO format object-id, center_x, center_y, width, height.
</p>

<ul>  
<li>object-id represents the number corresponding to the category of objects listed in the 'classes.txt' file</li>  
<li>center_x and center_y represent the central point of the selection rectangle</li>  
<li>width and height represent the width and height of the rectangle.</li>  
</ul>

<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/label.jpg" width="720">
</p>
<p align="justify">
We labeled two pedestrian crossings at a time, to avoid selecting too large a portion of the image.
</p> 

<h2 align="center">  YOLO Model </h2>
<p align="justify"> 
We took the pre-trained model YOLOv8m, then re-trained it with 300 epochs; we could not choose a better model due to the GPU RAM limitations of Google Colaboratory. YOLOv8 is the version of YOLO that was chosen, not only for its effectiveness but above all for its speed, as well as for the possibility of choosing the pre-trained weight to use.
</p> 
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/yolo_model.png" width="720">
</p>

<h2 align="center"> Bounding Box Merge </h2>
<p align="justify"> 
A major challenge was to merge the various bounding boxes. For our problem, in the state of art, we could not find any algorithm already tested and working. For this reason, we have personally implemented an algorithm for bounding boxes merge in Python, using OpenCV.
</p> 
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/merge.png" width="720">
</p>
<p align="justify"> 
We know that OpenCV, in order to construct a rectangle, uses the coordinates of the highest left vertex and the lowest right vertex expressed in pixels, while the YOLO labels provide us with the coordinates of the center, width and height of the single bounding box. In our algorithm, after transforming the coordinates from YOLO to OpenCV, we took the minimum x and y coordinates for the top left vertex, while the maximum x and y coordinates for the lower right vertex.
</p> 

<h2 align="center"> Dataset </h2>
<p align="justify"> 
The photos were taken personally, then the dataset was uploaded into <a href="https://roboflow.com/">Roboflow</a> to create the dataset. At this point, we carried out the augmentation step, as we can see the image below.
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/augmentation.jpg" width="480">
</p>
<p align="justify"> 
As results we have 1046 images for training, 224 for validation, and 224 to calculate the test metrics. We reserved 75% of the images for training, 15% for validation and 15% for testing. We took an additional 202 photos which we used to calculate the inferences.
</p> 

<h2 align="center"> Qualitative Results </h2>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/inference.jpg" width="720">
</p>
<p align="justify">
The results are in line with what was expected.
</p>

<h2 align="center"> Quantitative Results </h2>
<p align="justify"> 
The final model we have taken into consideration is what is called <i>best.pt</i>, and is chosen by YOLO through these metrics, giving a greater weight to the mAP 0.5: 0.95, a lower weight to the mAP 0.5 and a zero weight to recall and precision. The data we get from the training are also automatically graphed as follows.
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/results.png" width="720">
</p>

<p> The results on the test data are: </p>
<ul> <li>Precision of 0.96</li> <li>Recall value of 0.93</li> </ul>

<p> F1 Score: </p>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/F1.png" width="640">
</p>

<h2 align="center">  Intersection over Union</h2>
<p align="justify"> We wrote oursleves the algorithm to test the IoU metric, obtaining a result equal to 0.63. </p>

<h2 align="center"> Algorithm Refinement </h2>
<p align="justify">
Now we describe the extra work we did on our project. The first training on v1.0.0 was almost perfect, but we wanted more. The first run we did was on 250 epochs on yolov5m pre-trained model. We wanted to use yolov5l, but we were limited on GPU RAM by Colab (for better GPU Colab Pro is needed).
When we did the first detection, we noticed some problems. First of all, some stop road markings were identified as pedestrian crossings; secondly, not all pedestrian crossings were fully identified.
For these reasons, we modified training dataset and parameters. In the version v1.2.0 we used yolov5m as the first run, but this time with 300 epochs. In the training dataset, we did a better augmentation (parameters are explained in dataset paragraph), and then we added some examples of stop road markings, with empty label; in this way, CNN has learned to recognize stops correctly.
In the v2.0.0 we used the same dataset and parameters just described , this time with updated yolov8m weight.
In the image below, we can see on the left the detection obtained with the first model. On the right, the detection disappeared, as expected.
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/stop.jpg" width="720">
</p>
<p align="justify">
In this image, instead, we can see that the second model performs better, as it detected a larger portion of the crosswalk.
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/crosswalk.jpg" width="720">
</p>
<h2 align="center"> Merge Results </h2>
<p align="justify">
Last thing we did was to merge YOLO bounding boxes. This was necessary as we have labelled two crosswalks at a time; in this way the model performs better, but in the detection phase crosswalks are identified several times. As you can see from the figure below, we have achieved excellent results and we have also managed to save the maximum confidence.
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/xN1ckuz/Crosswalks-Detection-using-YOLO/main/resources/merge_result.jpg" width="720">
</p>

<h2 align="center"> License </h2>
<p align="justify"> 
This work is under <b>GPL-3.0 License</b>, check <a href="https://github.com/xN1ckuz/Crosswalks-Detection-using-YOLO/blob/main/LICENSE.md">LICENSE</a> file for details. All rights reserved to <a href="https://github.com/ultralytics/ultralytics">Ultralytics</a> for the YOLO model.
</p>

<h2 align="center"> Contacts </h2>
<p align="justify"> For code bug reports and feature requests feel free to ask in <a href="https://github.com/xN1ckuz/Crosswalks-Detection-using-YOLO/discussions">Discussions</a>. </p>
