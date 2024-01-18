---
{"dg-publish":true,"permalink":"/adv/space-and-transformation/","title":"Space and transformation","noteIcon":""}
---



**图形学渲染中的空间变换**

什么是空间变换？当我们想渲染一个物品到屏幕中，例如一个小盒子，从小盒子的角度出发，也就是**Local Space**，它看所有的物品都是相对于它来说的，例如小盒子的正前方有一个小球，是相对于小盒子来说它的前方有一个小球，但是对于小球来说，那么就不一定了，可能是小球的后方或者前后左右上下都有可能，那么就会出现小盒子说，不行，大家要以我为参考，小球会说，凭什么以你为参考呢？以我为参考不好吗？争执不下，那这样下去不行，所以最后我们规定某个指定的地方插一个小旗子:triangular_flag_on_post:，就指定这个地方在三维下就是$(0,0,0)$为坐标原点，所有的物品不管是小盒子还是小球或者其它什么物品，都必须以这个原点为相对参考，那么这样就清晰明了了，小盒子相对于原点在$(1,0,0)$处，等等，小球相对于原点在$(0,1,0)$处，这就是**World Space**

当我们观察一个物品时，以我们自己为原点，朝前方规定为朝$-z$方向看去，所以假设有一个小盒子在我们的左手边时，也就是$-x$方向，我们需要朝左转动头部90°，如果在上方的话，也就是$+y$方向，我们就需要朝上转动头部90°，但每次都要转动头部，好麻烦对吧？那么有没有其它的方法呢？还是假设这个小盒子在我们的左手边，如果我们想要观察它，则需要向左转动头部90°对吧，那么我就在想能不能不转动头部而是把这个小盒子拿到我的前方，也就是把这个小盒子相对我来说往右转动90°到我的正前方，仔细想想可以吗？是可以的，排除其它所有的事物，就想象只有我们自己和这个小盒子，**我们朝左转动去看这个小盒子和把这个小盒子朝右转动到我们面前再去看，最终的观察效果是一模一样的**！所以我们给在世界空间中的所有物品再做一个变换，能够让我们观察到，就像类似这个小盒子朝右转动一样，类似这样的变换之后的空间我们称作**View Space**

到了这里，我们知道物品的位置了，还知道了观察的方向，但是似乎还缺少一些东西，那就是如何去观察，就像我们的eyes眼睛一样，你是激光眼，透视眼，还是千里眼，还是写轮眼，眼睛不一样，看到的画面就会有不一样的效果，而我们的眼睛看到的呢就是最普通的透视效果，叫做**Perspective Transformation**，例如两条平行的铁轨<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/PerspectiveProjection.6ujk7r238gs0.webp" width="190"></div>我们最终看到的画面是这种近大远小的，并且超出画面外的物品，也就是我们所看不到的都会被"裁切"掉，这种在指定了我们如何去**观察的方式**之后的我们称为**Clip Space**，当然还有另外一种常用的观察方式叫做**Orthogonal Transformation**

所以对于任何物体，应用**Model Transformation**到**World Space**，然后应用**View Transformation**到**View Space**，再应用**Perspective Transformation**或者**Orthogonal Transformation**到**Clip Space**，再除以w分量到**NDC Space**，最后再应用**Viewport Transformation**到**Screen Space**，**Screen Space**就是最终的显示屏幕, 以上统称为**空间变换**

**透视变换与正交变换**<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/orthographic_perspective_view.6van0bm8j340.webp" width="790"></div>

(https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/Perspective-and-Orthographic-Frustum.424xa80owoe.webp)

透视变换[[adv/perspective_transformation\|perspective_transformation]]是将一个锥内我们所可以看到的事物，最终投射到一"点"处

正交变换[[adv/orthographic_transformation\|orthographic_transformation]]是将一个方体内我们所可以看到的事物，最终投射到一"面"上

视口变换[[adv/viewport_transformation\|Viewport transformation]]

**NDC Space** 

(Normalized device coordinate/规格化设备坐标) 归一化设备坐标或NDC空间是独立于屏幕的显示坐标系统; 它包含一个立方体，其中$x、y$和$z$组件的范围从$−1$到$1$。

**Space Transformation**<div align=center><img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/graphics/空间变换.3uujhna7v4o0.webp"></div>

**W Component**

可以通过增大$w$分量的值，是点朝向原点移动，减小$w$分量的值，可以使点朝向无穷点。(https://stackoverflow.com/questions/2422750/in-opengl-vertex-shaders-what-is-w-and-why-do-i-divide-by-it)

在到clip空间后，我们会根据$-w \leq x,y,z \leq w$来决定丢弃哪些模型,因为这是都是超出了屏幕之外的。 例如$-w \leq x \leq w$是在$x$轴向上超出了，以此类推，这里为什么是与$w$的值比较与在NDC空间中为什么是除以$w$的值是归一化是如出一辙的。

See. MVP_Understanding.hip