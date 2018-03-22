**关键词**
```
django ssl SECURE_SSL_REDIRECT https sslserver runsslserver
```
---
**演示环境**
> - python 3.5
> - django 1.10.6  

---
**注**
> 官方文档：[ssl-https-django][1]
> 本文django支持https通过sslserver插件实现，另外通过stunnel在开发环境中也可使用https,参考:[在django/flask开发服务器上使用HTTPS][2]

---
## django中的HTTPS
**HTTPS**在web应用中与web服务器有关，比如搭建nginx+django应用，通过反向代理https和http请求重定向到django的http请求上，https证书在web服务器上配置，与django应用无关。当反向代理也是走https请求时，django则需要通过插件使django可支持https。

## django中的SECURE_SSL_REDIRECT配置
在```settings.py```中添加```SECURE_SSL_REDIRECT = True```,默认下配置为```SECURE_SSL_REDIRECT = False```
### 1. 设置```SECURE_SSL_REDIRECT = True```
此时在浏览器发出http请求时django会重定向到https上。
以 ```$ python manage.py runserver```启动应用，发出http请求后django后台日志如下：
```
"GET / HTTP/1.1" 301 0
Self-signed SSL certificates are being blocked:Fix this by turning off 'SSL certificate verification' in Settings > General...
```
但此时web应用是不支持https的，报错如下
```
You're accessing the development server over HTTPS, but it only supports HTTP.
```
### 2. 设置```SECURE_SSL_REDIRECT = False```
此时http请求不会跳转到https,http此时django能正确访问。如果直接请求HTTPS时会报错如下：
```You're accessing the development server over HTTPS, but it only supports HTTP.```

## django的https支持：sslserver插件
#### 如果django需要HTTPS支持，可安装有sslserver插件:
```$ pip install django-sslserver```

#### 在settings.py中添加配置
```
INSTALLED_APPS = (...
"sslserver",
...
)
```
#### 自带证书启动django应用
```$ python manage.py runsslserver```
#### 指定证书启动django应用
```$ python manage.py runsslserver --certificate /path/to/certificate.crt --key /path/to/key.key```

当```SECURE_SSL_REDIRECT = False```时，http请求无响应，https请求能正确访问。
当```SECURE_SSL_REDIRECT = True```时，http请求会重定向https，此时django支持https，可正确访问。

## 可能遇到的问题
暂无

--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 03月 19日 


[1]: https://pypi.python.org/pypi/django-sslserver/0.12 
[2]: https://www.vpsee.com/2014/07/django-and-flask-development-server-with-https-support/
[101]: https://www.lianch.com
