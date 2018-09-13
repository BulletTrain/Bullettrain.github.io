title: nginx反向代理oracle数据库
date: 2014-12-30 20:17:43
categories: linux
tags: [nginx,oracle]

---

##引言
旧的四台服务器访问10.xx.xx.25数据库服务器速度很快，无延迟，无掉包现象，而新添加的几台服务器访问48数据库速度很慢，经常性掉包（检测端口开启访问正常，防火墙正常，初步断定就是网络路由的问题？！），导致无法启动一些需要访问oracle数据库的应用，但是新的几台服务器访问老的几台服务器很快，查询了资料，可以通过反向代理的方式去在新的服务器去访问数据库服务器，方法如下：

- 旧的服务器通过ssh代理端口映射，新的服务器通过访问旧的服务器来访问数据库服务器

- 旧的服务器通过旧服务器nginx的tcp代理插件进行反向代理访问数据库

经过测试ssh代理端口映射ssh -L1521:10.xx.xx.48:1521 oracle@10.xx.xx.115,经过测试无法完成所需要的需求，暂时先pass
所以目前只剩下第二种解决方案，特此记录下过程。

###一、下载所需软件

nginx目前最新稳定版本是1.6.2，所以下载最新的1.6.2版本

下载nginx的tcp插件

````
https://codeload.github.com/yaoweibin/nginx_tcp_proxy_module/zip/master
````
nginx解压到/tmp文件夹，tcp插件解压到nginx解压的文件夹

###二、编译安装nginx和tcp插件

````
$ tar zxvf nginx-1.6.2.tar.gz
$ cd nginx-1.6.2
$ ./configure --add-module=/tmp/nginx1.6.2/nginx_tcp_proxy_module
$ make
$ make install

````
###三、测试nginx运行状态

````
启动nginx
$ /usr/local/nginx/sbin/nginx

测试nginx是否启动
$ ps aux | grep nginx

测试启动完成进行下一步
````

###四、修改nginx配置添加oracle服务器的tcp反向代理

````
进入到/usr/local/nginx/conf目录编辑nginx.conf
添加如下配置

tcp {
    upstream oracle {
        server 10.xx.xx.48:1521;
        check interval=3000 rise=2 fall=5 timeout=1000;
    }
    server {
        listen 1521;
        proxy_pass oracle;
    }
}

````
此时会出现一个问题，tcp连接会中断掉线，因为服务端关闭时候，客户端不可能马上就关闭，这是就需要添加保持连接配置，修改参数如下

````
tcp {
    timeout 1d;
    proxy_read_timeout 10d;
    proxy_send_timeout 10d;
    proxy_connect_timeout 30;
    upstream oracle {
        server 10.xx.xx.48:1521;
        check interval=3000 rise=2 fall=5 timeout=1000;
    }
    server {
        listen 1521;
        proxy_pass oracle;
        so_keepalive on;
        tcp_nodelay on;
    }
}
````
此时就可以通过代理服务器的1521端口去访问目标数据库了,即新的服务器如果需要访问10.xx.xx.48数据库，只需要配置老服务器服务配置10.xx.xx.115:1521进行访问。