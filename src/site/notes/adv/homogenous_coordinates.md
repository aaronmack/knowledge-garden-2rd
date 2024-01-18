---
{"dg-publish":true,"permalink":"/adv/homogenous-coordinates/","title":"Homogenous coordinates","noteIcon":""}
---


**齐次坐标**

以二维为例，在平移变换中我们新增一个**坐标分量**叫$w$，让其先等于$1$

则平移的矩阵可以表示为
$$\left ( \begin{matrix} x' \\ y' \\ 1 \end{matrix} \right ) = \left ( \begin{matrix} 1&0&t_x \\ 0&1&t_y \\ 0&0&1 \end{matrix} \right ) \left ( \begin{matrix} x \\ y \\ 1 \end{matrix} \right ) = \left ( \begin{matrix} x+t_x\\ y+t_y \\ 1 \end{matrix} \right )$$
再来看一下用齐次坐标是否可以表示之前的那些变换,均匀缩放
$$\left ( \begin{matrix} x' \\ y' \\ 1 \end{matrix} \right ) = \left ( \begin{matrix} s&0&0 \\ 0&s&0 \\ 0&0&1 \end{matrix} \right ) \left ( \begin{matrix} x \\ y \\ 1 \end{matrix} \right )$$
以及旋转
$$\mathbf{R}_\theta = \left ( \begin{matrix} cos\theta & -sin\theta&0 \\  sin\theta & cos\theta&0 \\ 0&0&1 \end{matrix} \right )$$
都是可以的

[^1]: 可以认为齐次坐标是对一个点的缩放. 用数学表示就是增加一个维度. 该维度表示缩放值. 当该值为0的时候会把所有点汇集到一个相同的点上, 即0点