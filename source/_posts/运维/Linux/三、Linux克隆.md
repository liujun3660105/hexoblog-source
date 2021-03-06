---
title: 三、Linux克隆
toc: true
date: 2020-02-23 22:53:01
tags:
categories:
---
# 三、Linux克隆

之前的模板机配置完毕后，为了要做集群，克隆几台一模一样的机器。

右键点击模板机->快照->拍摄快照，然后拍摄快照。之后点击vm上面的小扳手，如图所示

![1](1.png)

进入克隆虚拟机向导

现有快照-》创建链接克隆-》填写虚拟机名字和路径。此时就得到一个新的虚拟机。

为了做集群，可以多克隆几个克隆机。此时可以查看，模板机的mac地址和克隆机的mac地址是一样的。正常情况下不应该一样。此时我们开机克隆机，开机后，在查看克隆机的mac地址，发现和模板机不一样了。克隆机的用户名密码和模板机是一模一样的。

下面我们要对克隆机进行一些配置

就想我们上网输入域名而不是ip地址，我们也想让我们的克隆机相互之间访问是通过名称而不是通过ip地址。所以要做名称和ip地址之间的映射。

登录克隆机

### 1.设置独立ip地址

```bash
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

设置IPADDR=192.168.11.3。然后重启service network restart，然后ping测试是否通讯。

```bash
ifconfig
```

这个命令可以查看这台机器的ip地址。

### 2.设置主机名

```bash
vi /etc/sysconfig/network
```

这是HOSTNAME=node0001，原先的主机名是模板机的主机名。然后**重启克隆机**。

### 3.设置主机名和ip地址之间的映射，编辑hosts文件 在后面直接添加映射关系

```bash
vi /etc/hosts
```

添加映射关系

```bash
192.168.11.3 node0001

192.168.11.4 node0002

192.168.11.5 node0003

192.168.11.6 node0004
```

然后power off

此时拍个快照，目的就是为了后期方便克隆。如果一台机子有很多个快照，点击你想要的快照，然后点击转到，此时克隆机就是当前快照的状态。



当配置完这四台克隆机后，测试这四台机器的连接性，比如在node0001上，可以ping node0002（或者ip地址），看是否可以ping通。

最后测试windows电脑上是否可以ping通克隆机，如果需要ping克隆机的主机名称，则在window上也需要配置。具体方法如下：

c：/window/system32/drivers/etc/中的hosts文件  在最后添加映射克隆机的映射关系。然后保存。



