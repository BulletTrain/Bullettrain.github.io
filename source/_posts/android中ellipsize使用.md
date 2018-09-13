title: android中ellipsize使用
date: 2014-11-23 13:57:12
categories: android
tags: ellipsize

---

TextView文本过长的情况,使用ellipsize属性即可内容过长的情况自动加省略号

##用法如下

**再xml中使用ellipsize属性**

- android:ellipsize = "end" 省略号在结尾
- android:ellipsize = "start" 省略号在开头
- android:ellipsize = "middle" 省略号再中间
- android:ellipsize = "marquee" 跑马灯

最好加一个约束android:singleline = "true"

**在代码中使用**

- tv.setElipsize(TextUtils.TruncateAt.valueOf("END"));
- tv.setElipsize(TextUtils.TruncateAt.valueOf("START"));
- tv.setElipsize(TextUtils.TruncateAt.valueOf("MIDDLE"));
- tv.setElipsize(TextUtils.TruncateAt.valueOf("MARQUEE"));

 当然最好加一个约束tv.setSingleLine(true);
 
 edit同样支持该属性，只是不支持marquee属性

