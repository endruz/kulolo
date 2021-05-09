# Linux 设置应用程序全局可用及创建桌面图标

- [Linux 设置应用程序全局可用及创建桌面图标](#linux-设置应用程序全局可用及创建桌面图标)
  - [Linux 设置应用程序全局可用](#linux-设置应用程序全局可用)
  - [创建桌面图标](#创建桌面图标)

## Linux 设置应用程序全局可用

在 /usr/bin 下创建应用程序可执行文件的软连接即可。

以安装 Postman 为例。

```bash
# 解压 Postman 压缩包
tar -xvf Postman-linux-x64-7.5.0.tar.gz
# 在 /usr/bin 目录下建立软连接
sudo ln -s /home/endruz/Postman/Postman /usr/bin/postman
# 这时你就可以在任何路径使用 postman 命令来启动 Postman 了
postman
```

## 创建桌面图标

创建 `postman.desktop` 文件，写入以下内容。

```ini
# 文件头 [必填]
[Desktop Entry]
# 编码方式
Encoding=UTF-8
# 应用程序名称，根据当前系统语言匹配显示，优先匹配更细化的语言标识名称 [必填]
Name=Postman
Name[en]=Postman
Name[en_US]=Postman

# 鼠标经过图标时的提示
Comment=comment

# 图标使用的图片，可以为空
Icon=/home/endruz/Postman/app/resources/app/assets/icon.png

# 分类 [必填]
# "Type = Link" 表示图标指向了一个 url
Type=Application

# 只有在 "Type = Application" 时才有意义
# 定义图标执行的命令或程序
Exec=/home/endruz/Postman/Postman

# 是否使用终端
Terminal=false

# 图标所属类别
Categories=Development;

# 只有在 "Type = Link" 时才有意义
# 定义指向的 url
# URL = https://www.baidu.com/
```

注意：`postman.desktop` 文件不能以空行结尾，否则使用时会报 "启动应用程序出错" 错误。

将 `postman.desktop` 放至 `/usr/share/applications` 就可以在应用菜单中找到 Postman
