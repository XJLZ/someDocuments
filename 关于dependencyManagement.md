---
title: dependencyManagement
date: 2020-08-10 15:32:00
tags:
- Maven
---

#### dependencyManagement

1.出现在父项目

2.指定好版本号，子项目无需写版本号

##### 父

```
<dependencyManagement>
    <dependencies>
        <dependency>
           <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>mysql.version</version>
        </dependency>
	</dependencies>
</dependencyManagement>
```

##### 子

```
<dependencyManagement>
    <dependencies>
        <dependency>
           <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
        </dependency>
	</dependencies>
</dependencyManagement>
```

#### PS:

**dependencyManagement里只是声明依赖，并不实现引入，需要子项目显示需要用的依赖并且没有具体指定版本号**

