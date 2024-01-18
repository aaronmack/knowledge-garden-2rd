---
{"dg-publish":true,"permalink":"/adv/linear-transforms-and-translation/","title":"Linear transforms and translation","noteIcon":""}
---


**线性变换Linear Transforms**

**缩放Scale**
$$\left ( \begin{matrix} x'\\ y' \end{matrix} \right ) = \left ( \begin{matrix} s&0\\ 0&s \end{matrix} \right ) \left ( \begin{matrix} x\\ y \end{matrix} \right )$$

  

[UniformScale](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/UniformScale.mp4 ':include :type=video controls width=100% height=360px')

  

**非均匀缩放 Scale (Non-Uniform)**
$$\left ( \begin{matrix} x'\\ y' \end{matrix} \right ) = \left ( \begin{matrix} s_1&0\\ 0&s_2 \end{matrix} \right ) \left ( \begin{matrix} x\\ y \end{matrix} \right )$$

  

[NonUniformScale](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/NonUniformScale.mp4 ':include :type=video controls width=100% height=360px')

  

**反射Reflection**
$$\left ( \begin{matrix} x'\\ y' \end{matrix} \right ) = \left ( \begin{matrix} -1&0\\ 0&1 \end{matrix} \right ) \left ( \begin{matrix} x\\ y \end{matrix} \right )$$

  

[Reflection](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/Reflection.mp4 ':include :type=video controls width=100% height=360px')

  

**错切Shear**
$$\left ( \begin{matrix} x'\\ y' \end{matrix} \right ) = \left ( \begin{matrix} 1&a\\ 0&1 \end{matrix} \right ) \left ( \begin{matrix} x\\ y \end{matrix} \right )$$

  

[ShearMatrix](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/ShearMatrix.mp4 ':include :type=video controls width=100% height=360px')

  

[Shear](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/Shear.mp4 ':include :type=video controls width=100% height=360px')

  

**旋转Rotate**<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/手绘-Rotate.3lljascgw0k0.webp" width="790"></div>

$$\mathbf{R}_\theta = \left ( \begin{matrix} \color{teal}{cos\theta} & \color{black}{-sin\theta} \\  \color{teal}{sin\theta} & \color{black}{cos\theta} \end{matrix} \right )$$

  

[Rotation](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/Rotation.mp4 ':include :type=video controls width=100% height=360px')

  

**平移Translation

**平移**

平移不是一个线性变换，它是一个**仿射变换**，因为线性变换其中的一个特性就是变换前后坐标系的原点保持不变，显然平移不满足这个特性；或者还可以说我们无法用一个矩阵去描述这个变换，不像之前的旋转缩放等变换我们都可以用一个矩阵就可以描述。再从另外一个角度来描述就是我们"无法通过只使用乘法来描述这个变换"

我们假设一个场景，在一维情景下，我们有一个数轴，还有三个点在
$$a=1,\ b=2,\ c=3$$
处，我们想要将这三个点往右移动一个单位,也就是
$$a'=a+1;\ b'=b+1,\ c'=c+1$$
这是对这三个点都使用"+1"这个一个同样的操作我们做到了，那么我们可不可以使用乘法呢？我们观察到，$a'=2,\ a=1$，要想使用乘法就需要乘以$2$，因为之间相差$2$倍，也就是$a'=2a$,但是对于另外两个点，$2b$和$2c$则不是我们想要的结果，问题我们是否可以找到一个像"+1"这么一个统一又优美的操作，使乘法也可以作用于变换呢？答案是，我们暂时找不到

所以，Translation我们需要使用这种方式来表示
$$\left ( \begin{matrix} x'\\ y' \end{matrix} \right ) = \left ( \begin{matrix} a&b\\ c&d \end{matrix} \right ) \left ( \begin{matrix} x\\ y \end{matrix} \right ) + \left ( \begin{matrix} t_x\\ t_y \end{matrix} \right )$$
可这样表示后面会多出
$$\left ( \begin{matrix} t_x\\ t_y \end{matrix} \right )$$
使得偏偏平移变换与其它的变换**不同**，那有没有其它的方法呢？答案是: [[adv/homogenous_coordinates\|homogenous_coordinates]] 齐次坐标


**变换复合**

[[adv/complex_transformation\|complex_transformation]]变换复合，很容易就理解

$$
\mathbf{T} \quad
\mathbf{R} = 
\left ( 
\begin{matrix} 1 & 0 & 1 \\ 0 & 1 & 0 \\ 0 & 0 & 1 
\end{matrix} 
\right ) 
\left ( 
\begin{matrix} \cos45^\circ & -\sin45^\circ & 0 \\ \sin45^\circ & \cos45^\circ & 0 \\ 0 & 0 & 1 
\end{matrix} 
\right )
$$

Here[^1]

3维下
$$\left ( \begin{matrix} x' \\ y' \\ z' \\ 1 \end{matrix} \right ) = \left ( \begin{matrix} 1&0&0&t_x \\ 0&1&0&t_y \\ 0&0&1&t_z \\ 0&0&0&1 \end{matrix} \right ) \left ( \begin{matrix} x \\ y \\ z \\ 1 \end{matrix} \right ) = \left ( \begin{matrix} x+t_x \\ y+t_y \\ z+t_z \\ 1 \end{matrix} \right )$$

[^1]: For math render, we use `\mathbf{T}\_{x} ` instead of `\mathbf{T}_{x} `, But some doesn't support.