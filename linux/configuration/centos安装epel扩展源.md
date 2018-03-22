# CentOS安装epel扩展源 
---
**关键词**
```
epel centos找不到安装包
```
---
**环境**
> - Centos 7 

---
**注**
> **#** 表示在root用户下执行，如果非root用户，执行sudo命令 
> 本文采用yum安装，rpm包安装参考：[如何在centos安装epel源][1]
---

## 关于epel源
EPEL (Extra Packages for Enterprise Linux，企业版Linux的额外软件包) 是Fedora小组维护的一个软件仓库项目，为RHEL/CentOS提供他们默认不提供的软件包。这个源兼容RHEL及像CentOS和Scientific Linux这样的衍生版本。

## 安装epel源
```# yum -y install epel-release``` 

---
## 可能遇到的问题
### 1. centos6.7以下版本安装出现以下问题：

```Could not get metalink https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=x86_64 error was 14: Peer cert cannot be verified or peer cert invalid```

**解决方法：**
编辑epel.repo文件```# vim /etc/yum.repos.d/epel.repo```,**修改配置文件下的https为http**

--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 03月 18日 


[1]: https://www.cnblogs.com/chenliyang/p/6633756.html
[101]: https://www.lianch.com