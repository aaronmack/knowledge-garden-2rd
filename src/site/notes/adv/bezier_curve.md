---
{"dg-publish":true,"permalink":"/adv/bezier-curve/","title":"Bezier Curve","noteIcon":""}
---

# Graphics-Bezier Curve

> Bezier曲线在例如Photoshop中的钢笔工具等工业软件中有很重要的应用，可以帮助艺术家制作出一些直观上的光滑的曲线；其中最主要的部分是**伯恩斯坦多项式**，也就是下方代码中的`compute`函数

$$
B_{i,n}(t) = \left ( \begin{matrix} n\\i \end{matrix} \right ) t^i(1-t)^{n-i}
$$
这是个多项式喔。

<div class="caption">
    Bernstein Polynomial.
</div>

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/demonstration/BezierCurve.3r4fli5ucs00.gif" alt="image" />


<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/demonstration/BezierHoudiniNode.4yjg8bfl2xw0.png" alt="image" />

```c
vector compute(vector p1, p2; float t){
    return (1-t)*p1+t*p2;
}

vector gen_bezier( vector p[]; float t ){ 
    vector ps[] = p;  // Note: p is reference, so we need a new variable
    int iter = len(ps);
    for(int z=0; z<iter; z++){
        int n=len(ps);
        vector tmp[];
        for(int i=0;i<n-1;i++){
            vector cr = vector(compute(ps[i], ps[i+1], t));
            push( tmp, cr); 
        }
        ps=tmp;
        iter--; 
    }

    return compute(ps[0], ps[1], t);
}


int step=chi("step");
float interval=1.0/step;
int prim0 = addprim(0, "polyline");
vector tp[];
int np = npoints(0); 
int i=0; 
while(i<np){
    push(tp, vector(point(0, "P", i)));
    i++; 
}

for(float t=0;t<1;t+=interval){
    int pt=addpoint(0, gen_bezier(tp, t));
    addvertex(0, prim0, pt);
} 
```

**原理**:

Bernstein多项式与概率密切相关(假设我们在做一个实验，做了n次，成功i次的概率是多少？怎么计算？就是上面的那个公式)，我们知道假设的概率，概率的范围是`[0,1]`，则我们多尝试一些，会有

<img src="https://cdn.jsdelivr.net/gh/aaronmack/aaronmack.github.io@master/assets/img/graphics/BernsteinPolynomial.png" alt="image" />

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.5jkfxvr1cik0.webp" alt="image" />

<div class="caption">
    Bernstein Polynomial. Figure from - <b>Wolfram MathWorld</b>
</div>

它的值总是往所取值处的尖峰处靠拢，再进行迭代，就得到Bezier曲线了

***

[贝塞尔曲线中的伯恩斯坦多项式（Bernstein Polynomial） - 知乎](https://zhuanlan.zhihu.com/p/366082920) - 前面的还能读懂，后面就只知其意，不知其理了。看来还是只能听懂大白话。

[贝塞尔曲线入门](https://pomax.github.io/bezierinfo/zh-CN/index.html) - 一个非常全的介绍贝塞尔曲线的网站。