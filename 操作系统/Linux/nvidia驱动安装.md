# nvidia 驱动安装

- [nvidia 驱动安装](#nvidia-驱动安装)
  - [卸载旧版本](#卸载旧版本)
  - [禁用 Nouveau](#禁用-nouveau)
  - [在线安装](#在线安装)
  - [离线安装](#离线安装)
  - [验证](#验证)

## 卸载旧版本

```bash
# 清理旧驱动
sudo apt-get purge *nvidia*
sudo apt-get autoremove
# 若存在原有离线安装包则可执行以下命令
sudo sh NVIDIA-Linux-x86_64-xxx.xx.xx.run --uninstall
```

## 禁用 Nouveau

```bash
# 禁用系统自带显卡驱动，Nvidia 系列显卡通常对应 Nouveau (开源驱动)
# 查看是否存在驱动。如有返回信息，则存在驱动。
lsmod | grep nouveau

# 编辑文件，末尾添加：
# blacklist nouveau
# options nouveau modeset=0
sudo vim /etc/modprobe.d/blacklist.conf

# 重启服务器
sudo reboot
# 确认驱动是否还存在。没有返回信息，则说明成功。
lsmod | grep nouveau
```

## 在线安装

```bash
# 添加驱动 PPA
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update

# 通过官网 https://www.nvidia.com/Download/index.aspx 查询所需显卡驱动版本
# 或通过 ubuntu-drivers 命令查询推荐的驱动，但通常晚于官网提供的离线版驱动包版本
ubuntu-drivers devices

# 驱动版本号根据实际情况填写
sudo apt-get install nvidia-<VERSION>

# 重启电脑使驱动生效
reboot
```

> Tip: 删除驱动 PPA 的命令如下：
> ```bash
> sudo add-apt-repository -r ppa:graphics-drivers/ppa
> sudo apt-get update
> ```

## 离线安装

在离线服务器上查询显卡型号：

```bash
# 查询显卡型号
lspci | grep -i nvidia
```

在 [官方驱动下载页面](https://www.nvidia.com/Download/index.aspx)，输入对应的硬件信息和操作系统版本，下载离线安装包。

将下载的驱动离线包上传到离线服务器执行以下命令安装：

```bash
# 离线包添加执行权限
sudo chmod a+x NVIDIA-Linux-x86_64-xxx.xx.xx.run

# 安装离线包
# –no-opengl-files  只安装驱动文件，不安装 OpenGL 文件。这个参数最重要
# –no-x-check       安装驱动时不检查 X 服务
# –no-nouveau-check 安装驱动时不检查 nouveau
sudo sh NVIDIA-Linux-x86_64-xxx.xx.xx.run \
--no-opengl-files \
-no-x-check \
-no-nouveau-check

# 重启电脑使驱动生效
reboot
```

如果在安装的过程中出现以下信息，请选择：

1. The distribution-provided pre-install script failed! Are you sure you want to continue?

   选择 yes 继续。

2. Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later?

    选择 No 继续。

3. Nvidia’s 32-bit compatibility libraries?

    选择 No 继续。

4. Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up.

    选择 No 继续

## 验证

命令 `nvidia-smi` 成功输出显卡信息
