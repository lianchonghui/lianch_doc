# Linux固定IP方法
---
**关键词**
```
Linux CentOS Ubuntu 固定IP 
```
---
**环境**
> - Ubuntu 16.04/CentOS 7
---
**注**
> **#** 在命令行前表示root用户下执行，如果非root用户则使用```sudo 命令```
---
**目录**
[TOC]
---

## 步骤
### 1. 查看网络设备状态

```# ifconfig```
 一般返回以下示例信息
```
    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.91  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 00:16:3e:0a:e8:da  txqueuelen 1000  (Ethernet)
        RX packets 47724  bytes 11873930 (11.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 37948  bytes 18622804 (17.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

lo是localhost设备，不用管。一般存在eth0或者ethn（n表示数字），centos7安装完是ens33。

### 2. 配置网络设备
- 进入配置文件夹下
    ```# cd /etc/sysconfig/network-scripts```
- 查看当前设备文件，一般会存在ifcfg-eth0类似的跟你设备对应的配置文件
    ```# ls```
- 编辑配置文件，如果不存在与你设备对应的配置文件则新建。以ifcfg-eth0为例
    ```# vim ifcfg-eth0```
添加或修改以下内容
```
    BOOTPROTO=static
    NAME=eth0
    DEVICE=eth0
    ONBOOT=yes
    DNS1=114.114.114.114
    DNS2=8.8.8.8
    IPADDR=192.168.1.200
    NETMAST=255.255.255.0
    GATEWAY=192.168.1.1
```
**配置含义**
```
    BOOTPRODTO=static/dhcp static表示ip固定,dhcp表示动态获取
    NAME和DEVICE 服务和设备名称，跟配置文件名保持一致
    ONBOOT=yes 表示开机启动该网络设备
    DNS1和DNS2 表示DNS服务器地址
    IPADDR 你需要指定的IP地址
    NETMAST 子网掩码，通常255.255.255.0
    GATEWAY 网关地址
```
### 3. 重启网络服务
```# service network restart```

### 4. 查看ip地址
```# ifconfig```

### 5. 检查网络状态
```# ping www.baidu.com```
能ping通表明配置正确

---
## 可能遇到的问题
### 1. ```ifconfig```命令不存在  
centos系统的最小化安装可能不存在```ifconfig```命令，可使用```ip addr```命令或者安装```net-tools```：```# yum install net-tools```则可使用```ifconfig```命令

--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 03月 18日 



[101]: https://www.lianch.com
