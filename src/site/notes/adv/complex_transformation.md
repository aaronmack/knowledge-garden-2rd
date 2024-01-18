---
{"dg-publish":true,"permalink":"/adv/complex-transformation/","title":"Complex transformation","noteIcon":""}
---


**变换复合**

变换是可以复合的，考虑一组数

$$\left ( \begin{matrix} -1 \\ 1 \end{matrix} \right ) 和 \left ( \begin{matrix} 2 \\ 2 \end{matrix} \right )$$

其中$\left ( \begin{matrix} 2 \\ 2 \end{matrix} \right )$我们知道是由$\left ( \begin{matrix} 2 & 0 \\ 0 & 2 \end{matrix} \right )$变换而来，而$\left ( \begin{matrix} -1 \\ 1 \end{matrix} \right )$是由$\left ( \begin{matrix} -1 & 0 \\ 0 & 1 \end{matrix} \right )$变换而来，我们是可以将这两个变换组合到一起的,组合到一起最终就是

$$\left ( \begin{matrix} -1 & 0 \\ 0 & 1 \end{matrix} \right )\left ( \begin{matrix} 2 & 0 \\ 0 & 2 \end{matrix} \right )$$

其中变换计算的顺序是从右到左的，如果交换顺序，结果不一定相等；其中后者这个变换如之前一样，在图像上来看，就是缩放，那么前者呢？就是$i$帽从$(1,0)$变到了$(-1,0)$去，$j$帽保持不变，这种变换就像我们平常看书时翻页一样，叫做**Reflection反射**最终结果就是从坐标基(①)开始先作用缩放(②)最后作用反射(③)
<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/mathematics/手绘复合变换.7kd8flleofs.webp" width="790"></div>
