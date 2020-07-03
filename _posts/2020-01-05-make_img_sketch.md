---
layout: post
title: "Make your image look like a sketch using python"
date: 2020-01-05
excerpt: ""
tags: [Python, CV]
comments: false
---



With python and `PIL` package, you can easily make you image look like sketch.

{% highlight python %}

#!/usr/bin/env python
# encoding: utf-8
 
from PIL import Image, ImageFilter, ImageOps

img_path = '～/Downloads/img.png'
img_save_path = '～/Downloads/img_sketch.png'

# load your image 
img = Image.open(img_path)

def dodge(a, b, alpha):
    return min(int(a*255/(256-b*alpha)), 255)

def draw(img, blur=2, alpha=1.0, img_save_path=img_save_path):
    '''
    comment 
    '''
    img1 = img.convert('L') # grey scale
    img2 = img1.copy()
    img2 = ImageOps.invert(img2)

    for i in range(blur):
        img2 = img2.filter(ImageFilter.BLUR)

    w, h = img1.size 

    for i in range(w):
        for j in range(h):
            a = img1.getpixel((i, j))
            b = img2.getpixel((i, j))
            img1.putpixel((i, j), dodge(a, b, alpha))

    img1.show()
    img1.save(img_save_path)


draw(img)

{% endhighlight %}

The results are shown  below.
{% highlight html %}
<figure class="half">
    <img src="/_img_/20190105/img1.png">
    <img src="/_img/20190105/img_sketch.png">
    <figcaption>sketch</figcaption>
</figure>
{% endhighlight %}