---
{"dg-publish":true,"permalink":"/adv/linear-and-nonlinear-depth/","title":"Linear and nonlinear depth","noteIcon":""}
---


**深度**

**线性深度**
$$F_{depth}=\frac{z-near}{far-near}$$

接近近平面的时值为$0$，远平面时为$1$，但我们通常不使用这个深度，因为我们的投影特性导致插值不是线性的(由于在投影平面上的相同步长随着三角形面与相机之间的距离增加而在三角形面上产生更大的步长-《Mathematics for 3D Game Programming and Computer Graphics-P107) (正交投影与透视投影的视锥不同，所以不存在透视投影中的近大远小效果，在这样的情况下，正交投影中的深度可以使用线性深度)

**非线性深度**

>In detail see `WorldGrid.fs.glsl`
$$\begin{aligned} F_{depth} = \frac{\frac{1}{z}-\frac{1}{near}}{\frac{1}{far}-\frac{1}{near}} \end{aligned}$$


<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/x分之1的函数图像.1wcc3u4z1sow.webp"></div>

观察$\frac{1}{x}$的图像我们可以知道，将$x$看做$z$，透视变换中我们也比上了$z$，所以最终总体的结果与这个函数的图像是类似的，通过观察我们可以看到$[0,0.5]$之间的区域占了总共了的$\frac{4}{5}$还多，这也就意味着，**距离屏幕越近的模型将会获得越高的精度，反之，越远精度越低，而线性则是非常平均的，无论远近，精度一致**，这只是一个观察结果

在[[adv/perspective_transformation\|perspective_transformation]]透视变换矩阵除以$w(w=-z)$分量进入NDC空间之前，它们是线性的，除以了$w$分量之后进入了NDC空间，就变为了非线性

函数图像链接: https://www.geogebra.org/m/zqnpanc7

See. MVP_Understanding.hip