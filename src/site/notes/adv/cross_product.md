---
{"dg-publish":true,"permalink":"/adv/cross-product/","title":"Cross product","noteIcon":""}
---


叉乘只在三维中有定义，<span style="color:purple">两个向量的叉乘可以产生一个与这两个向量都垂直的新向量</span>。例如计算一个物体表面的法向量

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/叉乘.1eb9eyex849s.webp" width="290"></div>

**叉乘定义**

$${\displaystyle \mathbf {a} \times \mathbf {b}=\left\|\mathbf {a} \right\|\left\|\mathbf {b} \right\|\sin(\theta)\mathbf{n}}$$

其中$\theta$表示$\mathbf{a}$向量和$\mathbf{b}$向量之间的夹角，而 ${\displaystyle \mathbf {n} }$则是一个与${\displaystyle \mathbf {a} }$、${\displaystyle \mathbf {b} }$所构成的平面垂直的单位向量，方向由右手定则决定 (https://zh.wikipedia.org/wiki/%E5%8F%89%E7%A7%AF)

**模长计算**

$${\displaystyle \left\|\mathbf {a} \times \mathbf {b} \right\|=\left\|\mathbf {a} \right\|\left\|\mathbf {b} \right\|\sin(\theta)}$$

<span style="color:purple">模长等于以两个向量为边的平行四边形的面积</span>

**叉乘计算**

**Part1**
叉乘计算规则：如果两个向量一样，叉乘结果为$0$
通过引入单位向量，向量就可以转化成代数形式，例如

$$a=a_1i+a_2j+a_3k, \ \ b=b_1i+b_2j+b_3k$$

$i，j，k$是三个相互垂直的向量。它们刚好可以构成一个坐标系。这三个向量就是

$$i=(1,0,0), \ j=(0,1,0), \ k=(0,0,1)$$

**Part2**

$$\mathbf{a}=(a_1,a_2,a_3),\mathbf{b}=(b_1,b_2,b_3)$$

分别为两三维向量，叉乘为

$$\mathbf{a}\times \mathbf{b}=\ <a_2b_3-a_3b_2,\ \ a_3b_1-a_1b_3,\ \ a_1b_2-a_2b_1>$$

也可以写成**伪行列式**的形式[[adv/equation_and_determinant\|equation_and_determinant]]

$$a\times b = \left ( \begin{matrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ a_1 & a_2 & a_3 \\ b_1 & b_2 & b_3 \end{matrix} \right ) = \mathbf{i}(a_2b_3-a_3b_2) + \mathbf{j}(a_3b_1-a_1b_3) + \mathbf{k}(a_1b_2-a_2b_1)$$

$$\mathbf{a}\times \mathbf{b} = \left ( \begin{matrix} a_2b_3-a_3b_2 \\ a_3b_1-a_1b_3 \\ a_1b_2-a_2b_1 \end{matrix} \right )$$

**Part3**
先以二维为例，假设有一个向量$\mathbf{a}=(a_1,a_2)$然后我们引入反对称矩阵

$$\mathbf{H} = \left ( \begin{matrix} 0&-1 \\ 1&0 \end{matrix}\right )$$

然后计算

$$\mathbf{a}\mathbf{H}\mathbf{a}^T$$

得结果为

$$\left ( \begin{matrix} a_1&a_2 \end{matrix} \right )  \left ( \begin{matrix} 0&-1 \\ 1&0 \end{matrix} \right )  \left ( \begin{matrix} a_1 \\ a_2 \end{matrix} \right ) = \left ( \begin{matrix} a_1&a_2 \end{matrix} \right )  \left ( \begin{matrix} -a_2 \\ a_1 \end{matrix} \right ) = \left ( \begin{matrix} a_1(-a_2) + a_2a_1 \end{matrix} \right ) = 0$$

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/反对称矩阵比较.190iia091ml.webp"></div><div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/反对称变换-图解.6sycqrfdsxk0.webp"></div>

由叉乘的规则我们有

$$a\times a = [a]_\times * a = 0$$

其中$[a]_\times$表示某个叉乘矩阵，然后作用到了$a$得结果为$0$,通过对比，我们可以发现，$\mathbf{a}\mathbf{H}$就是向量a的叉乘矩阵，当$\mathbf{a}$为列向量时，$\mathbf{a}^T\mathbf{H}$为a向量的叉乘矩阵，如果向量$\mathbf{a} = (a_1,a_2,a_3)$为三维向量，那么H为

$$\mathbf{H} = \left ( \begin{matrix} H_1 \\ H_2 \\ H_3 \end{matrix}\right ) \ \mathbf{H_1} = \left ( \begin{matrix} 0&0&0 \\ 0&0&\color{green}{-1} \\ 0&\color{green}{1}&0 \end{matrix}\right ) \ \mathbf{H_2} = \left ( \begin{matrix} 0&0&\color{blue}{1} \\ 0&0&0 \\ \color{blue}{-1}&0&0 \end{matrix}\right ) \ \mathbf{H_3} = \left ( \begin{matrix} 0&\color{red}{-1}&0 \\ \color{red}{1}&0&0 \\ 0&0&0 \end{matrix}\right )$$

最后将变换合并起来就是

$$\mathbf{H} = \left ( \begin{matrix} 0&\color{red}{-1}&\color{blue}{1} \\ \color{red}{1}&0&\color{green}{-1} \\ \color{blue}{-1}&\color{green}{1}&0 \end{matrix}\right )$$

则最终有

$$\mathbf{a}\mathbf{H} = \left ( \begin{matrix} 0&\color{red}{-a_3}&\color{blue}{a_2} \\ \color{red}{a_3}&0&\color{green}{-a_1} \\ \color{blue}{-a_2}&\color{green}{a_1}&0 \end{matrix}\right )$$

$$\mathbf{a}\times \mathbf{b} = \mathbf{A}*\mathbf{b} = \left ( \begin{matrix} 0&-a_3&a_2 \\ a_3&0&-a_1 \\ -a_2&a_1&0 \end{matrix} \right )\left ( \begin{matrix} b_1 \\ b_2 \\ b_3 \end{matrix} \right )$$

**Part4**
根据内积和外积的定义 [[adv/dot_product\|dot_product]]

$$(\mathbf{a}\times \mathbf{b})\cdot \mathbf{a} =<a_2b_3-a_3b_2,\ \ a_3b_1-a_1b_3,\ \ a_1b_2-a_2b_1> \cdot \mathbf{a} \\ =\ a_1(a_2b_3-a_3b_2) + a_2(a_3b_1-a_1b_3) + a_3(a_1b_2-a_2b_1) \\ $$ 

$$= \color{red}{a_1a_2b_3}-\color{orange}{a_1a_3b_2}+\color{blue}{a_2a_3b_1}-\color{red}{a_2a_1b_3}+\color{orange}{a_3a_1b_2}-\color{blue}{a_3a_2b_1} = 0$$

假设有两个不共线的向量，分别为$(a_1,a_2,a_3),(b_1,b_2,b_3)$,我们设我们要找的垂直于这两个向量的向量为$(x,y,z)$，那么我们则有如下方程

$$\left ( \begin{matrix} a_1&a_2&a_3 \\ b_1&b_2&b_3 \end{matrix} \right )\left ( \begin{matrix} x \\ y \\ z \end{matrix} \right ) = \left ( \begin{matrix} 0 \\ 0 \end{matrix} \right ) =0$$ 

$$\Rightarrow \left ( \begin{matrix} a_1x + a_2y + a_3z=0 \\ b_1x + b_2y + b_3z=0 \end{matrix} \right.$$ 

There is a footnote here [^1]

(这里可以看做向量$(a_1,a_2,a_3)$和$(b_1,b_2,b_3)$分别与要求向量$(x,y,z)$的点乘，如果垂直点乘结果为$0$) 令式子中的$z=1$,则我们有

$$\left ( \begin{matrix} a_1x+a_2y=-a_3 \quad (1) \\ b_1x+b_2y=-b_3 \quad (2) \end{matrix}\right .$$

因为我们的方程组的秩小于未知数的个数，这里不妨设$z=1$然后再求解)
然后解二元一次方程，另$(1)\times \frac{b_1}{a_1}-(2)$得

$$y=\frac{a_1b_3-a_3b_1}{a_2b_1-a_1b_2}$$

与

$$x=\frac{a_3b_2-a_2b_3}{a_2b_1-a_1b_2}$$

因此所求向量为

$$(\frac{a_3b_2-a_2b_3}{a_2b_1-a_1b_2}, \ \frac{a_1b_3-a_3b_1}{a_2b_1-a_1b_2}, \ 1)$$ 

改变一下形式我们有

$$(a_3b_2-a_2b_3, \ a_1b_3-a_3b_1, \ a_2b_1-a_1b_2)$$

这个形式与之前的形式相差了一个$-$号，还有另外一种解法使用$(2)\times \frac{a_1}{b_1} - (1)$所得的结果的形式与之前的形式相同

补充(https://github.com/Krasjet/quaternion/blob/master/quaternion.pdf)
 * 叉乘的定义在历史顺序上来说是从Graßmann积中导出

[^1]: For math render, use `\begin{cases}` instead of `\left {` that doesn't supported. Or use `\left (` with end of `\right.`