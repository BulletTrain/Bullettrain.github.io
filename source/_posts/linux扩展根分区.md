title: linux扩展根分区
date: 2015-01-15 16:41:06
categories: linux
tags: 分区
copyright: true
---

现况，现有一台服务器根分区空间不足了，目前有个一块sda硬盘未使用
解决方案：
sda划出一个分区扩展到根分区

操作如下

- 首先sda分出一个分区，并且格式化

分区

````
$ fdisk /dev/sda
command : n
command action
   e extended
   p primary partition
p
partition number(1-4):2 （如果此时存在一个sda1则选择2，如没有分区，直接选择1）
first cylinder(1-143595,default 1):直接enter
using default value 1
Last cylinder,+cylinders or +size: +700G
command : wq  此部wq保存退出
````

格式化

````
$ mkfs.ext4 /dev/sda2

此部可能比较慢
等待执行到writing superblocks and  filesystem accounting information:直接按回车按键，等待执行完成
````

- 扩展sda2到根分区

````
首先创建pv
$ pvcreate /dev/sda2
查看当前卷
$ vgdisplay   此时查看到vg_name后面需要
宽展sda2到当前卷
$ vgextend /dev/查询到的vg_name /dev/sda2
$ /sbin/resize2fs /dev/查询到的vg_name/根挂载点所对应的卷名
此步如果执行不可扩展根分区，请执行前先执行一下 
lvextend -L +700G /dev/mapper/vg_lc-lv_root 
在执行此条命令
$ df -h
此时可以看到挂载的文件系统名和挂载点以及容量的信息
````