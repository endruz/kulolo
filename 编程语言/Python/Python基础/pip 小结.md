# pip 小结

- [pip 小结](#pip-小结)
  - [安装 pip](#安装-pip)
  - [在线安装 pip](#在线安装-pip)
    - [离线安装 pip](#离线安装-pip)
    - [步骤一：安装 setuptools](#步骤一安装-setuptools)
      - [步骤二：安装 pip](#步骤二安装-pip)
  - [pip 安装第三方包](#pip-安装第三方包)
    - [pip 在线安装第三方包](#pip-在线安装第三方包)
    - [pip 离线安装第三方包](#pip-离线安装第三方包)

## 安装 pip

## 在线安装 pip

```bash
sudo apt install python-pip
```

### 离线安装 pip

### 步骤一：安装 setuptools

1. 在 [PyPi](https://pypi.org/ "PyPi网址") 下载 [setuptools 源码](https://pypi.org/project/setuptools/#files "下载链接")。
2. 使用 Python 安装 setuptools，命令如下。

```bash
unzip setuptools-41.0.1.zip
cd setuptools-41.0.1
python setup.py install
```

#### 步骤二：安装 pip

1. 在 [PyPi](https://pypi.org/ "PyPi网址") 下载 [pip 源码](https://pypi.org/project/pip/#files "下载链接")。
2. 使用 Python 安装 pip，命令如下。

```bash
tar -zxvf pip-19.1.1.tar.gz
cd pip-19.1.1
python setup.py install
```

通过 `pip -V` 查看当前 pip 版本。

## pip 安装第三方包

### pip 在线安装第三方包

```bash
pip install ***
```

### pip 离线安装第三方包

1. 在 [PyPi](https://pypi.org/ "PyPi网址") 下载第三方包的 wheel 文件。
2. 使用 pip 命令 `pip install ***.whl` 安装第三方库。

通过 `pip list` 查看第三方包是否安装成功。
