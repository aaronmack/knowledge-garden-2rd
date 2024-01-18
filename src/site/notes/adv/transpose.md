---
{"dg-publish":true,"permalink":"/adv/transpose/","title":"Transpose","noteIcon":""}
---


**转置**

假设我们有一个矩阵
$$A=\left ( \begin{matrix} 5 & 6 \\ 9 & 4 \end{matrix} \right )$$
它表示了从$2$维到$2$维空间的变换，对应任意的$m \cdot n$矩阵，都表示一个从 n 维空间到 m 维空间的变换，例如我们有一个$1 \cdot 2$矩阵$\left ( \begin{matrix} 1 & -2 \end{matrix} \right )$，它表示了一个从2维空间到1维空间的变换，如图中我们可以看到$(1,-2)$变换到了$-1$<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/2维到1维的变换.3zimar96skc0.webp" width="520"></div>可以想象成一部机器，喂入两个数，得到一个数，具体的操作过程与**投影**[[adv/projection\|projection]]有关,就像一个小人在数轴上，先移动了$1$步，然后又移动了$-2$步，那么最终看来，这个小人只移动了$-1$步,即后退了一步

而转置呢，操作上来说就是*行变为列，列变成行*，转了一下，例如
$$\left ( \begin{matrix} 1 & -2 \end{matrix} \right )$$
转过来就是
$$\left ( \begin{matrix} 1 \\ -2 \end{matrix} \right )$$ 
(即我们应当结合维度变换的角度去理解转置)

将这些操作转换成代数上就是
$$\left ( \begin{matrix} 1 & -2 \end{matrix} \right ) \cdot \left ( \begin{matrix} 1 \\ 1 \end{matrix} \right ) = 1\cdot 1+ (-2\cdot 1)=-1$$
我们使用$\cdot$这个小点来表示这种"走了1步，又走了-2步"这种计算，而这种计算呢，又恰巧就是我们待会要讲到的**点乘**[[adv/dot_product\|dot_product]]

* 补充：关于更加详细的**坐标变换**， 线性代数的本质 [【搬运】【线性代数】线性代数的本质\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV18J411T7vS)
