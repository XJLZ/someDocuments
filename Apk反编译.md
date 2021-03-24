---
title: Apk反编译
date: 2021-01-19 09:32:00
tags:
- Android
- Apk
---

#### 1.工具准备

1） 

[apktool]: https://ibotpeaches.github.io/Apktool/install/

2) 

[dex-tools]: https://github.com/pxb1988/dex2jar

3)  

[jadx-gui]: https://github.com/skylot/jadx

或者

[jd-gui]: http://java-decompiler.github.io/



#### 2.使用

1、将apktool.bat和apktool.jar,然后把这两个工具放到C:\Windows底下

2、命令

```shell
apktool d D:\test\Andorid\demo.apk -o D:\test\Andorid\demo -f
```

​	将apk反编译，目标目录为 -o 后面的参数 ，-f 为覆盖

3、使用压缩软件将apk解压，找到classes.dex

4、解压dex-tools-2.0.zip到任意目录，将classes.dex复制到该解压目录，使用命令

```shell
d2j-dex2jar.bat classes.dex
```

​	会得到一个jar包：classes-dex2jar.jar

5、双击jadx-gui-1.2.0-no-jre-win.exe，打开上面得到的jar包，即可看到源代码