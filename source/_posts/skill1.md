---
title: '移动端适配之视口和meta标签'
series: skill
---

这是关于移动端适配的第一篇文章，这篇文章主要介绍**视口以及和视口有关的meta标签**的使用。

不管三七二十一，我们先新建一个页面：

    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>这不是一个demo</title>
            <style type="text/css">
                *{margin: 0; padding: 0;}
                div{height: 100px; background: red;}
            </style>
        </head>
        <body>
            <div></div>
        </body>
    </html>

在谷歌浏览器下，我们可以看到不同手机的适配情况：  
![iPhone5下](/img/bVbgcA4?w=716&h=480 "iPhone5下")

![图片描述](/img/bVbgcBf?w=786&h=362 "图片描述")

![图片描述](/img/bVbgcBh?w=860&h=332 "图片描述")

可以看出，不管是i5/i6/i6plus下，div的宽度都为980px，这个980是什么值，为什么它好像和移动设备无关？

其实这个980所表示的就是**布局视口**。  
**布局视口layout viewport** ：就是移动设备上用来装载我们的网页的那一块区域。浏览器厂商为了让用户在小屏幕下网页也能够显示地很好，所以把布局视口宽度设置地很大，一般在 768px ~ 1024px之间。不同的设备有不同的宽度。最常见的宽度是980。下图是不同设备下布局视口的大小。

![300958470402077.png](/img/bV9WZA?w=653&h=89 "300958470402077.png")

布局视口有980像素，可是我们的屏幕很小，按理说应该会有滚动条才是，可实际上并没有。这就需要归功于另一个视口：**视觉视口**。

**视觉视口visual viewport**：屏幕上显示的页面的一部分。听起来很玄乎，可是你认真看下面这张图，你就能明白视觉视口(visual viewport)和布局视口(layout viewport)的关系了：  
![图片描述](/img/bVbggWD?w=1026&h=800 "图片描述")  
![图片描述](/img/bVbggWJ?w=1040&h=790 "图片描述")

此时，因为我们的视觉视口 = 布局视口，所以没有出现滚动条。虽说是没有滚动条，但是pc端能友好显示的页面，在移动端上就显示的很怪异。字体很小，很难看清。比如亲爱的百度：  
![图片描述](/img/bVbgcCP?w=788&h=908 "图片描述")

如果想让字体大小可读，又该怎么办？在此之前，我们还需要了解另一个视口，**理想视口**。  
**理想视口ideal viewport**：它提供了设备上理想网页的大小。这个值可以在[不同设备的理想视口](http://viewportsizes.com/)查到。常用的有：i5是320，i8是375，plus是414。

扯了那么多，那要如何适配呢？  
相信一定会有一些前辈，他们苦口婆心地告诉你，你只要加这一行代码就可以了：

    <meta content="width=device-width, initial-scale=1.0" name="viewport">
    

怀有好奇心的我们，勇敢地作出了尝试：

    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta content="width=device-width, initial-scale=1.0" name="viewport">
            <title>这不是一个demo</title>
            <style type="text/css">
                *{margin: 0; padding: 0;}
                div{height: 100px; background: red;}
            </style>
        </head>
        <body>
            <div>hello world</div>
        </body>
    </html>

得到了如下的效果：  
![图片描述](/img/bVbgg0A?w=708&h=446 "图片描述")  
![图片描述](/img/bVbgg0M?w=806&h=418 "图片描述")  
![图片描述](/img/bVbgg0Z?w=796&h=396 "图片描述")

字体在三大尺寸下，显示都算是友好，但是奇怪的是div的宽度不再是980，而是和设备的宽度有关。这一切，都得归功于我们刚刚加的`width=device-width`了。

`width=device-width`，这句代码的意思就是把布局视口 = 理想视口。那既然布局视口跟着变了，那刚刚的视觉视口又算咋回事啊，它现在的值等于多少？这不还有我们刚刚设置的另一个代码`initial-scale=1.0`，它的作用就是改变视觉视口的。

`initial-scale`指的是缩放系数。其中有这样的公式：

> 视觉视口宽度 = 理想视口宽度 / 缩放系数  
> 缩放系数 = 理想视口宽度 / 视觉视口宽度

拿i5的机型来说，根据公式可得：  
视觉视口宽度 = 320 / 1 = 320  
布局视口宽度 = 320  
所以：视觉视口宽度 = 布局视口宽度。页面无滚动条。

那如果我改变`initial-scale`的值会有什么反应呢，同样我以i5的机型做示范：

    initial-scale = 0.5 「 div宽度640，页面无滚动条 」

![图片描述](/img/bVbgg2K?w=694&h=358 "图片描述")

    initial-scale = 1 「 div宽度320，页面无滚动条 」

![图片描述](/img/bVbgg0A?w=708&h=446 "图片描述")

    initial-scale = 1 「 div宽度320，页面有滚动条 」

![图片描述](/img/bVbgg3P?w=702&h=630 "图片描述")

现在我们好好来捋捋：

    initial-scale = 0.5 「 div宽度640，页面无滚动条 」
    视觉视口宽度 = 320 / 0.5 = 640
    布局视口宽度 = 320
    又因为：视觉视口不能大于布局视口，所以此时，将布局视口的宽度提高等于640
    总结：视觉视口 = 布局视口 = 640

    initial-scale = 1 「 div宽度320，页面无滚动条 」
    视觉视口宽度 = 320 / 1 = 320
    布局视口宽度 = 320
    总结：视觉视口 = 布局视口 = 320

    initial-scale = 2 「 div宽度320，页面有滚动条 」
    视觉视口宽度 = 320 / 2 = 160
    布局视口宽度 = 320
    总结：视觉视口 < 布局视口 页面出现了滚动条。

最后总结：

1.  **页面是否出现滚动条，看布局视口和视觉视口的大小**
2.  **页面元素的宽度取决于布局视口的大小**

留个思考题：为什么在未设置<meta>的情况下，页面无滚动条，浏览器做了哪些默认的设置？

参考：

1.  [ppk大神讲解视口第一篇](https://www.quirksmode.org/mobile/viewports.html)
2.  [ppk大神讲解视口第二篇](https://www.quirksmode.org/mobile/viewports2.html)
3.  [ppk大神讲解视口第三篇](https://www.quirksmode.org/mobile/metaviewport/)