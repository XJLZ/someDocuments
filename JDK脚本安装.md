---
title: JDK脚本安装
date: 2020-08-11 15:32:00
tags:
- JDK
- Java
- Centos7
---



#### 脚本

**脚本与压缩包同目录**

```
#!/bin/bash
#判断是否安装有openJDK
yum list installed |grep -e java -e jdk
if [ $? -eq 0 ]
then
        read -p "继续执行将卸载已有JDK,y确定，其他退出?" choose
        if [ $choose=="y" ]
        then
                yum -y remove java-* &> /dev/null
                yum -y remove tzdata-java* &> /dev/null
        else
                exit 1
        fi
fi

#判断安装包是否存在
if [ -f jdk-8u261-linux-x64.tar.gz ]
#判断是否已经安装
then
        java &> /dev/null
        if [ $? -eq 0 ]
        then
                echo "已经安装JDK"
                exit 1
        else
                echo "开始安装JDK"
                tar zxvf jdk-8u261-linux-x64.tar.gz -C /usr/local
                echo "JDK安装完成，开始配置环境变量"
	        cat /dev/null > /etc/profile.d/jdkconf.sh
                echo 'export JAVA_HOME=/usr/local/jdk1.8.0_261' >> /etc/profile.d/jdkconf.sh
                echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile.d/jdkconf.sh
                echo 'export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar' >> /etc/profile.d/jdkconf.sh
                echo "环境变量配置完成"
        fi
else
        echo "请将当前shell脚本和安装包放在同一目录"
fi

chmod +x /etc/profile.d/jdkconf.sh

source /etc/profile.d/jdkconf.sh

echo "======JDK安装完成，请使用命令：java -version检查是否成功安装,若失败请执行source /etc/profile.d/jdkconf.sh======="



```

#### rpm包

```
#!/bin/bash
#判断是否安装有openJDK
yum list installed |grep -e java -e jdk
if [ $? -eq 0 ]
then
        read -p "继续执行将卸载已有JDK,y确定，其他退出?" choose
        if [ $choose=="y" ]
        then
                yum -y remove java-* &> /dev/null
                yum -y remove tzdata-java* &> /dev/null
        else
                exit 1
        fi
fi

#判断安装包是否存在
if [ -f jdk-8u261-linux-x64.rpm ]
#判断是否已经安装
then
        java &> /dev/null
        if [ $? -eq 0 ]
        then
                echo "已经安装JDK"
                exit 1
        else
                echo "开始安装JDK"
                rpm -ivh jdk-8u261-linux-x64.rpm &>/dev/null
                echo "JDK安装完成，开始配置环境变量"
                echo 'export JAVA_HOME=/usr/java/jdk1.8.0_261-amd64' >> /etc/profile.d/jdkconf.sh
                echo 'export PATH=$JAVA_HOME/bin:$PATH' >> /etc/profile.d/jdkconf.sh
                echo 'export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar' >> /etc/profile.d/jdkconf.sh
                echo "环境变量配置完成"
        fi
else
        echo "请将当前shell脚本和安装包放在同一目录"
fi
chmod +x /etc/profile.d/jdkconf.sh

sh /etc/profile.d/jdkconf.sh

echo "======JDK安装完成，请使用命令：java -version检查是否成功安装======="
```

