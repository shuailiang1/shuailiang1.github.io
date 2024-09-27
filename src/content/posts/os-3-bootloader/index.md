---
title: os64：bootloader
published: 2024-06-19
description: "对应书上第三章内容"
tags: ["os","bootloader"]
category: os
draft: false
---

- boot: 开机启动、加载loader
- loader: 配置硬件工作环境、引导加载内核

# 设置x服务转发
&emsp;&emsp;bios检查硬盘、软盘等设备，若第0磁头0磁道1扇区以0x55、0xaa结尾，则将该扇区认为是引导扇区，将其内容复制到内存0x7c00处，将控制权交给这段程序。（引导程序大小为510B）<br>
&emsp;&emsp;南京大学镜像源下载的bochs2.7虚拟机有问题，开启时提示无法加载x服务。于是去下载了源码自己编译安装。现在目测冇问题。<br>
&emsp;&emsp;又遇到了bochs虚拟机开启过程中找不到x服务器的问题，但是又只有vscode有这个问题，mobaxterm可以非常丝滑地打开bochs通过x渲染出来地画面，debian虚拟机内部自然也可以丝滑地打开。检查vscode的ssh设置，发现x forward是开启的。经过一番stackoverflow+chatgpt大法后发现可能是虚拟机内部ssh服务端没有默认开启x转发。先添加环境变量
```shell
export DISPLAY=localhost:10.0
```
&emsp;&emsp;SSH服务器会为这个会话创建一个虚拟的X11服务器，并分配一个显示编号（通常从 10 开始）。这避免了与本地 X11 服务器（通常是 :0）的冲突。发现还是寄。再在虚拟机内开启ssh x转发，即/etc/ssh/sshd_config内取消注释
```shell
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost yes
```
&emsp;&emsp;再次设置DISPLAY环境变量后成功。为什么mobaxterm就可以成功转发呢？在mobaxterm中打开终端
```shell
echo $DISPLAY
```
&emsp;&emsp;发现已经设置好了DISPLAY环境变量，其值为:10.0，和在vscode里面设置的一模一样。为什么mobaxterm一直可以转发就很神奇了。
&emsp;&emsp;参悟了。在windows中查看x有关进程，发现只有mobax，vscode没有x显示服务。也就是说只有mobaxterm连上虚拟机了vscode才能正常显示x窗口（用的是mobaxterm的显示服务），昨天居然没有发现。
# boot
&emsp;&emsp;bios显示程序部分就先跳过了，看解释能看懂，记不住。
# loader
&emsp;&emsp;在boot.bin中写入了FAT12系统的元数据，将boot.img挂载上能够直接从文件系统写入loader。<br>
- loader功能：检测硬件、处理器模式切换、向内核传递数据。
&emsp;&emsp;x86机开机时处理器运行在实模式，地址空间只有20位，只能用寄存器16位，无内存保护。随后进入保护模式（32位）->长模式（64位）。