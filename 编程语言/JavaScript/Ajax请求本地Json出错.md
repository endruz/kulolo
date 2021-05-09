# Ajax请求本地Json出错

- [Ajax请求本地Json出错](#ajax请求本地json出错)
  - [Question](#question)
  - [Analysis](#analysis)
  - [Solution](#solution)
    - [配置 Chrome 支持本地 file 协议的 ajax 请求](#配置-chrome-支持本地-file-协议的-ajax-请求)
    - [使用本地 Web Server 访问](#使用本地-web-server-访问)
    - [使用跨域请求支持 file 协议的浏览器](#使用跨域请求支持-file-协议的浏览器)

## Question

使用 JQuery Ajax 加载不了本地 Json 文件。
打开 DevTools(F12) 发现 Console 中报错：

```log
jquery-3.3.1.js:9600 Access to XMLHttpRequest at 'file:///C:/Users/endru/GitRep/JavaScriptUtils/DataTables/data.json?_=1542954193429' from origin 'null' has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.
```

## Analysis

报错的大概意思是：
跨源请求支持的协议有：`http,data,chrome,chrome-extension,https`。
因为不支持`file`协议，所以我的跨域请求被 CORS(跨域资源共享) 政策拒绝了。

## Solution

有以下三种思路来解决这个问题：

### 配置 Chrome 支持本地 file 协议的 ajax 请求

这种思路有两种方法：

1. 利用 CMD 解决。

    关闭所有 Chrome 窗口后，打开 CMD cd 到 chrome.exe 所在文件夹。

    然后运行`chrome.exe --allow-file-access-from-files`，然后在被打开的 Chrome 窗口内访问。
2. 使用 Windows 快捷方式。

    Windows 右击 Chrome 的快捷方式 `属性 - 快捷方式 - 目标`，将目标内容后添加`--allow-file-access-from-files`。

    注意添加的内容和原有内容要用空格分割，例如：`"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --allow-file-access-from-files`。

    点击确定后，关闭所有 Chrome 窗口，使用快捷方式打开的 Chrome 窗口进行访问。

### 使用本地 Web Server 访问

因为我使用 VScode 推荐大家使用 Live Server 插件。

运行本地 Web Server 之后进行访问 ，本地 Json 地址变为 `http://127.0.0.1:5500/DataTables/data.json?_=1542964505300`变成 http 协议，且与请求页面同源。

### 使用跨域请求支持 file 协议的浏览器

比如，我们神奇的 Firefox。。。

> PS:
>
> 不知道有没有小伙伴有过被流氓软件修改浏览器主页，但是在浏览器里设置主页后并不起作用的经历。
>
> 这个时候可以查看一下浏览器快捷方式的 属性 - 快捷方式 - 目标，目标内容后有没有跟着网址，如果有删掉即可。
>
> 快捷方式的“目标” 是要调用执行的程序。如果后面跟着网址的话，就是打开浏览器并打开相应网址的意思。
