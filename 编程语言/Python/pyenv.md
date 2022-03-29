# pyenv

- [pyenv](#pyenv)
  - [安装](#安装)
  - [卸载](#卸载)
  - [使用缓存安装 Python 版本](#使用缓存安装-python-版本)

## 安装

使用官方 Github 仓库中的安装脚本安装：

```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```

安装完毕后，在 `.bashrc` 中添加以下几行内容：

```bash
export PYENV_PATH="$HOME/.pyenv"
export PATH="$PYENV_PATH/bin:$PYENV_PATH/shims:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## 卸载

```bash
# 删除 ~/.pyenv
rm -fr ~/.pyenv
# 删除 .bashrc 中添加的内容
```

## 使用缓存安装 Python 版本

直接使用 `pyenv install 3.7.13` 安装时，下载源码包非常慢。

先去[官网](https://www.python.org/downloads/source/)下载对应版本的源码包，这里下载 Python-3.7.13.tar.xz 源码包。

然后创建 `~/.pyenv/cache` 目录，并把源码包放进该目录中。

最后执行 `pyenv install 3.7.13` 可直接安装。
