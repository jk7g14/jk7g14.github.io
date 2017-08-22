---
layout: post
title:  "Image based Plant Growth Analysis Sysytem"
comments: true
---

This project is the part of some Smart Farm Proejcts. It is developed by using TensorFlow opensources and Python OpenCV. It took 2 months to finish the main module parts and 1 month for the Web UI. In this post, only the main module part will be described.

## Problem Statement

The goal of this project is to extract some plant features from images wihout measuring each plant by human hands to observe the growth of the plants. To solve this, some object detection and computer vision techniques are going to be used.

## Dataset

There are about 200 images for each section with some labels but labels are uncertain, so it is decided to use images for training without the given labels (date, weather info, count, length). Given images contains compare-objects to measure real length of the plants.

* Tomato Plants
* Tomato Stems
* Tomato Flowers
* Tomato Fruits

**⚠️  All the sample images are made from google images becuase of the copy right issues.**

These are samples of each datasets:

![Sample Images](/img/tomato/sample.png){:width="600"}

## Length and Thickness of the Stem

**1) Back Tape and Red Circle Detection**

In tomato plant images, there are black tapes which farmers put. This has to be clearly detected to measure the length of the tomato plants. At first, color filtering and edge detection were applied. It seemed working, but it failed when the brightness of images is different. To solve this problem, TensorBox, which is TensorFlow opensource Object Detection using GoogLeNet-OverFeat algorithm was tried. With this Object Detection, black tapes were extremely well detected regardless of the birghtness or any other factors. 

**2) Red Circle Detection**

Red Circle Detection was also achieved by TensorBox, but after detection, because the bounding box of the detector was too large, so the result of the image was cropped and some circle detection in Python OpenCV was applied.

**3) Exceoption Handling and Results**
There were some mutiple object predictions on one object, so the object that had higher confidence had to be chosen. As a result, in one image, one red circle and two black tapes were detected. After that, since the length of the red circle is known, the length of the tomato plant in the image can be calculated by counting pixels and comparing with detected red circle.

<br />

---

## Flower and Fruit Detection 
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
