---
title: "DevEnviroment"
date: 2019-12-24T20:47:52+08:00
categories: ["Java Tools"]
tags: ["jdk，maven 环境配置", "maven仓库配置"]
#draft: true
---
# 常见的java开发环境配置
- jdk环境配置
- maven环境配置


## jdk环境配置
- 配置 JAVA_HOME  
右键电脑找到属性-->系统属性-->环境变量-->系统变量新建JAVA_HOME: C:\Program Files\Java\jdk1.8.0_231(如图)  
![java_home](../assert/java_home.png)
- 配置CLASS_PATH  
新建变量CLASS_PATH: .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
![java_classpath](../assert/java_classpath.png)
- 配置path  
编辑path变量: %JAVA_HOME%\bin&emsp;&emsp;%JAVA_HOME%\jre\bin(window10)  
![java_path](../assert/java_path.png)
## maven环境变量配置
- 右键电脑找到属性-->系统属性-->环境变量-->系统变量新建MAVEN_HOME: D:\javatools\apache-maven-3.6.3(如图)  
![maven_home](../assert/maven_home.png)
- 配置path:  
编辑path变量: %MAVEN_HOME%\bin(window10)  
![maven_path](../assert/maven_path.png)
- maven settings.xml 仓库配置（按需）添加到``` <mirrors> </mirrors>```节点之间
```xml
     <!-- 阿里云仓库-->
        <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>

        <!-- 中央仓库1 -->
        <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo1.maven.org/maven2/</url>
        </mirror>
    
        <!-- 中央仓库2 -->
         <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
        </mirror> 
        <!-- 阿里云的maven路径 -->
          <mirror>
            <id>nexus-aliyun</id>
            <mirrorOf>*</mirrorOf>
            <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
          </mirror>
```