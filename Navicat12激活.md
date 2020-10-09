---
title: Navicat12激活
date: 2020-08-10 15:32:00
tags:
- Navicat
---



本教程更新时间：2019-4-19 16:37:38

https://www.52pojie.cn/thread-934566-1-1.html

本文所需软件
1、navicat_premium原版安装包
官网下载地址：https://www.navicat.com.cn/download/navicat-premium
2、注册工具
github地址：https://github.com/DoubleLabyrinth/navicat-keygen
以上本文所需文件已整理到网盘，自取：
链接：https://pan.baidu.com/s/1MDuDFBsS0EI-rz4WkQ7kJw
提取码：gdn5
复制这段内容后打开百度网盘手机App，操作更方便哦

\~~~~~~~~~~~ 分割线~~~~~~~~~~~~~~

好了，教程开始
**1、安装原版navicat**
正常安装，一直下一步，直到安装成功，这个我就不截图了。
默认安装路径是：C:\Program Files\PremiumSoft\Navicat Premium 12
**2、开始激活**
**2.1、首先需要先替换掉Navicat激活公钥**
a、解压navicat-keygen-for-x64.zip文件，开始执行cmd命令
b、打开命令提示符（管理员），win+x
(因为我是安装在了C盘，文件写入需要管理员权限，如果安装在别的盘，普通的命令提示符就行，或者在解压文件夹按住shift点右键，也能选择命令提示符打开)

![img](https://attach.52pojie.cn/forum/201904/19/160557tm0i0zk08q0a9f99.png)


c、进入到navicat-keygen-for-x64.zip文件的解压目录，执行如下命令

```
cd [解压目录]
.\navicat-patcher.exe "C:\Program Files\PremiumSoft\Navicat Premium 12"
```



![img](https://attach.52pojie.cn/forum/201904/19/161149zmt1zxgarvrqatj2.png)


d、回车继续，一顿飘屏猛如虎，好的，出现这样的提示表示替换成功。成功之后会在当前文件夹下生成RegPrivateKey.pem文件

![img](https://attach.52pojie.cn/forum/201904/19/161405f9gdzno4z3d4ob5d.png)


**2.2、接下来我们就要开始生成序列号和激活码**
a、和替换公钥类似，执行如下命令

```
.\navicat-keygen.exe -text .\RegPrivateKey.pem
```


b、接下来你会被要求选择Navicat产品类别、语言以及输入主版本号。之后会随机生成一个序列号。

![img](https://attach.52pojie.cn/forum/201904/19/162347wpp8fo7pktogsavt.png)


c、得到序列号，复制下来，注意：**窗口不要关闭**

d、此时打开安装的原版Navicat，第一次打开会出现如下提示，点击注册按钮，进入注册页面

![img](https://attach.52pojie.cn/forum/201904/19/155913jd4w4coog0sx25n5.png)



![img](https://attach.52pojie.cn/forum/201904/19/162730x44dr8xzfg77zwbj.png)


e、接下来输入用户名，组织名和请求码，然后回车两次，获得激活码

![img](https://attach.52pojie.cn/forum/201904/19/163431if96i5uu2610fs0s.png)


f、把获得到的激活码粘贴到navcat中，点击激活，激活成功。

![img](https://attach.52pojie.cn/forum/201904/19/163431yrza4l445rprd888.png)


**附上激活成功的截图。**

![img](https://attach.52pojie.cn/forum/201904/19/163432k6rbb5wdbb116pp2.png)



![img](https://attach.52pojie.cn/forum/201904/19/164734oam0stsajsuotaja.png)