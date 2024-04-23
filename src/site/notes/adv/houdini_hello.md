---
{"dg-publish":true,"permalink":"/adv/houdini-hello/","title":"Hello Houdini","noteIcon":""}
---

## 常用命令

```c
opscript -r -o /obj/* > C:/temp/nodes.cmd
cmdread C:/temp/nodes.cmd

// https://www.patreon.com/posts/quick-tip-using-23451468
```

## Houdini 引擎

Curve 2.0 在Unreal与Unity目前不起作用，需要切换到1.0 https://www.youtube.com/watch?v=rN4Y6ucy_hY

## 几何属性

w - vec3 - Angular speed of the particle. This can be thought of as a vector giving the rotation axis with its magnitude being the spin rate. Spin rate is in radians per second.

[VEX Attribute Glossary - kunz](https://wiki.johnkunz.com/index.php?title=VEX_Attribute_Glossary)

## 仓库

Github上分享的Houdini相关的资源 [GitHub - ribponce/particula](https://github.com/ribponce/particula)

## 效果制作

树木吹动模拟 https://vimeo.com/420539635 (Solaris) [来源](https://www.sidefx.com/tutorials/solaris-workshop/)


---

> 看到一种感觉比较好的讲课方式，照着已经做好的文件进行讲解，但在开始前，先将主要的部分内容，进行简单的原理讲解。然后再切换到文件顺着去讲解下去。

* 效果思路
	* Pyro发射源
		* ①pyro trail path, pyro burst source ②当发射源巨大时例如宇宙飞船，可以用Ray节点到表面。再挤出厚度后转VDB。将源物体抽象为一个point，计算出v，再使用VolumeWrangle节点，(`v@collsionVel = point(1, 'v', 0)`) 传递v到vdb上。
	* 自定义曲线旋转力场
		* 创建一个bound VDB vel场，曲线计算curveu和N属性，在volumevop中，使用pcopen filter曲线P,再减去vel的P，得到指向曲线的向量，再与pcopen filter曲线N，叉乘，得到旋转向量，再与曲线N mix就可得到沿曲线方向运动的旋转力场。
	* 蜡烛火焰效果
		* ①在pyro bake volume中，使用Fuel作为Fire的Intensity Volume, temperature做为Color Volume。调节Fire Color Ramp为底部蓝色火焰效果。②<img src="https://cdn.jsdelivr.net/gh/aaronmack/picx-images-hosting@master/e/image.4xubfxmii0.webp" alt="image" width=100/>可以在火焰底部创建一个橙色区域所示的球型volume作为Fire的Color Volume，其中蓝色区域所示为Density可以做为Mask剔除多余的球型区域。③只有temperature与fuel, 使用Volume blur节点，将temperature模糊一下作为Fire的Color Volume输入调节Color Ramp。
	* 冲击波效果
		* 可以使用Line + Resample，与curveu属性。再使用`attribute remap`节点控制`curveu`分别为`pscale`与`startframe`作为pyroburstsource的输入控制。pyroburstsource的Brust Type设置为Shockwave并调整成为冲击波形态。
	* 随机的动画路径
		* 使用Line，头尾固定在原点，noise影响除了头尾的点。
	* 打击效果
		* guideprocess: ①可以使用guideprocess处理毛发引导线的节点来控制类似冲击光束从天空打击地面的效果。Operation设置为Set Length,key帧Scale Factor。②其实应该也可以使用Transform，调整Pivot到线段的顶部，再控制缩放。

* 辅助技巧
	* 体积可视化
		* vel 使用vdb analysis节点，operator属性设置为length，再使用volume visualization可视化。
		* temperature: 使用primitive properties，在Volume选项卡勾选Adjust Visialization。
	* 将Volume置于慢动作
		* 使用Retime节点，Speed调慢，例如0.5，在Volumes选项卡里，将Blend Mode改为Advected。再用Timeshift,使Frame $FF+0.25，这样避免整数帧。因为Blend中间帧会使Volume稍微模糊一点，而在整数帧时又恢复原始的。
	* Wedging
		* 使用Labs File Cache，设置好Attribute后，使用`@<attr_name>`到属性栏中。
	* Viewport更好的可视化Volume
		* 1.工具架sky light。2.取消勾选Display Options->Background->Display Environment Lights as Backgrounds。3.增加Display Options->Texture->3D Textures Limit Resolution分辨率。
	* gl_spherepoints pscale
		* 想要预览pscale属性，可以创建detail属性gl_spherepoints,类型为Integer，值为1。这样就可在窗口中预览。
	* Key动画
		* ①key axiom sourceShape节点中的Density，Fuel这些控制发射源。②key axiom Solver中的Turbulence控制扰乱的作用。③key 材质pyro back volume中的Intensity，Color控制最后渲染效果。
	* Trail的技巧
		* 在trail运动的点时，可以使用timewarp节点,Mode为Fit Range，Input Frame Range与Output Frame Range相同，开启Interpolate Between Input Frames。这样可以插值小数帧，使产生类似火焰中小火花的效果。
* 节点参数
	* Houdini
		* Attrib from volume: 传递Volume中属性到点上，例如vel场到点的v属性上。
		* pyro post process: 当有多个Volume体积时，例如两个density场，可以使用这个节点合并。转换为VDB。转为16bit操作。
		* VDB Smooth SDF: 使用convert vdb将vdb转换成SDF，Convert To设置为VDB，VDB Class设置为Convert Fog to SDF。
		* Pyro Bake Volume
			* 在Pyro Bake Volume节点中的Quick Setups中有Sharpen Volume会创建出一个Subnet去锐化体积。
		* VDB Advect
			* 使用vdb的速度场去影响输入的vdb，例如用vel影响一下输入的density场。
	* Axiom
		* Axiom Source:
			1. Temperature: ①可以自定义Temperature同时存在正值与负值，产生同时往上与往下的效果。②可以用attrib noise float调整temperature增加animate，做出蜡烛火焰底部青色闪烁的效果。
			2. Axiom Source naming: Burn -> Fuel, Divergence-> pressure, v->vel
			3. Sourcing 中的场可以使用Add,Pull,Replace,Blend: 其中Pull会尝试匹配传递进来的场。Add是增加上去。Replace将会完全采用输入的场。
			4. Sourcing Fuel的输入强度可以使用 fit(rand($F), 0, 1, 0.8, 1.5) 完全随机，可以产生蜡烛火焰往上一冲一冲的效果。
			5. Sourcing influence: 可以将pyro burst source节点或者只要有v和density的输入，将density命名成influence，将v命名成influenceVel产生不同效果。
		* Axiom Combustion:
			1. Emission: 当Fuel达到温度燃点时，分别创建的Density,Temperature,Pressure场。
			2. Ignition: Temperature 阈值，仅当温度大于这个值才燃烧。默认为0表示所有都燃烧。
			3. Fuel Burn: 每个Step控制燃烧多少燃料。如果这个值很大，表示所有东西立即燃烧。模拟火球同时可设置为Frame Step。
			4. Fuel Inefficiency:控制百分之多少的无用燃烧。降低燃烧的效果。
			5. Fuel Advect: 是否让fuel流动。小型燃烧中，fuel一般是不流动的。
		* Axiom Houdini:
			1.  Axiom influence：可自定义 influencePressure, influenceTemperature，在Axiom Solver->Sourcing->Influence中取负的Temperature可做出类似干冰效果。
			2. Axiom collision: 添加collision，collisionVel控制碰撞速度。也可添加collisionTemperature。或collision物体key动画运动。
			3. Axiom sink: axiom sourceShape节点中，类型选为sink，添加消失的地方。
			4. Axiom Camera Frustum: axiom sourceShape节点中,使用摄像机裁切解算区域。节省空间。
			5. Axiom Step: ①Frame Step相比于Time Step会比较快。但这两者不与Time Scale有关。②Solver Step会与Time Scale有关。相乘关系。
			6. Axiom Control Field: 使用哪个场控制行为。例如Y-Axis， Range是5-15，则效果会在5-15之间产生，且强度根据Ramp。
			7. Axiom collision: Axiom中VDB命名collision作为碰撞体。带有动画可以加上collisionVel。
			8. Axiom influence: Axiom中使用influenceVel与influence控制场的影响。influence控制范围，influenceVel控制速度。两者需要都存在。
			9. Axiom source: 复制同一发射源多次Merge一起可以在小空间下解算时也产生较浓的烟雾。
		* Axiom Simulation
			1. Velocity Force: 与风相似，不同的是会根据时间加速。
			2. Velocity Turbulence: Turbulence中的Scale Field默认是Density，按照密度扩大，所以当Density非常浓时，Turbulence会非常强烈。
			3. Velocity Disturbance: Disturbance中Cut off Field控制中断，默认是Density，当密度非常淡时受到的Disturbance就会弱。同时Control Field可以选择例如Velocity(Speed)，Range在5-10,这样当速度在5-10区间时才影响Disturbance效果。
			4. Velocity Viscosity: 粘度，当数值很大时可以模拟出类似香烟的烟雾。
			5. Time Scale: 与Time Scale相关的参数Turbulence, 当TimeScale非常大时，变得非常混乱，这时可以用 例如 `5.0*(1.0 / ch("timescale"))`这样参数去控制。
		* Axiom Settings
			1. Sparse - Max Velocity Margin: 当发射源速度非常快时，可以加大这个值扩张计算的边界。
			2. Sparse - Activation Fields: 当不需要Density，纯火时，可以将其改为Density+Temperature, 同时再将 Output -> Compression -> Mask Field 改为Temperature或者Off。
			3. Project Non Divergent: 当你的碰撞不起作用时，可以增加Steps。
* Nuke节点
    * Keylight
	    * 如果天空与主体区分明显，可以用此节点产生Alpha通道。
	* ColorCorrect Mask
		*  用Mask控制ColorCorrect的调整范围，例如产生辉光范围。

### lessons
* Axiom Fundamentals <img src="https://cdn.jsdelivr.net/gh/aaronmack/picx-images-hosting@master/e/image.58h5hno41a.webp" alt="image" width=200/>
* lessons/realistic-explosions <img src="https://cdn.jsdelivr.net/gh/aaronmack/picx-images-hosting@master/e/image.7awxsrtg5u.webp" alt="image" width=200/>

* lessons/big-fire <img src="https://cdn.jsdelivr.net/gh/aaronmack/picx-images-hosting@master/e/image.7egk3fhzwz.webp" alt="image" width=200/>

* lessons/anime-impact-fx
