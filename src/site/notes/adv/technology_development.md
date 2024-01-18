---
{"dg-publish":true,"permalink":"/adv/technology-development/","title":"Technology Development","noteIcon":""}
---


# Wave Function Collapse
## 概念

**双缝干涉** - 当没有观测时,光子呈现的是波的性质.当观测时,光子呈现的是粒子的性质.

**薛定谔的猫** - 当匣子里的猫被观测时,猫的状态要么死要么活,只能在两者之间二选一,而没有观测的话,那么猫的状态既不死也不活

**熵** - 物理意义表示物质的混乱程度,熵越大,说明物质越混乱,熵越小,说明物质越稳定,例如水加热变成雾,这个过程熵值增加(熵增),水降温变成冰,这个过程熵值减少(熵减)

**波函数塌陷** - 从混乱变成确定的现象,就叫做波函数塌陷

**算法的核心原理** - 就是动态使候选对象的范围变得越来越小,直到最后所有的位置都能够选取到合适的对象. 而如何动态使候选对象范围变小呢? 就是通过约束规则,传播和回溯.

1. **规则，传播** - 波函数坍缩的算法传播思路可以根据已坍缩位置的邻居进行传播.把规则套进去,对每个邻居的可选集合进行处理.
2. **回溯** - 把出错的方案排除,然后再回退.重新选一遍.这就是回溯的意义

## 过程

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.3zipv3vc1ue0.webp" alt="image" width=600 />

结合可视化中的示例与下方的文字会更好理解。

假设我们有一个空间，我们把这个空间划分为若干个格子，每个格子在初始状态下有很多种可能性，很多种状态，我这里假设有ABCD四种种状态好了。

状态：ABCD（A，AB，CD）
规则：A-C,D 匹配，A-A,B互斥，C-A匹配, D-B匹配，C-C,D互斥 （下面灵魂画手的图片会比较明了）

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.1vyjv7wuvy68.webp" alt="image" width=520/>

1. 每个空间小格在初始状态都处于超态状态，它有ABCD四种可能状态。

2. 我们对其中一个小格进行观测，假设对中间的小格进行观测好了，这个小格由于受到了我们的观测所以它会发生塌陷，从超态塌陷为确定态，这里假设这个小格塌陷成了A确定状态。
3. 当中间的小格塌陷成一个确定状态以后，周围小格会受到中间小格的影响也会发生塌陷。根据规则，那么周围的一圈空间只能是状态C,D，并且这里在坍缩后会熵减，并且影响周围。
4. 我们再扩大一圈，那些状态只能是C,D的格子的周围格子又只能是状态A,B了。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.1kziiicpzls0.webp" alt="image" />
5. 这样循环往复，迭代每次熵最低的那个，然后塌缩，如果发现规则冲突无法塌缩，则回溯到之前保存的状态，继续。

## 多维

三维空间下生成建筑的例子

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.2sr1bpkix7s0.webp" alt="image" width=500/>

二维空间下生成地图的例子

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.3jwl5bxpce00.webp" alt="image" width=500/>

## 参考


可视化 - [wfc2d 算法过程可视化](https://anseyuyin.github.io/wfc2D/demos/algorithmVisualization/)

[GitHub - marian42/wavefunctioncollapse: Walk through an infinite, procedurally generated city](https://github.com/marian42/wavefunctioncollapse)

[基于《波函数坍缩算法》的无限城市程序化随机生成 - 知乎](https://zhuanlan.zhihu.com/p/66416593)

描述比较简单易懂的 - [虚幻4渲染编程(程序化世界篇)【第四卷：Wave Function Collapse生成算法】 - 知乎](https://zhuanlan.zhihu.com/p/364474077)

# Interior Mapping


这个技术实现了一种假室内效果。

## 代码

```c
Shader "MyShader/InteriorMappingTest"
{
	static float3 up=float3(0,1,0); 
	static float3 right=float3(1,0,0); 
	static float3 forward=float3(0,0,1);

	fixed4 frag (v2f i) : SV_Target
    {
		
    }
}



```

[UNITY SHADER GRAPH with Fake Interiors Shader - YouTube](https://www.youtube.com/watch?v=tXtu8Yzp7I0)

# Billboard Cloud