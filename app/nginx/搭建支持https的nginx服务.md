**关键词**
```
nginx https
```
---
**环境**
> - Ubuntu 16.04/CentOS 7

---
**注**
- **#** 表示以root用户身份执行命令 ，**$** 表示以普通用户执行命令

- nginx安装官方文档：[nginx: Linux packages][2]

---

## 安装nginx并启动服务
### 安装nginx
#### CentOS下安装nginx:
在/etc/yum.repos.d/目录下创建一个源配置文件nginx.repo：
```# vim /etc/yum.repos.d/nginx.repo```
填写如下内容并保存：
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/6/$basearch/
gpgcheck=0
enabled=1
```
baseurl的6表示centos6版本，如果centos7则将6替换7。
执行yum命令安装。
```# yum -y install nginx```

#### Ubuntu下安装nginx:
```$ sudo apt-get install nginx```

### 启动nginx.service
启动nginx服务
```$ sudo service nginx start```或者```$ sudo systemctl start nginx```

如果需要设置开机启动nginx服务执行命令：
```$ sudo systemctl enable nginx```,centos下还可使用```$ sudo chkconfig nginx on```
禁用nginx服务开机自启：
```$ sudo systemctl disable nginx```,centos下还可使用```$ sudo chkconfig nginx off```

### 检查服务是否正常
#### 本地检查服务状态
通过浏览器请求```http://localhost```或者使用命令```$ curl http://localhost```,如果请求返回nginx的页面则服务正常。

#### 外部请求服务状态
如果外部请求服务，则需要注意防火墙配置开放80端口，如果支持https请求还需要开放443端口。或者直接关闭防火墙。对centos还需注意selinux的配置。
- CentOS6.7以下版本开通指定端口
```
    方法一：
    /sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
    /etc/init.d/iptables save
    service iptables restart
    方法二：
    vi /etc/sysconfig/iptables
    添加以下内容
    -A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT

    或者直接关闭防火墙：
    $ sudo chkconfig –level 35 iptables off

    注：
    1.35表示命令号下（3），窗口界面下（5）关闭防火墙；
    2.开启防火墙off替换为on；
    3.禁用端口ACCEPT改为DROP执行即可。
```
- CentOS7以上版本开通指定端口
```
    开启80端口
    # firewall-cmd --zone=public --add-port=80/tcp --permanent

    重启防火墙 
    # firewall-cmd --reload
    或者重启防火墙服务
    # service firewalld restart

    命令含义：
    --zone #作用域
    --add-port=80/tcp  #添加端口，格式为：端口/通讯协议
    --permanent   #永久生效，没有此参数重启后失效

```
- Ubuntu下开通指定端口
```
    开通80端口
    $ sudo ufw allow 80

    或者关闭防火墙
    $ sudo ufw disable

    注：ubuntu安装ufw并启用方式如下
    $ sudo apt-get install ufw 
    $ sudo ufw enable 
    $ sudo ufw default deny 
```
通过```$ ifconfig```获取主机IP,外部机器通过浏览器请求```http:主机IP```检查服务状态。

## 生成证书
### 通过openssl生成证书
如果没有安装openssl，centos下通过```# yum install openssl```,Ubuntu下执行```$ sudo apt-get install openssl```
#### 1.首先，进入你想创建证书和私钥的目录，例如：
```cd /etc/nginx/```

#### 2.创建服务器私钥，命令会让你输入一个口令：
```openssl genrsa -des3 -out server.key 1024```

#### 3.创建签名请求的证书（CSR）：
```openssl req -new -key server.key -out server.csr```

#### 4.在加载SSL支持的Nginx并使用上述私钥时除去必须的口令：
```cp server.key server.key.org```
```openssl rsa -in server.key.org -out server.key```
#### 5.最后标记证书使用上述私钥和CSR：
```openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt```

### 在阿里云申请免费的证书 
阿里云申请免费证书是浏览器承认的。文档参考：[申请免费SSL证书 - 阿里云云盾证书 - Digicert+Symantec 免费型DV SSL][1]

## 配置nginx
配置内容示例如下：
```
server {
    listen       443;
    server_name  本机的IP地址;
    ssl                  on;
    ssl_certificate      /etc/nginx/server.crt;
    ssl_certificate_key  /etc/nginx/server.key;
    ssl_session_timeout  5m;

    location / {
        #root   html;
        #index  testssl.html index.html index.htm;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://IP地址/ssl/;
    }
}
```

---
## 可能遇到的问题
暂无


--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 07月 07日 


[1]: https://bbs.aliyun.com/read/573933.html?spm=5176.10695662.1996646101.searchclickresult.208c3bdcPo8g6s
[2]: http://nginx.org/en/linux_packages.html#stable
[101]: https://www.lianch.com
