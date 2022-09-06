---
title: "SPI组件"
date: 2021-09-06T13:43:18+08:00
categories: ["JavaBase"]
tags: ["Java SPI"]
---
# Java 服务提供者接口
- 什么是java Spi
- SPI组件构成
- API和SPI区别
- SPI实战演练


## 什么是java Spi
**SPI**: 用于发现和加载与给定**接口**匹配的**实现**。服务提供者接口:（供服务的提供者或者框架扩展者去实现服务的一套接口标准）

## SPI组件构成
### Service(服务)
一组众所周知的编程接口和类，它们提供对某些特定应用程序功能或特性的访问。
### Service Provider Interface（服务提供者接口）
为了实现特定应用功能的接入，制定的一套标准化的接口，被用于服务提供者去实现该接口，完成对功能的实现。
一般为具体的sdk服务厂家提供的接口标准。（jdk 为 oracle 驱动提供的接口）
### Service Provider （服务提供者（实现这个服务的具体模块或者类））
实现提供者接口的具体实现。通过配置 META-INF/services下的文件来确定服务器提供者接口和服务提供者实现。
### ServiceLoader
SPI 的核心是 ServiceLoader 类。具有延迟发现和加载实现的作用。
它使用上下文类路径来定位提供者实现并将它们放入内部缓存中即
通过读取服务提供者配置的META-INF/services下的文件信息获取对应实现类。
## API和SPI区别
- 当实现方提供了接口和实现，我们可以通过调用实现方的接口从而拥有实现方给我们提供的能力，这就是 API ，这种接口和实现都是放在实现方的。
- 当接口存在于调用方这边时，就是 SPI ，由接口调用方确定接口规则，然后由不同的厂商去根据这个规则对这个接口进行实现，从而提供服务。
## Java 生态系统中的 SPI 示例
- [CurrencyNameProvider](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/spi/CurrencyNameProvider.html):
  为 Currency 类提供本地化货币符号
- [LocaleNameProvider](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/spi/LocaleNameProvider.html):
  为 Locale 类提供本地化名称
- [TimeZoneNameProvider](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/spi/TimeZoneNameProvider.html): 
  为 TimeZone 类提供本地化的时区名称
- [DateFormatProvider](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/spi/DateFormatProvider.html):
  为指定的语言环境提供日期和时间格式。
- [NumberFormatProvider](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/text/spi/NumberFormatProvider.html):
  为 NumberFormat 类提供货币、整数和百分比值
- [Driver](https://docs.oracle.com/en/java/javase/11/docs/api/java.sql/java/sql/Driver.html):
  从 4.0 版开始，JDBC API 支持 SPI 模式。旧版本使用 Class.forName() 方法加载驱动程序。
- [PersistenceProvider](https://docs.oracle.com/javaee/7/api/javax/persistence/spi/PersistenceProvider.html):
  提供 JPA API 的实现。
- [JsonProvider](https://docs.oracle.com/javaee/7/api/javax/json/spi/JsonProvider.html):
  提供 JSON 处理对象
- [JsonbProvider](https://javaee.github.io/javaee-spec/javadocs/javax/json/bind/spi/JsonbProvider.html):
  提供 JSON 绑定对象。
- [Extension](https://docs.oracle.com/javaee/7/api/javax/enterprise/inject/spi/Extension.html):
  为 CDI 容器提供扩展。
- [ConfigSourceProvider](): 提供用于检索配置属性的源。
- 很多框架都使用了 Java 的 SPI 机制，比如：Spring 框架、数据库加载驱动、日志接口、以及 Dubbo 的扩展实现等等

## SPI实战演练
- 这里我们通过日志类型进行项目演示
- 我们首先创建一个名为service-provider-interface的 Maven 项目
- 然后我们创建一个Logger接口用于日志输出
```java
package com.ahut.spi;

/**
 * 日志接口定义相关日志信息输出
 * @author Cherubr
 * 通用服务用于日志输出
 */
public interface Logger {
    /**
     * 输出日志
     * @param message
     */
    void write(String message);
}
```
- 然后我们创建服务提供者用于制定创建那种日志输出
```java
package com.ahut.spi;

/**
 * @author Sumin.G
 * @title: Logger
 * @projectName java-base
 * @description: SPI 接口提供者 用于服务提供者去实现该接口，完成对功能的实现
 * @date 2022/9/69:40
 */
public interface LoggerProvider {
    /**
     * 创建日志管理类
     * @return
     */
    Logger create();
}
```
- 最后是我们的serviceloader类加载对应接口
```java
package com.ahut.spi.impl;

import com.ahut.spi.LoggerProvider;

import java.util.ArrayList;
import java.util.List;
import java.util.ServiceLoader;

/**
 * @author Sumin.G
 * @title: Logger
 * @projectName java-base
 * @description: TODO
 * @date 2022/9/69:40
 */
public class LoggerManager {

    private static final LoggerManager SERVICE = new LoggerManager();


    public static List<LoggerProvider> loggerList;

    private LoggerManager() {
        ServiceLoader<LoggerProvider> loader = ServiceLoader.load(LoggerProvider.class);
        List<LoggerProvider> list = new ArrayList<>();
        for (LoggerProvider log : loader) {
            list.add(log);
        }
        loggerList = list;
    }

    public static LoggerManager getService() {
        return SERVICE;
    }


}
```
到这里用户的服务提供者完成。
![SPI_serverProvider.png](../assert/SPI_serverProvider.png)
- 
- 下面我们创建服务提供者实现
创建一个名为service-provider的 Maven 项目，并将 service-provider-interface 依赖项添加到pom.xml
创建**SPI**实现类
用于构建对应Logger 日志具体实现类
```java
package com.ahut.spi.server;

import com.ahut.spi.Logger;
import com.ahut.spi.LoggerProvider;

/**
 * @author Sumin.G
 * @title: LogbackProvider
 * @projectName java-base
 * @description: TODO
 * @date 2022/9/617:16
 */
public class LogbackProvider implements LoggerProvider {
    @Override
    public Logger create() {
        return new Logback();
    }
}

```
- 然后是日志具体输出实现
```java
package com.ahut.spi.server;

import com.ahut.spi.Logger;

/**
 * @author Sumin.G
 * @title: Logback
 * @projectName java-base
 * @description: TODO
 * @date 2022/9/610:16
 */
public class Logback implements Logger {

    @Override
    public void write(String message) {
        System.out.println("logback输出日志 "+message);
    }
}
```
为了被发现，我们创建了一个提供者配置文件
META-INF/services/com.ahut.spi.LoggerProvider  -->文件内容为实现类名称 com.ahut.spi.server.LogbackProvider
![SpiProvider.png](../assert/SpiProvider.png)

- 新建APP类Main
```java
package com.ahut;

import com.ahut.spi.impl.LoggerManager;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {
        testSpi();
    }

    public static void testSpi(){
       LoggerManager.loggerList.stream().forEach(e->{
           e.create().write("123");
       });
    }
}

```
```bash
输出结果: logback输出日志 123
```