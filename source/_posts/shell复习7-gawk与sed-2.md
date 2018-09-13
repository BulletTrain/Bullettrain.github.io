title: shell复习7-gawk与sed(2)
date: 2015-01-20 14:15:53
categories: linux
tags: [shell,awk,sed]

---

##gawk编程设计

###函数

- length(str)  返回str的字符个数
- int(num) 返回num的整数部分
- index(str1,str2) 返回str2在str1中的位置，如果str2不存在则返回0
- split(str,arr,del) 用del作为定界符，将str的元素放到数组arr[1]...arr[n]中，返回数组元素个数
- sprintf(fmt,args) 根据fmt格式化args并返回格式化后的字符串，模范c语言同名函数
- substr(str,pos,len) 返回str中从pos开始的长度为len个字符的字符串
- tolower(str) 返回str的副本，且其中所有大写字母换成相应的小写字母
- toupper(str) 返回str的副本，且其中的所有小写字母换成相应的大写字母

###关联数组

关联数组是gawk最强大功能之一，关联数组使用字符串作为索引，在使用关联数组时候，用户可以用数值字符串作为索引来模仿系统的数字索引数组。

用户可以像某个值指派给任何其他awk变量一样，语法如下

````
array[string] =value
array为数组的名称，string为用户将要赋值的元素在数组中的索引，value为将指派给该元素的值
````
可以将特殊的for结构用于关联数组，语法如下

````
for (elem in array) action
````
for结构将遍历数组中的元素，elem表示数组中每个元素的值，array为数组名称，action为awk对数组中每个元素所采取的动作。可以在action中使用elem变量。

例如：

````shell
$ vim arrayexample
#filename:arrayexample
awk '{name[$1]=$5}END{for(elem in name)print elem" 的成绩为:" name[elem]}' example

$ chmod u+x arrayexample
$ ./arrayexample
xuxin 的成绩为:90
lisi 的成绩为:99
xuxing 的成绩为:81
liskai 的成绩为:78
lichang 的成绩为:99
wangxuebing 的成绩为:94
zhangsansan 的成绩为:88
wanglijiang 的成绩为:78
zhangsan 的成绩为:83
langxuebing 的成绩为:86
````

###格式化输出命令
使用printf代替print控制awk产生的输出。
printf "control-string",arg1,arg2,...,argn

- d 十进制
- e 指数表示
- f 浮点数字
- g 使用f或者e中较短的一个
- o 无符号八进制
- s 字符串
- x 无符号十六进制

###关系运算符

````
<、<=、==、!=、>=、>
````
###算数运算符

````
*、/、%、+、-、=、++、--、+=、-=、*=、/=、%=
````
例子

````
$ vim report
#!/bin/bash
#filename:report
if [ $# == 0 ]

then
   echo "必须指定一个数据文件"
   exit 1
fi
(date;cat $1)|

awk '
  BEGIN{total=0}
  NR == 1 {print "统计时间:",$2,$3","$6;print "--------------"}
  NR > 1 {print $1 ":" $5;total+=$5}
  END {print "--------------";printf "平均分为: %4.2f\n",total/(NR-1)}
  '
$ chmod u+x report
$ ./report
必须指定一个数据文件

$ ./report example
统计时间: 1月21日 星期三,
--------------
lichang:99
wanglijiang:78
zhangsansan:88
langxuebing:86
liskai:78
zhangsan:83
lisi:99
wanglijiang:78
xuxing:81
xuxin:90
wangxuebing:94
--------------
平均分为: 86.73
````

###流程控制语句

- if...else结构

````
$ vim ifelse
#filename:ifelse
BEGIN{
      name="John"
      if( name == "zhangsan" )
         print " name is zhangsan"
      else
         print " name is not zhangsan, it is",name
}

$ awk -f ifelse
 name is not zhangsan, it is John
````

- while循环结构

````
$ vim while1
#filename:while1
BEGIN{
   n = 2
   while( n <= 9 )
   {
      print "2^" n"=",2**n
      n++
   }
}

$ awk -f while1
2^2= 4
2^3= 8
2^4= 16
2^5= 32
2^6= 64
2^7= 128
2^8= 256
2^9= 512
````

- for循环结构

````
$ vim for1
#filename:for1
BEGIN {
      for(n=2;n<=9;n++)
         print "2^" n"=",2**n
}

$ awk -f for1
2^2= 4
2^3= 8
2^4= 16
2^5= 32
2^6= 64
2^7= 128
2^8= 256
2^9= 512
````

- break
- continue

###getline:控制输出

例子

````
$ vim gline
BEGIN{
      while( getline LINE )
      { 
         print NR": "LINE
      }
}

$ vim testdate
1111111
2222222
3333333
4444444

$ awk -f gline < testdate
1: 1111111
2: 2222222
3: 3333333
4: 4444444
````

###协进程
协进程是指与另一个进程并行的进程。gawk从版本3.1开始就可以通过协进程方式直接与某个后台进程进行信息交换
gawk通过在程序名称前面添加双向管道运算符|&来标识在后台运行的协进程

###网络数据交换
gawk可以通过IP网络连接到另一个系统上的某个进程进行信息交换。当用户指定了某个以/inet/开头的特殊文件的格式如下。
/inet/protocol/local-port/remote-host/remote-port
其中的protocol通常为TCP也可以为UDP。local-port表示为本地端口，如果用户希望让gawk选择一个端口，也可以将local-port设置为0。remote-host为远程主机的IP地址或域名，remote-port表示远程端口号，可以指定http也可以指定ftp地址。