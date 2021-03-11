---
title: '配置ubuntu系统常用技能'
date: 2019-12-25
permalink: /posts/2019/12/skills-on-ubuntu/
tags:
  - skills
---

## 配置静态ipv6地址

首先进入编辑界面
```
vi /etc/network/interfaces
```

再添加
```
iface eth0 inet6 static
```

## 配置新用户

这里推荐这种方式

```
useradd -d /home/user_name -m user_name
```
在home/下创建一个user_name 目录

之后再配置密码

```
passwd user_name
```

为该用户指定命令解释程序（通常为/bin/bash）
```
usermod -s /bin/bash user_name
```

删除账号
```
userdel -r user_name
```
-r 参数为删除该目录下所有的文件

## SSH 相关服务

### 指定ssh端口映射:
```
ssh -L 6006:localhost:6006 username@ip
```

### 杀死占用某个端口的程序：
```
fuser 6006/tcp -k
```
### 快捷ssh命令访问服务器
- 生成ssh 公钥
```
ssh-keygen
```

- 上传到服务器
```
scp -r ~/.ssh/id_rsa.pub  mirror@170.18.40.99:~/
```

- 写入
```
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

- 在本地处理
```
vim ~/.ssh/config
```
```
Host alians
    HostName dev.example.com
    Port 22
    User fooey
```

- 修改文件权限
```
sudo chmod 600 .ssh/config 
```

[参考](https://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/)

## 限定服务器python程序数量

原理是替换掉python程序启动方式
```
alias python='run-one python'
```
