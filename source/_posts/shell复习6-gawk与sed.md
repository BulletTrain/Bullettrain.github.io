---
title: shell复习6-gawk与sed
date: 2015-01-15 11:08:38
categories: linux
tags: [shell,gawk,sed]
copyright: true
---

##过滤器gawk(awk的GNU版本)
gawk是可以完成模式扫描和数据处理的语言。gawk搜索一个或者多个文件，查找其中匹配模式的记录,通常为行，通过执行指定的动作来处理这些记录。gawk可以使用变量、函数、算数运算符、关系运算符、关系数组、控制语句和c语言的printf语句，而高级gawk编程可以利用getline语句输入数据，使用协作进程让gawk与其他程序进行数据交换，或者通过网络连接与运行在远程系统上的数据交换数据。

***gawk常用选项说明***
 
 ````
 -F x  把x作为输入字段分隔符的值
 -f 从后面的文件中而不是标准输入中读取gawk程序，可命令行多次指定这个选项
 -W help 显示gawk的帮助信息
 -W hint 对不正确或者不利于移植的结构发出警告
 
 ````
 
 ***例子***
 
 模式的使用
 
````shell
$ awk '$2 >= 1988{print NR}' example 
1
5
7
8

$ awk '/math/' example   //数据行包含math的行
lichang       1989   male     math       99
langxuebing   1978   male     math       86
lisi          1988   male     math       99
wangxuebing   1979   male     math       94

$ awk '$1~/l/' example     //第一列包含l的数据行
lichang       1989   male     math       99
wanglijiang   1987   femail   chinese    78
langxuebing   1978   male     math       86
liskai        1990   female   chinese    78
lisi          1988   male     math       99
wanglijiang   1989   female   chinese    78

$ awk '{if($2~/1990/)$5=$5+5;print}' example  //第二列为1990的行第五列加上5
lichang       1989   male     math       99
wanglijiang   1987   femail   chinese    78
zhangsansan   1977   male     computer   88
langxuebing   1978   male     math       86
liskai 1990 female chinese 83
zhangsan      1977   male     computer   83
lisi          1988   male     math       99
wanglijiang   1989   female   chinese    78
xuxing        1979   male     economic   81
xuxin         1985   female   english    90
wangxuebing   1979   male     math       94

$ awk 'BEGIN{print "-----------------学生基本信息----------------"} {print}' example  
-----------------学生基本信息----------------
lichang       1989   male     math       99
wanglijiang   1987   femail   chinese    78
zhangsansan   1977   male     computer   88
langxuebing   1978   male     math       86
liskai        1990   female   chinese    78
zhangsan      1977   male     computer   83
lisi          1988   male     math       99
wanglijiang   1989   female   chinese    78
xuxing        1979   male     economic   81
xuxin         1985   female   english    90
wangxuebing   1979   male     math       94

$ awk 'BEGIN{print "统计学生总成绩";total=0} {print $5;total=total+$5} END{printf"学生平均成绩：%.2f\n",total/NR}' example 
统计学生总成绩
99
78
88
86
78
83
99
78
81
90
94
学生平均成绩：86.73

$ awk -F: '/zhangsan/,/zhangsansan/' example //包含zhangsan和zhangsansan之间的文本行
zhangsansan   1977   male     computer   88
zhangsan      1977   male     computer   83
lisi          1988   male     math       99
wanglijiang   1989   female   chinese    78
xuxing        1979   male     economic   81
xuxin         1985   female   english    90
wangxuebing   1979   male     math       94
````

***指定动作***

如果awk匹配某个模式，它将执行awk命令的动作部分所指定的动作。如果没有指定动作，awk将执行默认动作，即print命令（可以用{print}显示显示），这个动作将记录从输入复制到标准输出。
如果在print命令后面带上参数，awk将只显示用户指定的参数。这些参数可以是变量或者字符串常量。可以将print命令的输出发送到文件（>）、追加到文件（>>）或者通过管道将其发送到另外一个程序的输入（|）。协进程（|&）是一个双向管道，可以与运行在后台的程序交换数据。
除非用逗号将print命令中的各输入项区分开，否则awk将他们连起来输出。逗号使得awk用输出字段分隔符将各项分隔开。


***使用重定向输出***

awk可通过（>）可以重定向输出到文件

````shell
$ vim redrectout
/1989/ {print > "year1989"}
/1978/ {print > "year1978"}
END{print "输出完毕!"}
$ awk -f redirectout example
输出完毕!
$ cat year1978
langxuebing   1978   male     math       86
$ cat year1989
lichang       1989   male     math       99
wanglijiang   1989   female   chinese    78
````

***字符分隔符输出***
默认分隔符为空格，保存在变量OFS中，在用print语句进行输出打印时候，指定的多个字段之间用逗号分隔，而逗号表示就是OFS的值

````shell
//下面是输出包含Tom并且以:作为分隔符的行的第二行和第三行
$ awk -F[:] '/Tom/{print $2,$3}' student 
10511163 93-09-11
10501166 88-01-11

//下面是输出字段的分隔符重置为Tab(转义字符为\t)
$ awk 'BEGIN{FS=":";OFS="\t"}{print $1,$2,$3}' student
Tom Helleen	10511163	93-09-11
Tom Changle	10501166	88-01-11
Billy Black	10501165	93-02-18
John Hellen	10501162	93-04-13
Sam Possion	10501171	98-11-19
Mary Degens	10501169	95-05-22
Suli Vanlen	10501170	90-09-17

````
***记录和字段介绍***

gawk把输入文件看做一个具有一定格式和结构的连续字符串来处理。默认每一个以换行符结束的行为一个记录
记录中每一个以空格或者Tab分隔符的字符串为一个字段，称为字段（也称为域）。每一行记录的字段数目是不一样的，变量NF中保存了当前记录的字段数目。变量FS保存了字段使用的分隔符，默认为空格符或者Tab。可以在BEGIN语句中或命令行使用-F选项来修改FS的设置，从而修改分隔符。