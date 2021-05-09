# 使用 Tomcat 运行 JSP

- [使用 Tomcat 运行 JSP](#使用-tomcat-运行-jsp)
  - [前言](#前言)
  - [1. 将 JSP 文件部署到 Tomcat 中](#1-将-jsp-文件部署到-tomcat-中)
  - [2. 启动 Tomcat](#2-启动-tomcat)
  - [3. 运行 JSP 文件](#3-运行-jsp-文件)

## 前言

最近实习的工作任务是修改一个项目的前端页面，拿到手后却发现是 JSP 文件，不能直接在浏览器上查看修改的效果，感觉十分反人类。。。

无奈之下只有使用 Tomcat 来运行查看效果，不过方法也非常简单。

## 1. 将 JSP 文件部署到 Tomcat 中

打开 Tomcat 根目录下的 webapps 文件夹，新建一个文件夹模拟 Web应用，将 JSP 文件放入这个文件夹。

比如我新建一个 `endRuz` 文件夹，放入一个叫 `index.jsp` 的 JSP 文件。

文件路径：`apache-tomcat-8.0.50\webapps\endRuz\index.jsp`

index.jsp 文件内容：

```jsp
 <%@ page contentType="text/html; charset=utf-8" language="java" %>
 <!DOCTYPE html>
 <html>
 <head>
     <title>index</title>
 </head>
 <body>
    <% out.println("welcome! endRuz"); %>
 </body>
 </html>
```

## 2. 启动 Tomcat

打开 Tomcat 根目录下的 bin 文件夹，运行 `startup.bat` 文件（若是 Linux 则运行 `startup.sh`）启动 Tomcat。
弹出窗口若显示`Server startup in xxx ms`，则说明 Tomcat 启动成功，不要关掉这个窗口。

## 3. 运行 JSP 文件

打开浏览器，在地址栏中输入 `http://localhost:8080/文件夹名/JSP文件名`，就可以看到 JSP 文件的运行结果了。

![tomcat jsp](https://user-images.githubusercontent.com/19424408/49743552-fd239000-fcd5-11e8-8de9-df337ab0e395.png)
