# Linux 搭建 shadowsocks 服务器

- [Linux 搭建 shadowsocks 服务器](#linux-搭建-shadowsocks-服务器)
  - [安装 shadowsocks 服务器](#安装-shadowsocks-服务器)
    - [安装 pip](#安装-pip)
    - [安装 shadowsocks](#安装-shadowsocks)
  - [帮助命令](#帮助命令)
  - [启动 shadowsocks 服务](#启动-shadowsocks-服务)
    - [配置文件启动服务](#配置文件启动服务)
    - [通过命令启动](#通过命令启动)
  - [重启 shadowsocks 服务](#重启-shadowsocks-服务)
  - [关闭 shadowsocks 服务](#关闭-shadowsocks-服务)
  - [shadowsocks 日志](#shadowsocks-日志)

## 安装 shadowsocks 服务器

先安装 pip，再通过 pip 安装 shadowsocks 服务器。

### 安装 pip

```bash
# 方法一  下载安装脚本并运行
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python get-pip.py
# 方法二 Linux 包管理器安装
# Ubuntu Debian
sudo apt-get install python-pip
# CentOS
yum install python-setuptools
easy_install pip
```

### 安装 shadowsocks

```bash
pip install shadowsocks
```

## 帮助命令

```bash
ssserver -h
usage: ssserver [OPTION]...
A fast tunnel proxy that helps you bypass firewalls.

You can supply configurations via either config file or command line arguments.

Proxy options:
  -c CONFIG              path to config file
  -s SERVER_ADDR         server address, default: 0.0.0.0
  -p SERVER_PORT         server port, default: 8388
  -k PASSWORD            password
  -m METHOD              encryption method, default: aes-256-cfb
  -t TIMEOUT             timeout in seconds, default: 300
  --fast-open            use TCP_FASTOPEN, requires Linux 3.7+
  --workers WORKERS      number of workers, available on Unix/Linux
  --forbidden-ip IPLIST  comma seperated IP list forbidden to connect
  --manager-address ADDR optional server manager UDP address, see wiki

General options:
  -h, --help             show this help message and exit
  -d start/stop/restart  daemon mode
  --pid-file PID_FILE    pid file for daemon mode
  --log-file LOG_FILE    log file for daemon mode
  --user USER            username to run as
  -v, -vv                verbose mode
  -q, -qq                quiet mode, only show warnings/errors
  --version              show version information

Online help: <https://github.com/shadowsocks/shadowsocks>
```

## 启动 shadowsocks 服务

通过查询帮助可得知，我们有两种方法启动服务：

1. 通过配置文件启动
2. 通过命令启动

### 配置文件启动服务

创建 shadowsocks 配置文件

```bash
vim /etc/shadowsocks.json
```

内容大致如下：(使用的时候请删除注释文字，否则可能会报错！！)

```json
{
    "server": "0.0.0.0",        // 服务器ip地址
    "server_port": 443,         // 服务器端口
    "password": "******",       // ss密码
    "method": "aes-256-cfb",    // 加密方法
    "timeout": 300,             // 超时时间
    "fast_open": false,         // Linux 内核3.7+时，可以降低延迟
    "workers": 1                // 默认为1
}
```

如果需配置多个账号，可以按端口对应密码的方式进行配置：

```json
{
    "server": "your_server_ip",
    "port_password": {
        "441":"password1",
        "442":"password2",
        "443":"password3"
        },
    "timeout": 300,
    "method": "rc4-md5",
    "fast_open": false,
    "workers": 1
}
```

写好配置文件后，使用如下命令启动 shadowsocks 服务器。

```bash
ssserver -c /etc/shadowsocks.json -d start
```

### 通过命令启动

```bash
ssserver -s SERVER_ADDR -p SERVER_PORT -k PASSWORD -m METHOD -t TIMEOUT -d start
```

由帮助可知上面命令的大多选项都有默认值，若不需要特殊配置，则可以简化成

```bash
ssserver -k PASSWORD -d start
```

## 重启 shadowsocks 服务

```bash
ssserver -k PASSWORD -d restart
```

## 关闭 shadowsocks 服务

```bash
ssserver -d stop
```

## shadowsocks 日志

shadowsocks 的日志保存在 `/var/log/shadowsocks.log`
