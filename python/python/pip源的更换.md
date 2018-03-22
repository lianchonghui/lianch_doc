**关键词**
```
pip源 pip安装慢
```
---
**环境**
> - python 2.7/python 3.5 
> - CentOS/Ubuntu/Windows 10

---
**注**
> - 这里使用豆瓣的pip源为例：http://pypi.douban.com/simple/
> - **$** 在命令行表示当前用户下执行命令
---

## Linux下
在当前用户home目录下创建 **~/.pip/pip.conf**
```$ vim ~/.pip/pip.conf```
**添加以下内容：**
```
[global]
timeout = 6000
index-url = http://pypi.douban.com/simple/ 
[install]
use-mirrors = true
mirrors = http://pypi.douban.com/simple/ 
trusted-host = pypi.douban.com
```

## Windows下
windows下创建 **%HOME%/pip/pip.ini**
**编辑```pip.ini```,添加一下内容：**
```
[global]
timeout = 6000
index-url = http://pypi.douban.com/simple/ 
[install]
use-mirrors = true
mirrors = http://pypi.douban.com/simple/ 
trusted-host = pypi.douban.com
```
---
## 可能遇到的问题
暂无


--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 03月 22日 



[101]: https://www.lianch.com
