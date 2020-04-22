title: iOS9适配之Transport Security (ATS)
date: 2015-09-17 15:15:57
categories: iOS
tags: [iOS9,ATS]
copyright: true
---

今早升级了Xcode7，重新打开了公司的项目，编译运行，发现模拟器和真机都不能联网，以为后台接口出问题，再搜索半天才发现iOS9以及Xcode7增加了App Transport Security，缩写即ATS

查询apple官网文档发现如下

```
If you’re developing a new app, you should use HTTPS exclusively. If you have an existing app, you should use HTTPS as much as you can right now, and create a plan for migrating the rest of your app as soon as possible.
```
新的iOS9启用新的网络连接机制，推荐你使用https加密连接方式，没有使用https怎么办呢，使用如下办法就能解决

只需要在项目的Info.plist加入如下代码

```
<key>NSAppTransportSecurity</key> 
<dict> 
	<key>NSExceptionDomains</key> 
	<dict> 
		<key>yourserver.com</key> 
		<dict> 
			<!--Include to allow subdomains--> 
			<key>NSIncludesSubdomains</key> 
			<true/> 
			<!--Include to allow HTTP requests-->
			<key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key> 
			<true/> 
			<!--Include to specify minimum TLS version-->
			<key>NSTemporaryExceptionMinimumTLSVersion</key>
			<string>TLSv1.1</string> 
		</dict> 
	</dict> 
</dict>
```
