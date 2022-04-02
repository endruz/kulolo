# nvidia-smi 报错：Failed to initialize NVML: Driver/library version mismatch

- [nvidia-smi 报错：Failed to initialize NVML: Driver/library version mismatch](#nvidia-smi-报错failed-to-initialize-nvml-driverlibrary-version-mismatch)
  - [1. 问题](#1-问题)
  - [2. 分析](#2-分析)
  - [3. 原因](#3-原因)
  - [4. 解决方案](#4-解决方案)
    - [方案一：关闭自动更新](#方案一关闭自动更新)
    - [方案二：更新后自动重启](#方案二更新后自动重启)


## 1. 问题

之前使用正常的 Nvidia 驱动，在某个时间段后执行 `nvidia-smi` 指令报错如下：

```bash
$ nvidia-smi
Failed to initialize NVML: Driver/library version mismatch
```

## 2. 分析

通过 `last` 和 `history` 指令并未发现对 Nvidia 驱动做过任何操作。

使用以下命令查看当前驱动版本为 **470.82**。

```bash
$ cat /proc/driver/nvidia/version
NVRM version: NVIDIA UNIX x86_64 Kernel Module  470.82.00  Thu Oct 14 10:24:40 UTC 2021
GCC version:  gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)
```

尝试重启服务器后发现 `nvidia-smi` 指令可以正常执行，此时再查看驱动版本为 **470.86**。

```bash
$ cat /proc/driver/nvidia/version
NVRM version: NVIDIA UNIX x86_64 Kernel Module  470.86  Tue Oct 26 21:55:45 UTC 2021
GCC version:  gcc versiodan 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)
```

这时基本可以确定是更新 Nvidia 驱动后未重启生效导致的报错。

## 3. 原因

Ubuntu 的 `unattended-upgrades` 自动更新了 Nvidia 驱动后未重启生效。

## 4. 解决方案

### 方案一：关闭自动更新

同时更新 `/etc/apt/apt.conf.d/10periodic` 和 `/etc/apt/apt.conf.d/20auto-upgrades`：

```diff
- APT::Periodic::Update-Package-Lists "1";
- APT::Periodic::Unattended-Upgrade "1";
+ APT::Periodic::Update-Package-Lists "0";
+ APT::Periodic::Unattended-Upgrade "0";
```

### 方案二：更新后自动重启

有些软件更新需要重启系统，而 `unattended-upgrades` 默认的配置是不重启系统的。

更新 `/etc/apt/apt.conf.d/50unattended-upgrades` 下面的配置允许重启系统（即更新完成后，如果需要重启，立即重启系统）：

```diff
// Automatically reboot *WITHOUT CONFIRMATION* if
//  the file /var/run/reboot-required is found after the upgrade
- //Unattended-Upgrade::Automatic-Reboot "false";
+ Unattended-Upgrade::Automatic-Reboot "true"
```
