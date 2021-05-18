---
title: A Simple Implementation and Application of YOLOv1
date: "2020-09-02T00:12:00.000Z"
template: "post"
draft: false
slug: "simple-implementation-and-application-of-yolo-1"
category: "Unarchived"
tags:
  - " "
description: "This is a simple implementation and application of YOLOv1: a site that can detect up to 20 types of objects in a picture using YOLOv1 method."
socialImage: "/media/detect.jpg"


---

You only look once (YOLO) is a state-of-the-art, real-time object detection system. Below is a simple object detection site that can detect up to 20 types of objects (aeroplane, bicycle, bird, boat, bottle, bus, car, cat, chair, cow, diningtable, dog, horse, motorbike, person, pottedplant, sheep, sofa, train, tvmonitor) using YOLOv1 method 👇.

<style>
@media screen and (max-height: 750px) {
    #showcase, #showcase iframe {
        height: 600px;
    }
}
@media screen and (max-height: 640px) {
    #showcase, #showcase iframe {
        height: 500px;
    }
}
</style>
<div id="showcase" style="height:700px; border:1px solid">
    <!--iframe title="Simple object detection" src="http://120.26.174.117/detect" width="100%" height="700px" loading="lazy"></iframe-->
    <p>There was an iframe here, but I deleted it for better web performance.</p>
</div>
<figcaption>Or open it at <a href="http://120.26.174.117/detect" target="_blank" rel="noopener noreferrer">http://120.26.174.117/detect</a> if the above one goes wrong.</figcaption>

## Usage

1. Upload a jpg or png picture like this one 👇.

   ![Detect image.](/media/detect.jpg)

2. Click the button.

3. Waiting for it to response. (Yes it's very slow, it takes several seconds to compute out the result)

4. Get a detected picture like this one 👇.

   ![Detected image.](/media/detected.jpg)

## More

If you are interested in it, you can read  <a href="https://github.com/OlaWod/flask_e-store/blob/master/myYOLO.py" target="_blank">my code</a> or <a href="https://github.com/OlaWod/my-machine-learning/blob/master/YOLOv1/notes/Yolo1.pdf" target="_blank">my scratch</a> or <a href="https://zhuanlan.zhihu.com/p/32525231" target="_blank">this introduction</a> or <a href="https://pjreddie.com/darknet/yolo/" target="_blank">the official website of YOLO</a> or <a href="https://arxiv.org/abs/1506.02640" target="_blank">the paper</a> or just search on your own.

Thanks for reading!