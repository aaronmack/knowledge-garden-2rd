---
{"dg-publish":true,"permalink":"/adv/equation-and-determinant/","title":"Equation and determinant","noteIcon":""}
---


**方程与行列式**

有一个方程，我们试着解一下

$$\begin{array}{l} \left ( \begin{matrix}  5x+6y=7 \quad (1) \\ 9x+4y=3 \quad (2) \end{matrix} \right. \end{array}$$
我们用消去法
$$\begin{array}{l} (2) \times \frac{5}{9} - (1) \\ (1)\times \frac{4}{6} - (2) \end{array}$$
最终可以得到
$$\begin{array}{l} y(5\times 4-6\times 9)=3\times 5-7\times 9 \\ x(5\times 4-6\times 9)=7\times 4-6\times 3 \end{array}$$

大家有没有注意到$x$和$y$后面的
$$(5\times 4-6\times 9)$$
他们是一样的，所以为了简化计算，我们将这个$(5\times 4-6\times 9)$拿出来排列成
$$\left ( \begin{matrix} 5 & 6 \\ 9 & 4 \end{matrix} \right ) $$
这样一组数,再定义
$$\left ( \begin{matrix} 5 & 6 \\ 9 & 4 \end{matrix} \right ) = 5\times 4 - 6\times 9$$
叫做**行列式**, 而
$$\left ( \begin{matrix} 5 & 6 \\ 9 & 4 \end{matrix} \right ) $$
也是方程中的系数(称作**系数矩阵**)，通用格式就是
$$\left ( \begin{matrix} a&b \\ c&d \end{matrix} \right ) = ad-bc$$

---

再看另外一个方程
$$\left ( \begin{matrix}  1x+0y=0 \\ 0x+1y=0 \end{matrix} \right.$$ 
列出系数矩阵形式
$$\left ( \begin{matrix} 1 & 0 \\ 0 & 1 \end{matrix} \right )$$

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/手绘二维坐标系.qt5rv7k809s.webp" width="490"></div>

观察图中我们看到如果我们把$(1,0)$和$(0,1)$"围起来"，我们会得到一个正方形，在应用我们刚刚学的的行列式的计算方法，我们正好得到一个数字$1\times 1-0\times 0=1$，(同样的$2\times 2-0\times 0=4$，面积为4)，如果我们抛开坐标系不谈论，就单纯的把围成的区域看做是一个正方形，它的面积正好为1，不是吗？那是不是我们可以把**行列式与面积联系起来**呢？答案是：可以，我们再来看另外一个例子，我们现在讨论的是(1,0)和(0,1)下围成的区域，它正好是一个正方形，那么其它的情况呢？

<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/行列式的一般情形.7gtqx3nj4x80.webp" width="790"></div>

我们两个坐标$(a,c)$和$(b,d)$系数矩阵为
$$\left ( \begin{matrix}  a & b \\ c & d \end{matrix} \right )$$
现在形成的面积是一个平行四边形，经过计算后我们发现也满足。
