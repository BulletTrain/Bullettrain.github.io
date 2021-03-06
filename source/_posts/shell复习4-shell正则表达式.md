title: shell复习4-shell正则表达式
date: 2015-01-14 16:14:31
categories: linux
tags: shell
copyright: true
---

##字符集和类
###连字符表示一个字符范围

````
[a-zA-Z] 表示匹配到的任意一个字母

[A-Z] 表示匹配任意一个大写字母

[a-z] 表示匹配任意一个小写字母

[0-9] 表示匹配任意一个数字

[0-9\*\+] 表示匹配数字、星号、加号中的任意一个
````
###如果中括号使用符号^,则表示取非

````
如上前面加上[^a-z]为非某某
````
##重复
###重复定义

````
符号* 表示匹配0个或者多个前导表达式
符号？表示匹配0个或者1个前导表达式
符号+ 表示匹配1个或者多个前导表达式
大括号{} 表示前导表达式指定一个最小或最大匹配数目
{x,y} 表示前导表达式至少出现x次，但不超过y次
{x,} 表示前导表达式出现x次或更多次
````

##子表达式
###子表达式定义
正则表达式中可以使用小括号将一个长的表达式分隔成几个子表达式或将字符、元字符、子表达式组合在一起。
例如 ([xy])([wz])可以匹配"xw" "xz" "yw" "yz"

##定位字符串的开始和末尾
###开头和结尾的定义
插入符号^用于匹配字符串的头部，表示指定的字符序列必须出现在被匹配字符串的开始位置；美元符号$用于匹配字符串的尾部，表示指定字符串必须出现在被匹配字符串的结束位置

````
例子如下：
^welcome 表示匹配以welcome开头的字符串
est$ 表示匹配以est为结尾的字符串
^[[:alpha:]]{3}$ 表示正则表达式匹配所有三个字母的字符串
^[0-9]{1,}$ 匹配所有的正整数
^\-{0,1}[0-9]{1,}$或者^\-?[0-9]{1,}$ 匹配所有的整数
^\-{0,1}[0-9]{1,}\.{0,1}[0-9]{0,}$ 匹配所有的小数
````

##分支
在正则表达式中使用符号|表示一个分支，分支具有“或”的意义，表示可以在多个选择中任选一个或者多个

````
例如
a|b相当于[ab]
^good(ness|ful|)$ 相当于匹配goodness和goodful
````

##匹配特殊字符
正则表达式中特殊字符，如.、$等，是不能当做普通字符进行匹配的。要匹配这些特殊字符必须使用反斜杠\进行转义