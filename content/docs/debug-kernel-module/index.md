---
title: 'Debug Kernel Module'
date: 2023-01-12T10:28:11+08:00
draft: true
---

# 编译内核

## 安装便于所需要的软件包

```apt
sudo apt-get install build-essential linux-source bc kmod cpio flex libncurses5-dev libelf-dev libssl-dev dwarves bison
```

其中：

- build-essential - Essential packages required for compiling.
- linux-source - The Linux Kernel Source
- libncurses5-dev - Development files for ncurses5. Optional for using curses based menu driven configuration.

## 获取源码

可以直接通过 `sudo apt install linux-source-xxxx` 来获取内核源码，内核源码默认存放在 `/usr/src/` 内。

由于 `/usr/src` 文件夹内需要root权限，一般建议将下载的gz包解压到 `~`路径下，方便权限管理。

`tar xavf /usr/src/linux-source-4.15.tar.xz`

## 修改.config文件

首先复制当前config，在已有config的基础上修改。

`cp /boot/config-*** ./.config`

之后`make menuconfig`，勾选KDB所必须的项目。

![](C:\Users\78102\yF\yBlog\content\docs\debug-kernel-module\images\menuconfig.jpg)

之后，根据[kernel doc](https://www.kernel.org/doc/html/v5.0/dev-tools/kgdb.html)给出的建议，手动修改config文件并添加如下字段：

```config
# CONFIG_STRICT_KERNEL_RWX is not set
CONFIG_FRAME_POINTER=y
CONFIG_KGDB=y
CONFIG_KGDB_SERIAL_CONSOLE=y
CONFIG_KGDB_KDB=y
CONFIG_KDB_KEYBOARD=y
```

同时，为了避免证书错误导致编译中断，手动修改config文件，将对应项置空即可：

```config
CONFIG_SYSTEM_TRUSTED_KEYS=""
```

## 编译

直接执行`make deb-pkg`，将会在当前路径的上级路径生成数个deb包。

![](C:\Users\78102\yF\yBlog\content\docs\debug-kernel-module\images\生成的deb文件.jpg)

## 安装

通过`make deb-pkg` 所生成的包与平常的编译源码生成的包不同，安装方式自然也不同。

## 调试




