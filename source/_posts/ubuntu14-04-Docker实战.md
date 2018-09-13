title: ubuntu14.04 Docker实战
date: 2015-04-20 15:50:20
categories: linux
tags: docker

---
参考自[docker官网](https://www.docker.com/)
##安装docker
确认wget已经安装

如未安装请使用如下命令安装

```
$ sudo apt-get update $ sudo apt-get install wget
```

安装docker

```
$ wget -qO- https://get.docker.com/ | sh
```

确认是否安装完毕

```
$ sudo docker run hello-world
```

##docker基本命令使用

使用如下命令获取需要的docker镜像

``
$ sudo docker pull ubuntu:14.04
``

或者搜索docker容器

```
$ sudo docker search ubuntu
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                           Official Ubuntu base image                      1585      [OK]
ansible/ubuntu14.04-ansible      Ubuntu 14.04 LTS with ansible                   49                   [OK]
ubuntu-upstart                   Upstart is an event-based replacement for ...   25        [OK]
torusware/speedus-ubuntu         Always updated official Ubuntu docker imag...   24                   [OK]
tutum/ubuntu                     Ubuntu image with SSH access. For the root...   22                   [OK]
sequenceiq/hadoop-ubuntu         An easy way to try Hadoop on Ubuntu             14                   [OK]
dorowu/ubuntu-desktop-lxde-vnc   Ubuntu with openssh-server and NoVNC on po...   11                   [OK]
guilhem/vagrant-ubuntu                                                           9                    [OK]
ubuntu-debootstrap               debootstrap --variant=minbase --components...   7         [OK]
......
```

运行docker容器并且进入docker容器的命令行

```
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
```

查看正在运行的docker容器 

```
$ sudo docker ps
CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES
1e5535038e28  ubuntu:14.04  /bin/sh -c 'while tr  2 minutes ago  Up 1 minute        insane_babbage
```

其他docker命令可使用如下命令进行查询

```
$ sudo docker
Usage: docker [OPTIONS] COMMAND [arg...]

A self-sufficient runtime for linux containers.

Options:
  --api-cors-header=                   Set CORS headers in the remote API
  -b, --bridge=                        Attach containers to a network bridge
  --bip=                               Specify network bridge IP
  -D, --debug=false                    Enable debug mode
  -d, --daemon=false                   Enable daemon mode
  ......
```

##创建自己的docker images并且导出

###运行下载好的docker images

```
$ sudo docker run -t -i ubuntu:14.04 /bin/bash
root@92e3aeb08f18:/#
```
###docker镜像操作

例如

```
root@92e3aeb08f18:/#apt-get install ant
```
等待安装完毕

使用docker commit进行保存操作

```
root@92e3aeb08f18:/#exit
$ sudo docker commit -m "add ant" -a "ruisu"  92e3aeb08f18 ubuntu:14.04b
45c7b4118ce4da89bf1e8f9a52ac2e701b4de8546bfba790cb10fe847ed3c4fb
```
 使用如下命令便会看到本地images多了一个叫ubuntu:14.04b的docker images

```
$ sudo docker images
```

如果已经注册docker hub账户可以直接push images到云端

```
$ sudo docker push ubuntu:14.04b
```

##配置docker私有库

上传仓库的导出文件xx.tar.gz到服务器的任意目录,这里上传到根目录

加载仓库镜像 

```
# cd /# docker load < xx.tar.gz
```查看导入的仓库镜像

```# docker images
```

启动仓库容器

```
# docker run -it --name=xx --net=host --restart=always --privileged=true xx
```

查看启动的仓库容器

```
# docker ps -a
```

##docker常用操作

###镜像导入- load方式导入镜像

```用法:docker load < 导入的文件名称示例:docker load < nginx_v1.0.tar.gz 在服务器上加载镜像```- import导入镜像文件

```用法:cat 文件名称 | docker import - [REPOSITORY][:tag]示例: nginx_v1.0.tar.gz | docker import - 导入不打tag的镜像示例: nginx_v1.0.tar.gz | docker import - 192.168.83.107:5000/nginx:v1 导入打tag的镜像镜像更改
```- 修改镜像tag

```用法:docker tag 镜像ID REPOSITORY:tag 示例:docker tag d6 192.168.83.107:5000/nginx:v1
```- 删除镜像tag

```用法:docker rmi REPOSITORY:tag示例:docker rmi 192.168.83.107:5000/nginx:v1容器
```- 启动docker容器

```用法:docker run [OPTIONS] IMAGE [COMMAND] [ARG...]示例:docker run -it --name=nginx --net=host --privileged=true --restart=always 192.168.83.107:5000/nginx:v1 
启动容器 
示例:docker run -it --name=nginx --net=host --restart=always --privileged=true 192.168.83.107:5000/nginx:v1 /bin/bash 以shell的方式启动容器 
退出容器ctrl+p+q退出容器  以交互式方式登录容器后可以采用上述方式退出镜像上传下载
```
- push镜像到仓库

```用法:docker push REPOSITORY:tag示例:docker push 192.168.83.107:5000/nginx:v1 备注:在向私有仓库push镜像的时候需要在docker客户机器的docker文件中增加如下内容(标红内容):vi /etc/default/dockerDOCKER_OPTS="--insecure-registry 192.168.83.107:5000" // DOCKER_OPTS="--insecure-registry 仓库服务器地址:5000" service docker restart //重启docker服务生效
```
- pull镜像到本地

```用法:docker pull REPOSITORY:tag示例:docker pull 192.168.83.107:5000/nginx:v1镜像导出
```- save方式导出镜像

```用法:docker save -o 导出的文件名称 镜像ID 示例:docker save -o=nginx_v1.0.tar.gz b8053ab2e5d3 备注:该导出方式导出的对象是镜像
```- export方式导出镜像

```用法:docker export 容器ID > 导出的文件路径及导出的文件名称示例:docker export 8c71d5f9e436 > ./nginx_v1.0.tar.gz 备注:该导出方式导出的对象为容器,save方式导出的文件比export方式导出的文件小
```