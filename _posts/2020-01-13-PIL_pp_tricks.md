---
layout: post
title: "PIL image preprocess tricks using Python"
date: 2020-01-13
excerpt: ""
tags: [EN, Python, CV]
comments: false
---

{% highlight python %}
import numpy as np 
from PIL import Image
from PIL import ImageFilter
from PIL import ImageEnhance
{% endhighlight %}

# Preprocess images using PIL

- Open 
- Filter
- Contrast Change
- Grey scale
- Tricks to denoise 
- Binarize 
- Crop

We can use `PIL` package to preprocess images in order to do some other stuff(?). For example, we have the following image and we want to extract clear character from the images.


{% highlight python %}
# Open images 
img = Image.open('./code.png')
img
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_2_0.png)




{% highlight python %}
# Filter
# There are many filters, like averaging and maximgum filter. 
# Each one is used for different purpose.
# Here, we use a median fileter to keep the edge and the details of the imgage
img = img.filter(ImageFilter.MedianFilter(5)) 
img
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_3_0.png)




{% highlight python %}
# Contrast Change
# Change paramters 'factor' to change the contrast
# We can increase the contrast to highlight characters in compare to other noise 
img = ImageEnhance.Contrast(img).enhance(1.3)#enhance()
img
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_4_0.png)




{% highlight python %}
# Scale change
# We do not care abhttps://mru4913.github.io/Night/posts/img/20200113/out the color 
# change it to grey scale 
img = img.convert('L')
img
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_5_0.png)




{% highlight python %}
# However as you can seewe still have some unnecessary 
# lines in the background 
# we need to find a way to remove those lines 
# this approach has to be carefully used b
def remove_pixel(img, vrange=(150,220)):
    pixdata = img.load()
    w, h = img.size
    for j in range(h):
        for i in range(w):
            if pixdata[i,j] > vrange[0] and  pixdata[i,j] < vrange[1]:
                pixdata[i,j] = 255

    return img
{% endhighlight %}


{% highlight python %}
img = remove_pixel(img)
img
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_7_0.png)



This is another way to denoise. For each pixel, we look its surrounding pixels. If it has more than 2 surrounding pixels belong to background, we label as noise. However, it does not work well in this case. 

{% highlight python %}
def denoise(im):
    pixdata = im.load()
    w, h = im.size
    for j in range(1,h-1):
        for i in range(1,w-1):
            count=0
            if pixdata[i,j-1] > 245:
                count = count+1
            if pixdata[i,j+1] > 245:
                count = count+1
            if pixdata[i+1,j] > 245:
                count = count+1
            if pixdata[i-1,j] > 245:
                count = count+1
            if count > 2:
                pixdata[i,j] = 255
    return im
{% endhighlight %}


{% highlight python %}
# Binarize
# Now, we can make the characters clearer to see by binarizing.
def binarizing(img, threshold):
    pixdata = img.load()
    w, h = img.size
    for j in range(h):
        for i in range(w):
            if pixdata[i,j] < threshold:
                pixdata[i,j] = 0
            else:
                pixdata[i,j] = 255
    return img
{% endhighlight %}


{% highlight python %}
img = binarizing(img, 200)
img
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_10_0.png)




{% highlight python %}
# Crop
# Then we may need to crop imgages
n = 4
w, h = img.size
imgg_w = w/n
for i in range(n):
    img.crop((0 + imgg_w * i, 0, imgg_w * (i+1), h)).show()
{% endhighlight %}


{% highlight python %}
# Numpy to PIL image 
Image.fromarray(np.random.randint(0,255,(30,30)).astype('uint8'))
{% endhighlight %}




![png](https://mru4913.github.io/Night/posts/img/20200113/output_12_0.png)




{% highlight python %}
# PIl image to array
np.array(img)
{% endhighlight %}


```
array([[255, 255, 255, ..., 255, 255, 255],
        [255, 255, 255, ..., 255, 255, 255],
        [255, 255, 255, ..., 255, 255, 255],
        ...,
        [255, 255, 255, ..., 255, 255, 255],
        [255, 255, 255, ..., 255, 255, 255],
        [255, 255, 255, ..., 255, 255, 255]], dtype=uint8)
```


