---
{"dg-publish":true,"permalink":"/adv/coordinate-basis-vector-and-transformation/","title":"Coordinate basis vector and transformation","noteIcon":""}
---


**坐标基矢**

坐标基矢是我们人为定义的事物；一维情形下就是数轴上$0$到$1$之间那个距离，记为

$$(1,)$$

二维有两个坐标轴，分别为

$$(1,0)$$

叫做$i$帽，符号记为$\hat{i}$和

$$(0,1)$$

叫做$j$帽，符号记为 $\hat{j}$ 

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/手绘坐标基.2pvxzz9z2es0.webp" width="790"></div>

刚才我们未引入坐标基矢，引入坐标基矢后，现在就有了一种统一的形式，上述例子的完整情况为

$$\left ( \begin{matrix} 1x+0y=2 \\ 0x+1y=2 \end{matrix} \right.$$ 

矩阵形式

$$\left ( \begin{matrix} 1 & 0 \\ 0 & 1 \end{matrix} \right ) \left ( \begin{matrix} x \\ y \end{matrix} \right ) = \left ( \begin{matrix} 2 \\ 2 \end{matrix} \right ) \ \ (x=2,y=2)$$

其实我们可以看到，我们想要去到的那个点$(2,2)$，本质上就是我们定义的坐标基矢(i帽和j帽)<span style="color:green">"去到了"</span>点$(2,2)$,我们把这种<span style="color:green">"去到了"</span>哪里，称为**变换**
$x=2, y=2$是满足方程的一组数，也就是**坐标基矢变化的量** 这样，每当我们看到像
$$\left ( \begin{matrix} 2 \\ 2 \end{matrix} \right )$$
都应想起是笛卡尔坐标系中的两个**坐标基矢的变换**, 像

$$\left ( \begin{matrix} 1 \\ 1 \end{matrix} \right )$$

可以看做

$$\left ( \begin{matrix} 1 & 0 \\ 0 & 1 \end{matrix} \right ) \left ( \begin{matrix} x \\ y \end{matrix} \right ) = \left ( \begin{matrix} 1 \\ 1 \end{matrix} \right ) \ \ (x=1,y=1)$$

只不过这两个坐标基没有产生任何的变化; 再如
$$\left ( \begin{matrix} 2 \\ 2 \end{matrix} \right )$$
可以看做

$$\left ( \begin{matrix} 2 & 0 \\ 0 & 2 \end{matrix} \right )$$

为了便于描述，我们还通常给我们要去的$(2,2)$那个点称作**向量**，不仅有方向还有箭头，如$\vec{a}=(2,2),\quad a_1=2, \quad a_2=2$这是在二维的情形下
