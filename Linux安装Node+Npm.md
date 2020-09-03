#### Linux安装Node+Npm

- node官网下载node（**Linux Binaries (x64)**）安装包 [https://nodejs.org/zh-cn/download/](https://links.jianshu.com/go?to=https%3A%2F%2Fnodejs.org%2Fzh-cn%2Fdownload%2F)
- 执行解压操作

```css
tar -xvf node-v10.16.3-linux-x64.tar.xz 
```

##### **确认一下nodejs下bin目录是否有node 和npm文件,如果有执行软连接，如果没有重新下载**

- 建立软连接，变为全局,对应自己路径

```bash
ln -s /usr/local/src/node-v10.16.3-linux-x64/bin/npm  /usr/local/bin/(此处不改)
ln -s /usr/local/src/node-v10.16.3-linux-x64/bin/node /usr/local/bin/
```

- 验证node是否生效

```undefined
npm -v 
node -v
```

#### npm换源

```
npm config set registry https://registry.npm.taobao.org
// 配置后可通过下面方式来验证是否成功
npm config get registry
```

