---
{"dg-publish":true,"permalink":"/adv/field-of-view/","title":"Field of view","noteIcon":""}
---


**FOV (Field-Of-View) and aspect ratio**

**原理**

透视矩阵中我们使用$top、bottom、left、right、near、far$来定义，但通常我们不这样做，而是使用$fov、aspect\ ratio、near、far$来定义，但他们本质上都是一样的，像人的眼睛也是有$fov$，叫做[视度](https://baike.baidu.com/item/%E4%BA%BA%E7%9C%BC%E8%A7%86%E5%BA%A6/5997035#)这么一说的,顾名思义，视度越大，我们看到的事物也就越多，例如$fov90$就比$fov60$看到的多，它是视点中心到视锥左侧与视点中心到视锥右侧所张开的角度，当然这样只能计算出左右侧的，还需要一个$aspect\ ratio$(屏幕纵横比)来计算上下侧的。

**计算**<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/FOV-image.5ljfnjoc8mc0.webp"></div>

根据三角函数
$$\begin{array}{l} \tan( \dfrac{ FOV } {2}) = \dfrac{ opposite } { adjacent } = \dfrac {BC}{AB} = \dfrac{top}{near} \\ top = \tan( \dfrac{ FOV } {2}) * near \\ bottom = -top \end{array}$$
如果我们的$aspect \quad ratio$(宽高比)为$1$的话，则
$$\begin{array}{l} right = top\\ left = bottom = -top \end{array}$$
但通常我们的屏幕的宽高比都不是$1:1$的，如
<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/AspectRation.3dftg4ztsu00.webp"></div>

图中右边所示，我们有公式

$$\frac{width}{height}=\frac{right}{top}$$ 
$$\frac{width}{height}=\frac{left}{bottom}$$ 
(宽与左右对应，高与上下对应) 则我们有
$$\begin{array}{l} right = top * aspect \quad ratio \\ left = bottom * aspect \quad ratio \end{array}$$
其中
$$aspect\quad ratio = \frac{width}{height}$$ 
$$bottom = -top$$