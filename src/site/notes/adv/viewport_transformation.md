---
{"dg-publish":true,"permalink":"/adv/viewport-transformation/","title":"Viewport transformation","noteIcon":""}
---


**Screen Space**

从NDC（三维）到我们的屏幕空间（二维），也就是真正要显示在屏幕上；首先NDC空间范围是$-1$到$1$，而屏幕空间表示像素范围对应的是$(0,0)$到$(w, h)$例如我们屏幕分辨率为$520\times 520$那么就是$(520,520)$，也就是说我们需要从NDC的$(-1,-1)$到$(1,1)$映射到屏幕的$(0,0)$到$(520,520)$;假设有一点顶点为$(-0.5,-0.5,-0.5,1)$，是左下角的一个顶点 （也就是正交变换里我们计算并验证中的那个小盒子上的一个边界点）经过一个正交变换后是$(-1,1,-0.992,1)$，假设有一屏幕长为$520$，宽为$520$顶点$(-0.5,-0.5)$也就是NDC中的$(-1,-1)$会变换到屏幕空间中的$(0,0)$,同理如果是顶点$(0.5,0.5)$，NDC中的$(1,1)$会变成屏幕空间中的$(520, 520)$,我们假设另外一个顶点$(0.25,0.25)$,对应NDC中的$(0.5,0.5)$会变成屏幕空间中的$(390, 390)$<center><img style="border-radius: 0.3125em; box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/ScreenSpace.6m3i63w9eqc0.webp"><br><div style="color:orange; border-bottom: 1px solid #d9d9d9; display: inline-block; color: #999; padding: 2px;">Figure 1</div></center>

根据这些我们可以得到我们的**视口变换矩阵**

$$\begin{aligned} \left [ \begin{matrix} \frac{w}{2} & 0 & 0 & \frac{w}{2} \\ 0 & \frac{h}{2} & 0 & \frac{h}{2} \\ 0 & 0 & \frac{1}{2} & \frac{1}{2} \\ 0&0&0&1 \end{matrix}\right ] \end{aligned}$$ 

（矩阵中的$z$为$\frac{1}{2}$是为保持$z$值不变） 来验证一下,假设我们的屏幕是$520\times 520$的，则矩阵为

$$\begin{aligned} \left [ \begin{matrix} 260 & 0 & 0 & 260 \\ 0 & 260 & 0 & 260 \\ 0 & 0 & 0.5 & 0.5 \\ 0&0&0&1 \end{matrix}\right ] \end{aligned}$$ 

顶点$(0.25,0.25)$，NDC中为$(0.5，0.5)$则

$$\begin{aligned} \left [ \begin{matrix} 0.5&0.5&1&1 \end{matrix} \right ] \end{aligned} \begin{aligned} \left [ \begin{matrix} 260 & 0 & 0 & 260 \\ 0 & 260 & 0 & 260 \\ 0 & 0 & 0.5 & 0.5 \\ 0&0&0&1 \end{matrix}\right ] \end{aligned} \begin{aligned}  \left [ \begin{matrix} 390&390&1&1 \end{matrix}\right ] \end{aligned}$$ 

顶点$(-0.5,-0.5)$，NDC中为$(-1，-1)$则

$$\begin{aligned} \left [ \begin{matrix} -1&-1&1&1 \end{matrix} \right ] \end{aligned} \begin{aligned} \left [ \begin{matrix} 260 & 0 & 0 & 260 \\ 0 & 260 & 0 & 260 \\ 0 & 0 & 0.5 & 0.5 \\ 0&0&0&1 \end{matrix}\right ] \end{aligned} \begin{aligned}  \left [ \begin{matrix} 0&0&1&1 \end{matrix}\right ] \end{aligned}$$

映射到屏幕坐标$(0,0)$像素点（也就是说最左下的那个小盒子的边界点正好映射到屏幕的左下角）

如果长宽是不一样的

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/ScreenSpace.7cnosi0vi480.webp"></div>

长宽不一样我们依然可以处理，只不过要多一个概念$aspect$,

$$aspect=\frac{width}{height}$$

它是一个比例

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/视口变换演示.19ypc5014qkg.webp"></div> 

左上图是比例为$1:1$的情况下，如果我们改变了窗口大小，例如改为了$640\times 480$，也就是比例为$1:1.3333$,如果我们此时什么都不做，则我们会得到左下的结果（仔细想想，为什么，是因为我们的视口变换矩阵，像素是一一对应的，**即使窗口大小变了，但映射关系并没有变**）所以我们如果才能得到右上的结果呢？ 很简单，在进行正交变换或者透视变换的时候将$x$方向也就是水平方向按照$aspect$比例收缩一些
