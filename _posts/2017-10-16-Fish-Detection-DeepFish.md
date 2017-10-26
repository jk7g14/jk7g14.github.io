---
layout: post
title:  "Fish Detection with Modern Deep Learning Object Detection and Semantic Segmentation in a Production Level"
subtitle: "sdfsdf"
comments: true
---

Before this project, I've already gone through one of the Kaggle competitions about fish detection([The Nature Conservancy Fisheries Monitoring](https://www.kaggle.com/c/the-nature-conservancy-fisheries-monitoring)) and our team developed the fish detection to measuring the length of the fish.

## Problem Statement

The goal of this project is to make the detection techniques to the production level. Because the loading time of the trained weights are so long before the predictions, in the production  level, trained weights should be ready all the time in the server and if the testing data comes, the prediction of this data should be committed immediately without loading the weights.

## A brief explanation of Nature Conservancy Fisheries Monitoring Strategy

This Kaggle Competition was about detecting and classifying fish in fishing boats. I used same techniques as I did in my plant-detection-project. For the detection, I used [TensorBox](https://github.com/Russell91/TensorBox) with
Resnet pretrained weights and for the classification about the species of fish, CNN Slim library with Resnet-Inception_v2 weight is applied. In here, I fine-tuned the threshold that can decide whether the detected objects are fish or not. After I got several results, I used simple ensemble average to get the final result. therefore, I could reach to Top 7% in this competition. I thought that I could do better if I consider the night time of the data and image Semantic Segmentation techniques.


![Sample Images](/img/fish/kaggle.png){:width="600"}

## Dataset

We could collect the fish images easily in ImageNet and goolge, but couldn't get the fish images with golf balls, so we took pictures by ourselves. Golf balls here will be used for calculating the real length of the fish.

* Fish images from ImageNet x 200

![Sample Images](/img/fish/imgnet_samples.png){:width="600"}

* Our fish images with golf balls (taken images from fishering areas) x 600

![Sample Images](/img/fish/our_samples.png){:width="600"}
## Deep Learning Algorithms

**1) Object Detection**

There were two state-of-the-art object detections when we are building a detector. Faster-RCNN and [YOLO_v2](https://pjreddie.com/darknet/yolo/). In here, YOLO version 2 is applied because it is much faster than faster-rcnn and the speed of the prediction is one of the most important factors in this project. we are building a fish detector for people in the real world so I thought that people will not spend time for waiting a result.

The yolo detector which we created detects two objects including a golf ball and fish in an image. The reason to detect the golf ball is to measure real length of fish by comparing with known object length. We considered other objects like Ping-Pong balls, Pen and others, but golf balls are easy to find in the market and not easily damaged, and also it is a sphere so the shape of the objects never change in different angles. In addition, cigarette pack will be added in the future although this is obviously not a good choice because many fishermen normally use their cigarette pack to show how big their fish is.

However, there were issues when we are measuring the length of fish only using the object detection because the prediction of the fish and golf balls are not tightly drawing the rectangle for us. This is why Semantic Segmentation techniques are needed for this project.

![Sample Images](/img/fish/yolo_result.png){:width="600"}

![Sample Images](/img/fish/crop.png){:width="600"}

**2) Semantic Segmentation**

There were two issues after the object detection. First issue was that after the detection, the rectangle result of the prediction is not precise so the length of the objects could be not same as the real length. Second issue was that the result of the fish prediction does not show us the direction of the tail and the head which are the key points of the fish length.

To solve these problems, Semantic Segmentation is applied after the object detection. If we just apply Semantic Segmentation to a whole image, it could be hard to find the fish. Therefore, after the object detection, because the location of the fish is known, we just cropped the fish part and used Semantic Segmentation to find the shape of the fish. The shape of the fish gives us where the tail and the head are and this gives us the precise length of the fish. After this, we could get a Rotated Rectangle in the image.

In the case of golf ball, Hough Circle Detection is used to detect the circle. It fails sometimes, so if we improve the result of the circle detection, we can train our segmentation network same as fish.

For the Semantic Segmentation, [KittiSeg](https://github.com/MarvinTeichmann/KittiSeg) with ResNet pretrained weight was used here.


![Sample Images](/img/fish/seg.png){:width="600"}

- **Final Result**

![Sample Images](/img/fish/result2.png){:width="600"}
<br />

---

## Deploying to Production Environment

When people upload their fish picture through the web or the application, the object detection and Semantic Segmentation have to be committed. In the beginning, our trained weights have to be loaded and then the prediction starts, but loading trained weights when the image comes is time-consuming. Therefore, the load of trained weights needs to start in the beginning of the back-end server so that we do not need to load weights during the prediction every time.

For the back-end server, Flask is used in this project because this is written in Python and our deep network framework TensorFlow is also written Python.

As a result, it took 3 seconds to run the whole process(object detection, semantic segmentation and calculate the length of the fish) with GeForce GTX 1060, and with i7-6650U CPU, it took 4 seconds in the example photo.

- back-end server

![Sample Images](/img/fish/back-end.png){:width="600"}

- front-end server

![Sample Images](/img/fish/front-end.png){:width="600"}

## Collect More Data

If people start to use this application and they put the name of their fish, we could collect large amount of fish data. This means that someday we could give users the name of the fish using the CNN network after training all the data that users have uploaded.

<br />

---

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
  /*
  var disqus_config = function () {
  this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
  this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
  };
  */
  (function() { // DON'T EDIT BELOW THIS LINE
  var d = document, s = d.createElement('script');
  s.src = 'https://jk7g14-github-io.disqus.com/embed.js';
  s.setAttribute('data-timestamp', +new Date());
  (d.head || d.body).appendChild(s);
  })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  {% endif %}
