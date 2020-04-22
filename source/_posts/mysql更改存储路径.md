title: mysql更改存储路径
date: 2015-04-03 16:11:15
categories: database
tags: mysql
copyright: true
---

###1、 关闭selinux
```
vi /etc/selinux/config
设置SELINUX=disabled
```
###2、停止mysql服务
```
service mysql stop
```
###3、创建新数据存储目录
```
mkdir /dbdir
```
###4、更新新数据目录的属主与属组
```
chown mysql:mysql /dbdir
```
###5、拷贝var/lib/mysql/目录下的所有文件到/dbdir目录下
```
cp -rp * /dbdir/
```
###6、修改my.cnf文件
```
vi /etc/my.cnf 
修改datadir为datadir = /dbdir/
```
###7、重启mysql服务
```
service mysql start
```