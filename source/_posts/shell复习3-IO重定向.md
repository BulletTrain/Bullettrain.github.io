title: shell复习3-IO重定向
date: 2015-01-14 16:14:13
categories: linux
tags: shell
copyright: true

---
- 输出重定向 (<)
- 附加输出重定向（<<）

````
可连续输入邮件内容，直到输入exit
$ mail xxx@xx.com <<exit
> this is a text
> exit

````

- 错误重定向（2>）

````
$ ls errortest 2> file
把ls errortest打印的错误信息写进file文件
````

- 标准输出和错误输出重定向（&>）
- 输出重定向（>）

````
比如使用ls /etc > /ectfilelist
查看/etc下面文件列表写进etcfilelist文件
$ cat > file
hello shell!!!         //按回车按键
//按ctrl+c或者ctrl+d组合按键结束向file写入文件
````

- 附加输出重定向 (>>)

````
$ cat file    //源文件内容
$ cat > file  //写入内容，源内容会被覆盖
$ cat >> file  //append新的内容，源内容会被保留
````