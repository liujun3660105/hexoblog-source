---
title: 一、Linux安装
toc: true
date: 2020-02-23 22:46:30
tags: Linux
categories: 
- 运维
- Linux
---

# 一、Linux安装

## 1.虚拟机VMware workStation的安装

下载地址：https://www.7down.com/soft/310739.html

安装后的界面

![1.MVware](1.MVware.png)

## 2.Linux安装

### 2.1新建虚拟机

文件--》新建虚拟机，基本都是一路默认，但是有几个设置要注意一下

1.第一个配置项 要选择自定义高级

2.安装客户操作系统，选择稍后安装操作系统

3.选择用户操作系统 为centos 64位

4.网络类型，选择使用网络地址转换(MAT)

5.指定磁盘容量,这里调整容量大一些，建议200G。这里并不是直接开辟出200G的磁盘空间，而是虚拟机装的东西最大可以装200G。

最后安装后的界面是

![2](2.png)

### 2.2安装centos

在设备中，点击CD/DVD，在连接中选择使用IOS镜像文件，这里我下载的是centos 6.5 minimal镜像文件，是非常精简的系统。然后开启此虚拟机，开始安装。

弹出的安装界面为

![3](3.png)

第一步，当鼠标点击进去centos系统后，鼠标和键盘无法在自己电脑上使用，此时按住ctrl+alt，鼠标和键盘的控制权就会回到自己的电脑上。选择第一个Install or upgrade an existing system

第二步，检查磁盘介质直接跳过，因为不是物理光盘，而是ios镜像文件。

第三步，开启图形界面，点击next

第四步，语言和键盘都选择English

第五步，设备类型选择 Basic Storage Device

第六步，yes，discard any data

第七步，设置时区为东八区

第八步，设置密码

第九步，选择自动创建分区，create Custom Layout

第十步，点击create，创建分区，选择standard Partition。这里我们分了三个区，第一个引导分区，MountPoint为/boot，类型为ext4。第二个分区是磁盘和内存交互的分区，没有挂载点，用户是看不到的，类型为swap。最后一个是用户理解上的分区，就是安装操作系统和各种软件用的。挂载点为/ （根目录），类型为ext4。大小勾选最后一个，选择剩余空间。分区后的图为：

![4](4.png)

第十一步，同意格式化（Format），写入磁盘（write changes to disk）

后面就是默认了，然后自动进入安装。安装后点击Reboot，重启。此时Linux系统已经安装完毕。