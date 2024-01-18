---
{"dg-publish":true,"permalink":"/adv/orthographic-transformation/","title":"Orthographic transformation","noteIcon":""}
---


**正交变换Orthographic Transformation**

什么是正交变换？[图片](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/orthographic.24gyysktzse8.webp) 正交变换也是一个投影过程，这里则不像透视变换中那样是一个椎体了，而是一个方体，所以在正交变换中，近大远小这种情况则不存在，相反，你会看到所有的物体无论远近，在投影平面上都是有相仿的大小

**步骤**

创建一个能够包含的下场景内所有物体的一个BoundingBox, $(l:left, r:right, t:top, b:bottom, f:front, b:back)$,然后将其映射到$(-1,-1,-1)$到$(1,1,1)$范围内；$x$坐标映射到$(l, r) \Rightarrow (-1,1)$, $y$坐标映射到$(t,b) \Rightarrow (1,-1)$，然后将内容投射到投影平面上

**资料**

1. 正交矩阵计算过程

https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/orthographic-projection-matrix

**正交变换矩阵**
$$\begin{aligned} M_{ortho} \left [ \begin{matrix} \frac{2}{r-l} & 0 & 0 & -\frac{r+l}{r-l} \\ 0 & \frac{2}{t-b} & 0 & -\frac{t+b}{t-b} \\ 0 & 0 & \frac{2}{n-f} & - \frac{n+f}{n-f} \\ 0&0&0&1 \end{matrix}\right ] \end{aligned}$$

计算并验证一下，首先场景中那个包含所有物体的那个BoundingBox的值我们需要知道，这里假设场景中有一个正方体尺寸为$1x1x1$,并且放置在世界中心原点处，那么它的左右上下边界值分别为
$$(l:-0.5,　r:0.5,　t:0.5,　b:-0.5)$$
然后再设置远近平面我们假设为
$$(n:0.01,　f:100)$$
则我们有
$$left=-0.5,　right=0.5,　top=0.5,　bottom=-0.5,　near=0.01,　far=100$$
根据这些我们最终可以计算出矩阵
$$\begin{aligned} M_{ortho} \left [ \begin{matrix} 2 & 0 & 0 & 0 \\ 0 & 2 & 0 & 0 \\ 0 & 0 & -0.02 & -1.002 \\ 0&0&0&1 \end{matrix}\right ] \end{aligned}$$ 
正方体其中一个顶点$(-0.5, -0.5, -0.5)$经过正交变换后有
$$\begin{aligned}  \left [ \begin{matrix} -0.5&-0.5&-0.5&1 \end{matrix}\right ] \end{aligned} \begin{aligned}  \left [ \begin{matrix} 2 & 0 & 0 & 0 \\ 0 & 2 & 0 & 0 \\ 0 & 0 & -0.02 & 0 \\ 0&0&-1.002&1 \end{matrix}\right ] \end{aligned}\begin{aligned}  \left [ \begin{matrix} -1&-1&-0.992&1 \end{matrix}\right ] \end{aligned}$$ 
再举一个例子
$$\begin{aligned}  \left [ \begin{matrix} -0.6&-0.5&-100.1&1 \end{matrix}\right ] \end{aligned} \begin{aligned}  \left [ \begin{matrix} 2 & 0 & 0 & 0 \\ 0 & 2 & 0 & 0 \\ 0 & 0 & -0.02 & 0 \\ 0&0&-1.002&1 \end{matrix}\right ] \end{aligned}\begin{aligned}  \left [ \begin{matrix} -1.2&-1&1.002&1 \end{matrix}\right ] \end{aligned}$$
可以看到，后者超出了$(-1,1)$范围会被裁切掉