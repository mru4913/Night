---
layout: post
title: "Calculate IOU using Python"
date: 2019-12-28
excerpt: ""
tags: [EN, Python, Deel Learning, CV]
comments: false
---

In computer vision prediction with bounding boxes, one way to measure the similarity between anchor boxes and the ground-truth bounding box is IOU (Intersection over uion), simply shown as following:

\\[ \text{IOU} = \frac{\text{the area of the interection}}{\text{the are of the union}} \\]

The following code is used to achieve this method.

{% highlight python %}

#!/usr/bin/env python
# encoding: utf-8
 
 def cal_iou(rect1, rect2):
    '''
    calculate IoU ratio of the intersecting area to the union area of two bounding boxes.
    parameters:
    ----------
    rect1: array-like (y1, x1, y2, x2) where (x1, y1) coordinates 
            in the upper-left corner and the (x2, y2) coordinates 
            in the lower-right corner of the rectangle 1 
    rect2: array like (y1, x1, y2, x2)

    return:
    ------
    IOU value (scalar) between two rectangles
    '''

    # decide the upper-left and bottom-right coordinates of the intersect rectangle 
    top = max(rect1[0], rect2[0])
    left = max(rect1[1], rect2[1])
    bottom = min(rect1[2], rect2[2])
    right = min(rect1[3], rect2[3])

    # check whether two rectangles have an intersection 
    if left < right and top < bottom:
        # this shows that two rects truly have an intersection
        # calculate area of each rectangle
        s_rect1 = (rect1[2] - rect1[0]) * (rect1[3] - rect1[1])
        s_rect2 = (rect2[2] - rect2[0]) * (rect2[3] - rect2[1])
        
        # calculate the area of the intersection
        intersection = (right - left) * (bottom - top)
        return 1.0 * (intersection) / (s_rect1 + s_rect2 - intersection)
    else:
        return 0.0

{% endhighlight %}