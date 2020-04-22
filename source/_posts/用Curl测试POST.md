---
title: "用Curl测试POST"
date: 2015-07-16 13:36:04
categories: Web相关
tags: [curl,POST]
copyright: true
---

##POST请求  

例如如下
`http://xx.xx.xx.xx:8089/mobile/login?username=zhangsan&passwd=123456`

- 目的1：通过脚本发送post请求。

>答案： 

>`curl -d "username=zhangsan&passwd=123456" "http://xx.xx.xx.xx:8089/mobile/login"`


- 目的2：通过脚本发送post请求，顺便附带文本数据，比如通过"浏览"选择本地的card.txt并上传发送post请求

>答案：  

>curl  -F "blob=@card.txt;type=text/plain"  "http://xx.xx.xx.xx:8089/mobile/login?username=zhangsan&passwd=123456"

其中-F 为带文件的形式发送post请求，   blob为文本框中的name元素对应的属性值。`<type="text" name="blob">`