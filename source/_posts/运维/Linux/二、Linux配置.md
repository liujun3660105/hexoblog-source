---
title: 二、Linux配置
toc: true
date: 2020-02-23 22:51:13
tags:
categories:
---
# 二、Linux配置

在用VMware 安装好centos后，win系统的电脑上的网络连接会自动多出来两个网卡，见下图。这两块网卡是VMware新建的两块虚拟代理网卡。

![1](1.png)

要进行两步配置，

## 1.网络配置

进入网络配置目录  

```bash
cd /etc/sysconfig/network-scripts
```

找到ifcfg-eth0这个文件(以太网第1块网卡配置)，修改这个文件的配置(记住更改这个配置只能是用个人电脑或者虚拟机，企业实体机千万不要这样配置，我们的目的是要根据这个系统克隆出许多个Linux系统，做集群用)。配置如下：

```bash
DEVICE=eth0
#HWADDR=00:0C:29:BF:9C:A7
UUID=62e3cd92-dcc9-45a7-841f-5de886d79c28
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.11.3 #ip地址
NETMASK=255.255.255.0 #子网掩码
GATEWAY=192.168.11.2 #网关
DNS1=114.114.114.114 #DNS地址
```

VM在虚拟系统中会维护两个东西，一个是虚拟主机，一个是虚拟网络。当从一台机器克隆出另外一台机器时，硬件地址(HWADDR)是不一样的。所以当克隆的时候，新机器的网络配置文件和老机器是一样高的，但是实际的mac地址已经发生变化（vm自动改变的），所以要把HWADDR注释掉。下面要配置虚拟机的ip地址。在vm中，选中新建的虚拟机，点击编辑-虚拟网络编辑器，进入网络设置界面。选择NAT模式，左下角有子网IP和子网掩码。表示VM给net维护的局域网的网络号。所以可以配置的就是192.168.11.0的最后一位0。0代表网络号，255是广播地址，不能配置给主机。点击NAT设置，网关ip是192.168.11.2。（一般家庭tplink路由器拿1做网关）。vm网络设置还有一个地方，将主机虚拟配适器连接到此网络。这个虚拟配适器就是刚才window新生成的网卡。那么这块网卡的ip地址是192.168.1.1。所以能分配给虚拟机的ip地址为192.168.11.2-192.168.11.254。

> :wq保存修改退出   :q! 不保存修改  退出

然后重启网络

```bash
service network restart
```

ping测试一下 看看能不能ping通，如果ping通，则说明虚拟机已经可以正常连接网络



## 2.关闭防火墙和安全监测模块

这台虚拟机是用来做模板机，那么模板机的配置，会遗传给克隆机。

### 2.1关闭防火墙

停止防火墙服务

```bash
service iptables stop
```

关闭开机自动启动

```bash
chkconfig iptables off
```

此时chkconfig 查看iptables服务是否开机自动关闭

### 2.2关闭安全监测模块

```bash
cd /etc/selinux
vi config
```

修改SELINUX=disabled

```bash
cd /etc/udev/rules.d/70-persistent-net.rules
```

这个文件对eth0和网卡mac地址绑定，现在要解绑（企业千万不要这么做），克隆机就会用新的mac地址绑定eth0，这么做是为了克隆。解绑的方式就是删除这个文件

```bash
rm -f 70-persistent-net.rules
```

然后 关机

```bash
poweroff
```









