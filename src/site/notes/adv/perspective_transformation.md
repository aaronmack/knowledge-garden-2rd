---
{"dg-publish":true,"permalink":"/adv/perspective-transformation/","title":"Perspective transformation","noteIcon":""}
---


**透视变换Perspective Transformation**

什么是透视变换？[图片](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/Perspective.4ihgtm58l9u0.webp) 透视变换是一个投影过程，其中透视变换就像将人的眼睛当做一个中心点，外部世界是一个大平面，在眼睛与这个平面之间形成一个椎体，然后将这个平面上的内容投射到眼睛内。

为什么变换之后的空间被称为裁切空间？裁切就是把不需要的部分去除掉，就是这幅图片中一样，投影平面在左侧

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/projection.1onr15yp0sv4.webp" width="490"></div>

而在投影中心另一侧的右侧的点我们是不需要的，假设这个点的坐标为

$$(2,5,10)$$

应用计算之后是

$$x'=\frac{2}{-10}=-0.2$$ 
$$y'=\frac{5}{-10}=-0.5$$
$$z'=\frac{10}{-10}=-1$$ 

那么这个计算过程是如何进行的呢？根据相似三角形

<center><img style="border-radius: 0.3125em; box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/projection.5bbo6m777gs0.webp"><br><div style="color:orange; border-bottom: 1px solid #d9d9d9; display: inline-block; color: #999; padding: 2px;">Figure 1 (从P投射到P')</div></center>

我们有

$$\frac{BA=\color{blue}{z_{blue}}=1}{EA=\color{green}{z_{green}}=3}=\frac{BC=y'}{EF=y}$$ 
$$y'=\frac{y*\color{blue}{z_{blue}}}{\color{green}{z_{green}}}$$

其中，由于我们的摄像机是看向$-z$方向的，所以$\color{green}{z_{green}}$在计算的时候前面要加$-$号，这就是式子$y'=\frac{5}{-10}=-0.5$中$z$的值为$10$，计算是为$-10$

<center><img style="border-radius: 0.3125em; box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/透视校正插值.3imenutihfe0.webp" width="490"><br><div style="color:orange; border-bottom: 1px solid #d9d9d9; display: inline-block; color: #999; padding: 2px;">Look at the projection from another aspect</div></center>

**步骤**

假设我们有一个矩阵 

$$\begin{aligned} \left [ \begin{matrix} 1&0&0&0 \\ 0&1&0&0 \\ 0&0&1&0 \\ 0&0&0&1 \end{matrix}\right ] \end{aligned}$$

我们先让$z$坐标分量等于$-1$,这样计算后的$z'$就等于$1$,就可以满足如Figure 1中的

$$z'=z=1$$

接下来，再调整$w$分量从

$$[0,0,0,1] \rightarrow [0,0,-1,0]$$

这样一来,当除以$w$分量从齐次转三维中的点时，除以$w$分量相当于除以了$-z$分量，经过这样调整后我们有

$$\begin{aligned} \left [ \begin{matrix} 1&0&0&0 \\ 0&1&0&0 \\ 0&0&-1&-1 \\ 0&0&0&0 \end{matrix}\right ] \end{aligned}$$ 

其计算过程正好是我们想要的 

$$\begin{aligned} \left ( \begin{matrix} x' &= x \cdot 1 + y \cdot 0 + z \cdot 0 + 1 \cdot 0 = x \\ y' &= x \cdot 0 + y \cdot 1 + z \cdot 0 + 1 \cdot 0 = y \\ z' &= x \cdot 0 + y \cdot 0 + z \cdot (-1) + 1 \cdot 0 = -z \\ w' &= x \cdot 0 + y \cdot 0 + z \cdot (-1) + 1 \cdot 0 = -z \end{matrix} \right . \end{aligned}
$$

$$\begin{array}{ll} x' = \dfrac{x'=x}{w'=-z},\\ y' = \dfrac{y'=y}{w'=-z},\\ z' = \dfrac{z'=-z}{w'=-z} = 1. \end{array}$$ 

其中$z'=\frac{z}{w} = \frac{-z}{-z} = 1$,　　$x'=\frac{x}{w} = \frac{x}{-z}$,　　$y'=\frac{y}{w} = \frac{y}{-z}$接下来我们就需要详细计算每个部分了

[计算投影点的$x'$和$y'$坐标,将范围映射到[-1,1]](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/PerspProjectionDerivation.6edn0immf7o0.webp) 

$$\left[\begin{array}{cccc} { \frac{2n}{ r-l } } & 0 & ... & 0 \\ 0 & { \frac{2n}{ t-b } } & ... & 0 \\ { \frac{r + l}{ r-l } } & { \frac{t + b}{ t-b } } & ... & {-1}\\ 0 & 0 & ... & 0\\ \end{array}\right]$$

[将投影点的z坐标重新映射到范围[-1,1]](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/Remapping-the-z-coordinate-of-the-projected.2oof8otgmpu0.webp)

(我们根据齐次转三维点时, $(x,y,z)$这三个分量都要除以$w$分量，我们有

$$(x', y', z') = (x/w, y/w, z/w)$$

其中$x$和$y$对$z$无影响)，则上一步骤中的矩阵可以设为

$$\left[\begin{array}{cccc} { \frac{2n}{ r-l } } & 0 & 0 & 0 \\ 0 & { \frac{2n}{ t-b } } & 0 & 0 \\ { \frac{r + l}{ r-l } } & { \frac{t + b}{ t-b } } & \color{red}{A} & {-1}\\ 0 & 0 & \color{red}{B} & 0\\ \end{array}\right]$$ 

其中<span style="color:red">A</span>和<span style="color:red">B</span>是我们要求的,则可以列出等式

$$z' = \dfrac{0 * x + 0 * y + A * z + B * w}{w = -z} \rightarrow \dfrac{A z + B}{w = -z}.$$ 

**注意这里的$w=1$是$(x,y,z,w=1)$中的$w$，而不是这个矩阵中的$w$分量**<br>再根据我们已知$z$正好落在$near$近平面时应当等于$-1$，正好落在$far$远平面时应当等于$1$, 我们可以列出式子

$$\left ( \begin{array}{ll} \dfrac{(z=-n)A + B}{(-z=-(-n)=n)} = -1 &\text{ when } z = n\\ \\ \dfrac{(z=-f)A + B}{(-z=-(-f)=f)} = 1 & \text{ when } z = f \end{array} \right. $$ 
$$ \rightarrow  \left ( \begin{array}{ll} {-nA + B} = -n & (1)\\  {-fA + B} = f & (2) \end{array} \right.$$ 

$$ A=-\frac{f+n}{f-n} $$ 

$$ B=-\frac{2fn}{f-n} $$ 
(也可以映射到[0,1]范围内)

**补充资料**

1. 关于$P'_x = \frac{P_x}{-P_z}$

链接: https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/projection-matrices-what-you-need-to-know-first

2. 为什么除以了$-z$呢？投影过程中的相似三角形中的比例关系

3. 关于透视矩阵中的齐次项的$-1$

链接: https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/building-basic-perspective-projection-matrix

将原来透视矩阵中的$(0,0,0,1)$变为$(0,0,-1,0)$之后, 就说当我们用这个透视矩阵的$w$分量乘以一个齐次点
$$(x,y,z,1)$$
时，我们有
$$x \cdot 0 + y \cdot 0+z \cdot -1+1 \cdot 0 = -z$$
也就是说，这个齐次点$w$的值经过矩阵计算之后从之前的$1$变为了$-z$,同时，齐次点$(x,y,z)$变为三维点时需要经过$(x/w, y/w, z/w)$，此时这样就有
$$(x/w, y/w, z/w) \Rightarrow (x/{-z}, y/{-z}, z/{-z})$$

4. 关于透视矩阵中的$z$项的$-1$

链接: https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/building-basic-perspective-projection-matrix

因为摄像机是朝向$-z$方向的，所以摄像机前所有的点的$z$坐标都是负的，这就是为什么【资料1】中
$$x' = \frac{x}{-z}$$
中的$z$前方带有负号

**透视变换矩阵**

$$\begin{aligned} M_{persp} = \left [ \begin{matrix} \frac{2n}{r-l} & 0 & \frac{l+r}{l-r} & 0 \\ 0 & \frac{2n}{t-b} & \frac{b+t}{b-t} & 0 \\ 0 & 0 & \frac{f+n}{f-n} & \frac{2nf}{n-f} \\ 0 & 0 & 1 & 0 \end{matrix}\right ] \end{aligned}$$ 
或 
$$\begin{aligned} M_{persp} = \left [ \begin{matrix} \frac{2n}{r-l} & 0 & 0 & 0 \\ 0 & \frac{2n}{t-b} & 0 & 0 \\ \frac{r+l}{r-l} & \frac{t+b}{t-b} & -\frac{f+n}{f-n} & -1 \\ 0 & 0 & -\frac{2nf}{n-f} & 0 \end{matrix}\right ] \end{aligned}$$ 

<center>(注意其中两者之间的负号)</center><br>

则我们有

$$[x', y', z', w'] = \left [ \begin{matrix} x \\ y \\ z \\ w=1 \end{matrix} \right ] * M_{persp}$$ 
其中
$$w'=0 \cdot x + 0 \cdot y - 1 \cdot z + 0 \cdot 1 = -z$$

计算并验证一下，假设$n=1，f=20$公式

$$\frac{-\frac{f+n}{f-n} *z -\frac{2fn}{f-n} }{-z}$$

代入$n$和$f$并计算得

$$\frac{-\frac{21}{19} * z - \frac{40}{19}}{-z}$$

其中$\frac{-21}{19}=-1.1$和$\frac{-40}{19}=-2.1$则得到结果

$$z=1 \rightarrow \frac{-1.1*1 -2.1}{-1} = 3.2$$ 

$$z=-1 \rightarrow \frac{-1.1*-1 -2.1}{1} = -1$$ 

$$z=-20 \rightarrow \frac{-1.1*-20 -2.1}{20} = 0.995$$ 

$$z=-21 \rightarrow \frac{-1.1*-21 -2.1}{21} = 1.1$$
