---
{"dg-publish":true,"permalink":"/adv/houdini-hello/","title":"Hello Houdini","noteIcon":""}
---

## 常用命令

```c
opscript -r -o /obj/* > C:/tmp/nodes.cmd
cmdread C:/tmp/nodes.cmd

// https://www.patreon.com/posts/quick-tip-using-23451468
```

## Houdini 引擎

Curve 2.0 在Unreal与Unity目前不起作用，需要切换到1.0 https://www.youtube.com/watch?v=rN4Y6ucy_hY

## 几何属性

w - vec3 - Angular speed of the particle. This can be thought of as a vector giving the rotation axis with its magnitude being the spin rate. Spin rate is in radians per second.

[VEX Attribute Glossary - kunz](https://wiki.johnkunz.com/index.php?title=VEX_Attribute_Glossary)

## 仓库

Github上分享的Houdini相关的资源 [GitHub - ribponce/particula](https://github.com/ribponce/particula)