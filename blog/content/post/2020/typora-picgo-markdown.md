+++
author = "Raikay"
title = "Typora+Picgo图片自动上传打造Markdown写作神器"
date = "2020-03-11"
description = "Typora+Picgo图片自动上传打造Markdown写作神器"
tags = [
    "markdown"
]

+++

### 1、Markdown神器：Typora

Typora 是一款支持实时预览的 Markdown 文本编辑器。它有 OS X、Windows、Linux 三个平台的版本，并且由于仍在测试中，是完全免费的。



![1713d9f011fd1311](https://gitee.com/raikay/img/raw/master/default/20200405132032.jpg)



官网地址：https://www.typora.io/

### 2、图床工具：Picgo

将本地图片上传到七牛云、腾讯云COS、GitHub等图床。

![image-20200403091618489](https://gitee.com/raikay/img/raw/master/default/20200405132113.png)



### 3、如何实现Typora图片自动上传

##### 1、在Typora 依次点击 文件 --> 偏好设置 --> 图像

![image-20200403093744345](https://gitee.com/raikay/img/raw/master/default/20200405132203.png)

##### 2、点击下载PicGo,填写PicGo启动文件路径

![image-20200403094141548](https://gitee.com/raikay/img/raw/master/default/20200405132226.png)

### 4、PicGo相关设置

> PicGo默认安装了几个插件腾讯云、阿里云、七牛云、GitHub等，这里已blog-uploader这个插件为例，
>
> 这里收集了一些其他插件：https://github.com/PicGo/Awesome-PicGo



点击 插件设置 --> 搜索插件 --> 安装

![image-20200403094531535](https://gitee.com/raikay/img/raw/master/default/20200405132257.png)

点图床设置 --> 掘金 -->  设置为默认图床  -->  确定

![image-20200403095124656](https://gitee.com/raikay/img/raw/master/default/20200405132320.png)

如果没有博客图床，点PicoGo设置，拉到最下面，重新选择一遍即可

![image-20200403095323663](https://gitee.com/raikay/img/raw/master/default/20200405132340.png)

现在可以去到PicGo上传区或者Typora测试一下上传是否成功！

**拖拽：**

![123](https://gitee.com/raikay/img/raw/master/default/20200405132401.gif)

**粘贴板直接粘贴**

> 从粘贴板直接粘贴需要点一下自动上传  



![124](https://gitee.com/raikay/img/raw/master/default/20200405132440.gif)

### 5、相关错误解决：

如果报错先检查一下picgo设置Server是否开启，端口是否是36677



![image-20200403102845372](https://gitee.com/raikay/img/raw/master/default/20200405132459.png)



![image-20200403095752479](https://gitee.com/raikay/img/raw/master/default/20200405132523.png)

![image-20200403103139673](https://gitee.com/raikay/img/raw/master/default/20200405132537.png)



如果报什么upload文件夹错误，检查一下这个路径是否有权限读写权限`C:\Program Files\Typora\upload`

![QQ截图20200405124622](https://gitee.com/raikay/img/raw/master/default/20200405132504.png)

---





掘金现在已不支持外链，如果需要外链可以考虑其他插件七牛云、腾讯阿里码云等....





