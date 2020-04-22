title: 如何在centos6搭建shadowsockets
date: 2014-11-30 18:56:49
categories: linux
tags: [shadowsockets,翻墙]
copyright: true
---

###第一步

````
# yum install  openssl-devel
````
 

###第二步

安装gcc,make等源码编译安装必要的工具

````

yum -y install gcc automake autoconf libtool make

yum -y install gcc gcc-c++

yum -y  install git

````

###第三步
下载ss源码包进行编译安装

````
git clone https://github.com/madeye/shadowsocks-libev.git

cd  shadowsocks-libev

./configure

make  && make install 
````

###第四步

````
nohup ss-server -s ipaddress -p port -k password -m a
es-256-cfb &
````
说明：ipaddress是vps空间的ip地址，port是端口号，password是密码,-m后面是加密方式

**加入开机启动**

````
echo "nohup ss-server -s ipaddress -p port -k password -m aes-256-cfb &" >> /etc/init.d
````

