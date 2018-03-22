**关键词**
```
oracle用户 oracle表空间
```
---
**环境**
> - oracle xe 11g

---
**注**
> - 使用有相关权限用户以sysdba身份登录，这里以system用户为例。
---

## 用户和表空间查看
### 查找用户
```select * from dba_users;```

### 查找表空间路径
```select * from dba_data_files;```

## 创建用户和表空间

### 创建表空间

```create tablespace TABLESPACE_NAME datafile 'TABLESPACE_PATH/TABLESPACE_NAME.dbf' size 200M;```

TABLE_SPACEA通过上面sql可查看，下面创建名为bi,表空间文件名为bi的表空间为例：

```create tablespace bi datafile '/u01/app/oracle/oradata/XE/bi.dbf' size 200M;```

### 创建用户

```create user UAER_NAME identified by USER_PASSWORD;```

下面创建用户名bi，密码123456的用户为例：

```create user bi identified by 123456;```

### 分配表空间给用户
创建用户以后默认分配给用户表空间```SYSTEM```。

```alter user USER_NAME default tablespace TABLESPACE_NAME;```

### 赋予用户权限
这里以赋予创建session，表和view等权限为例，具体其他权限自行查询。

```grant create session,create table,create view,create sequence,unlimited tablespace to USER_NAME;```

## 删除用户和表空间

### 删除用户

```drop user USER_NAME cascade;```

### 删除表空间

```drop tablespace TABLESPACE_NAME including contents and datafiles cascade constraint;```

---
## 可能遇到的问题
暂无


--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 03月 22日 



[101]: https://www.lianch.com