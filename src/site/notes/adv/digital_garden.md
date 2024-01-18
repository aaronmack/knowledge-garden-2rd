---
{"dg-publish":true,"permalink":"/adv/digital-garden/","title":"Digital Garden","noteIcon":""}
---


# 题记

最早有一天是想改变自己的做笔记的方式，思考了半晚上，最后得到了个结论就是，笔记应当记成图，而不是树，遂第二天去查资料，看到这篇，顿时找到自己想要的~

[玩转 Obsidian 04：为什么推荐使用 Obsidian 做知识管理 - 少数派](https://sspai.com/post/67339)

笔记可以这样做，之前我们一般是使用树的层级结构去记笔记，但我们知道知识与知识之间一般是有联系的，知识与知识之间应该是一张图，通过相互之间的联系进行关联。

软件上使用Obsidian，但我们还想发布到网上，可以使用社区插件Digital Garden进行设置和发布，再使用免费的vecel进行部署。

# 安装


在[^1]中的文档写的非常详细，每一步，里面需要你知道一些关于Github的与Vecel的配置。

# 其它

1. https://github.com/oleeskild/Obsidian-Digital-Garden 社区插件的地址

2. https://github.com/oleeskild/digitalgarden 插件的配套网页模板，Vecel就是去构建它然后再部署。e.g. 我的笔记网站 https://github.com/aaronmack/knowledge-garden 就是使用的这个模板。

3. https://github.com/jackyzha0/quartz 还有个这个，可以self-hosting或使用Github-pages，也有Vevel的部署方式。两者选其一就可以了，建议就用那个插件，简单方便。

# 发布你的笔记

在你的笔记中，根据文档 https://dg-docs.ole.dev/getting-started/02-commands 的介绍，你需要添加dg-publish属性并且值为on，这样这个插件就会识别到你想要发布的内容了。

然后再在Obsdian的左侧栏有个小叶子，点击那个就可以一键发布。


[^1]: https://dg-docs.ole.dev/getting-started/01-getting-started/