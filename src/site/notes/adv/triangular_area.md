---
{"dg-publish":true,"permalink":"/adv/triangular-area/","title":"Triangular area","noteIcon":""}
---


三角形面积计算

![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.4l1flcfl04y0.webp)

$$
A=\frac{bh}{2}
$$

三角形面积可以用底x高，然后除以2。因为两个一样的三角形可以拼成一个矩形或者平行四边形

## **第一种**

$$
A=\frac{1}{2} ab \sin C
$$

$b \sin C = h$ 还是二分之一底乘高

## **第二种**

$$
A = \frac{abc}{4R}
$$
借助正弦定理[[adv/sine_law\|sine_law]]


## **第三种**

$$
A=\frac{a+b+c}{2} \cdot r
$$

## **第四种** 

余弦定理的延申， 也就是海伦公式， 根据 余弦定理 [[adv/cosine_law\|cosine_law]] 我们有 


$$\cos^2 A=\frac{a^4+b^4+c^4-2a^2b^2+2b^2c^2-2a^2c^2}{4b^2c^2} $$

> 计算验证: ([(-a^2+b^2+c^2)^2](https://zs.symbolab.com/solver/algebra-calculator/%5Cleft(-a%5E%7B2%7D%2Bb%5E%7B2%7D%2Bc%5E%7B2%7D%5Cright)%5E%7B2%7D?or=input))

根据

$$\cos^2A+sin^2A=1$$

$$ \sin A=\sqrt{1-\cos^2a}=\sqrt{\frac{4b^2c^2}{4b^2c^2}-\frac{a^4+b^4+c^4-2a^2b^2+2b^2c^2-2a^2c^2}{4b^2c^2}}$$

$$ =\sqrt{\frac{4b^2c^2-(a^4+b^4+c^4-2a^2b^2+2b^2c^2-2a^2c^2)}{4b^2c^2}}$$

$$ =\frac{\sqrt{-a^4-b^4-c^4+2b^2c^2+2c^2a^2+2a^2b^2}}{2bc}$$
有三角形面积
$$\Delta=\frac{1}{2}bc \sin A$$

代入$\sin A$有

$$\Delta=\frac{1}{4}\sqrt{(a+b+c)(-a+b+c)(a-b+c)(a+b-c)}$$ 
(排列组合)


> [!INFO] 变体
> 

$$
\Delta = \sqrt{p(p-a)(p-b)(p-c)}
$$
其中 
$$
p=\frac{a+b+c}{2}
$$



[^1]: [几何图形面积公式的发展简史，从“海伦公式”到“高斯公式” - 知乎](https://zhuanlan.zhihu.com/p/378369630)