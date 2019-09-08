title: yosemite开启HAXM硬件加速运行安卓虚拟机

date: 2014-10-27 22:50:40

categories: android

tags: [android,intel]

copyright: true

------

android sdk安装HAXM发现不能正常运行

``` 
$ kextstat | grep intel
```

发现无进程运行

``` 
$ sudo kextload –b com.intel.kext.intelhaxm  
/Users/Frank/–b failed to load - (libkern/kext) not found; check the system/kernel logs for errors or try kextutil(8).  
/Users/Frank/com.intel.kext.intelhaxm failed to load - (libkern/kext) not found; check the system/kernel logs for errors or try kextutil(8).  
```

发现无法加载libkern/kext，其实是内核不支持未签名的kext

解决办法：

``` 
Run sudo nvram boot-args="kext-dev-mode=1"
Restart.
Run sudo kextload -bundle-id com.intel.kext.intelhaxm
```

NOTE: By running sudo

 nvram boot-args="kext-dev-mode=1" you will allow ALL UNSIGNED KEXT to be loaded. Know your system.