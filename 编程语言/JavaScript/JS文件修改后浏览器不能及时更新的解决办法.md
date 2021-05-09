# JS文件修改后浏览器不能及时更新的解决办法

- [JS文件修改后浏览器不能及时更新的解决办法](#js文件修改后浏览器不能及时更新的解决办法)
  - [Question](#question)
  - [Analysis](#analysis)
  - [Solution](#solution)

## Question

在 Web项目中修改了 JS文件之后，运行项目并没有变化。

F12 之后发现运行的仍然是之前的 JS文件。

## Analysis

在 F12 之后仔细观察会发现没有生效的 JS文件下有这样一段信息`(from disk cache)`。

原因是浏览器会自动缓存网站的 JS文件。

在开发 Web项目中，因为修改前的 JS文件被缓存，所以在修改 JS文件后，浏览器还是继续引用浏览器缓存的那个 JS文件。

## Solution

解决方案有如下三种：

1. 运行项目前清除浏览器缓存 Ctrl+F5
2. chrome 在 F12 下勾选`Disable cache`设置浏览器不缓存页面
3. 设置和改变版本号，浏览器就会重新下载新的 JS 或 CSS 文件

``` html
<link rel="stylesheet" type="text/css" href="test.css?v=1.1.1">
<script type="application/javascript" src="test.js?version=1.1.1"></script>
```
