---
layout: post
title:  "Image based Plant Growth Analysis System"
comments: true
---

This project is the part of some Smart Farm Projects. It is developed by using TensorFlow open-source software and Python OpenCV. It took 2 months to finish the main module parts and 1 month for the Web UI. In this post, only the main module part will be described.

## Problem Statement

The goal of this project is to extract some plant features from images without measuring each plant by human hands to observe the growth of the plants. To solve this, some object detection and computer vision techniques are going to be used.

## Dataset

There are about 200 images for each section with some labels but labels are uncertain, so it is decided to use images for training without the given labels (date, weather info, count, length). Given images contains compare-objects to measure real length of the plants.

* Tomato Plants
* Tomato Stems
* Tomato Flowers
* Tomato Fruits

**⚠️  All the sample images are made from Google images because of the copy right issues.**

These are samples of each datasets:

![Sample Images](/img/tomato/sample.png){:width="600"}

## Length and Thickness of the Stem

**1) Object Detection**

In tomato plant images, there are black tapes which farmers put. This has to be clearly detected to measure the length of the tomato plants. At first, color filtering and edge detection were applied. It seemed working, but it failed when the brightness of images is different. To solve this problem, [TensorBox](https://github.com/Russell91/TensorBox), which is TensorFlow opensource Object Detection using GoogLeNet-OverFeat algorithm was tried. With this Object Detection, black tapes were extremely well detected regardless of the brightness or any other factors.

For the stem, it was same as Black Tape Detection, but after the detection, the stem was cropped and with this cropped image, edge detection was applied.


Red Circle Detection was also achieved by [TensorBox](https://github.com/Russell91/TensorBox), but after detection, because the bounding box of the detector was too large, so the result of the image was cropped and some circle detection in Python OpenCV was applied.

![Sample Images](/img/tomato/plant2_1.jpg){:width="600"}
![Sample Images](/img/tomato/stem2_1.jpg){:width="600"}
![Sample Images](/img/tomato/stem2_2.jpg){:width="600"}

**2) Exception Handling and Results**

There were some multiple object predictions on one object, so the object that had higher confidence had to be chosen. As a result, in one image, one red circle and two black tapes were detected. After that, since the length of the red circle is known, the length of the tomato plant in the image can be calculated by counting pixels and comparing with detected red circle.

![Sample Images](/img/tomato/plant2_2.jpg){:width="600"}
![Sample Images](/img/tomato/stem2_3.jpg){:width="600"}

<br />

---

## Flower and Fruit Detection

**1) Object Detection**

[TensorBox](https://github.com/Russell91/TensorBox) is used for detecting all the flowers and fruits in a image. In this time, multiple objects have to be detected. However, some other objects were detected during the test. Other objects were including leaves, bud, unripe fruits, some other things. In this detection, the number of flowers and fruits were uncertain because of the detection of other objects.


![Sample Images](/img/tomato/fruit2.jpg){:width="600"}
![Sample Images](/img/tomato/flower2.jpg){:width="600"}

**2) CNN**

To get rid of other objects in the detection process, CNN technique was applied to distinct the flowers and fruits. For the CNN, TensorFlow [Slim](https://github.com/tensorflow/models/tree/master/research/slim) is used and especially, for the pre-trained weight, Inception-ResNet-v2 model was tried to get more high accuracy. In the training, cropped images after the object detection was used and there are two classes including fruit and others. for the flowers, it is assumed that there are flowers and candidates including bud and others. After CNN could get rid of non flowers and non fruits, flowers and fruits were detected better than just applying the object detection.

- **fruit results of CNN**

![Sample Images](/img/tomato/tomato_fruit.png){:width="600"}

- **flower results of CNN**

![Sample Images](/img/tomato/tomato_flower.png){:width="600"}

**3) Semantic Segmentation**

To figure out the shape of flowers and fruits, Semantic Segmentation is used. In here, [KittiSeg](https://github.com/MarvinTeichmann/KittiSeg) TensorFlow opensource software was applied. Trained with masked images from cropped flowers and fruits in object detection. Although the color of fruits and stems were very similar, this could segment only fruits. It was also worked even there was an overlay between multiple fruits and flowers.

- **fruit results of Segmentation**

![Sample Images](/img/tomato/tomato_fruit_seg.png){:width="600"}

- **flower results of Segmentation**

![Sample Images](/img/tomato/tomato_flower_seg.png){:width="600"}
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
