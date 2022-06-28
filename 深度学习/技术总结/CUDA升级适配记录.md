# CUDA 升级适配记录

- [CUDA 升级适配记录](#cuda-升级适配记录)
  - [背景](#背景)
  - [适配 tensorflow 1.15](#适配-tensorflow-115)
  - [适配 darknet](#适配-darknet)

## 背景

由于 NVIDIA A100 显卡不兼容 CUDA 10，故以前的项目需要升级至 CUDA 11 运行。

## 适配 tensorflow 1.15

tensorflow 已经全面更新到 tensorflow 2+，官方版本的 tensorflow 1.15 只支持到 CUDA 10。

要想在新版的 CUDA 11 上使用 tensorflow 1.15，可以尝试 NVIDIA 官方维护的 tensorflow 版本 [nvidia-tensorflow](https://github.com/NVIDIA/tensorflow)。

安装要求：
- Ubuntu 20.04 或更高版本（64 位）
- 支持 CUDA® 的 GPU
- 安装 r455 的 NVIDIA GPU 驱动程序

车轮安装：

- Python 3.8
- pip 19.0 或更高版本

安装命令如下：

```bash
# nvidia-tensorflow 没有收录在 PyPI.org 中，需要安装 NVIDIA wheel index
pip install --user nvidia-pyindex
pip install --user nvidia-tensorflow[horovod]
```

`nvidia-tensorflow` 包括对 Linux 的 CPU 和 GPU 支持。

## 适配 darknet

[darknet](https://github.com/pjreddie/darknet) 依赖于 CUDNN 7，但是没有与 CUDA11 对应的 CUDNN 7。

在 CUDA 11 + CUDNN 8 的环境下编译 darknet，会报错：

```bash
./src/convolutional_layer.c:153:13: error: 'CUDNN_CONVOLUTION_FWD_SPECIFY_WORKSPACE_LIMIT' undeclared (first use in this function);

       did you mean 'CUDNN_CONVOLUTION_FWD_ALGO_DIRECT'?
```

后在 NVIDIA 论坛中的一个 [帖子](https://forums.developer.nvidia.com/t/darknet-compile-error-with-cudnn-8/141115) 中找到解决方案。修改文件 `src/convolutional_layer.c`，增加针对 `CUDNN_MAJOR>=8` 的处理逻辑。

合入补丁操作如下：

```bash
git clone https://github.com/pjreddie/darknet.git
cd darknet/
wget https://raw.githubusercontent.com/AastaNV/JEP/master/script/topics/0001-fix-for-cudnn_v8-limited-memory-to-default-darknet-s.patch
git am 0001-fix-for-cudnn_v8-limited-memory-to-default-darknet-s.patch
```

之后根据设备信息修改 `Makefile` 重新编译即可。
