---
{"dg-publish":true,"permalink":"/adv/unity-live-capture/","title":"Unity Live Capture","tags":["unity"],"noteIcon":""}
---

## 提要

在微博上看到樱花兔的视频，其中提到了Unity Face Capture，遂来捣鼓一番。

在App Store中搜索 Unity Face Capture,注意要打完整才能搜索出来，安装之后打开是获取一堆权限，全部同意后就可以在UnityEditor->Package Manager中安装Live Capture这个包。安装成功后在Window -> Live Capture -> Connections中开启服务了。

注意此时需要关闭防火墙，当然也有其它的方法，这里为了测试简单就直接关闭了防火墙后才可以链接成功。

同样，可以在App Store中搜索Unity Virtual Camera并安装。

## 步骤

### Face Capture

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.22m0l5c6gwtc.webp" alt="image" />

这里有Samples可以导入。

定位到Assets->Samples->Live Capture->3.0.0->ARKit Face Sample，打开FaceCaptureSample。

点击运行游戏。

再打开手机上的App，连接成功后即可看到Unity中的效果。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.5ftn8or4ekw0.webp" alt="image" />

<img src="http://xyzzyxwz.top:8080/wp-content/uploads/2023/11/20231125_023235000_iOS.jpg" alt="image" width="150" />

这是一个微笑的效果，以及在Editor中的效果。

[Unity Face Capture - Easy Tutorial (2022)](https://www.youtube.com/watch?v=UNW78Z8pvSU)

[Unity【Live Capture】- 关于人脸捕捉的解决方案-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2086337)

### Virtual Camera

1. 在场景中新建Virtual Camera Actor。并且更改Transform的位置等。
2. 在场景中新建Take Recorder，在大纲中选择新建的TakeRecorder，在Inspector->Take Recorder->Capture Devices,点击小+号，新建一个VirtualCameraDevice，再选择New Virtual CameraDevice(默认新建后是这个名称)，在Inspector中将Camera设置为刚刚新建的Virtual Camera Actor
3. 创建完上面两步后，就可以启动Game，并且同时打开手机App

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.5oyt6qqpp3o0.webp" alt="image" />

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.2xmlj65nwlo0.webp" alt="image" />

[Live Captue Apps Startup Guide PDF](https://forum.unity.com/attachments/live-capture-apps-startup-guide-pdf.961348/)