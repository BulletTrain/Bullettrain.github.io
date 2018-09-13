title: Fragment生命周期
date: 2014-11-23 10:40:21
categories: android
tags: Fragment

---
###Fragment生命周期
![Fragment生命周期](http://yspe2371e4aa7697989.yunshipei.cn/dHlwZT1mdyZzaXplPTY0MCZzcmM9YUhSMGNDVXpRU1V5UmlVeVJtbHRaeTV0ZVM1amMyUnVMbTVsZENVeVJuVndiRzloWkhNbE1rWXlNREV5TVRFbE1rWXlPU1V5UmpFek5UUXhOekEyT1RsZk5qWXhPUzV3Ym1jPQ==)

###与Activity生命周期对比
![Activity生命周期](http://yspe2371e4aa7697989.yunshipei.cn/dHlwZT1mdyZzaXplPTY0MCZzcmM9YUhSMGNDVXpRU1V5UmlVeVJtbHRaeTV0ZVM1amMyUnVMbTVsZENVeVJuVndiRzloWkhNbE1rWXlNREV5TVRFbE1rWXlPU1V5UmpFek5UUXhOekEyT0RKZk16Z3lOQzV3Ym1jPQ==)

###具体场景演示 : 
切换到该Fragment

- onAttach
- onCreate
- onCreateView
- onActivityCreated
- onStart-onResume

屏幕灭掉：

- onPause
- onSaveInstanceState
- onStop

屏幕解锁
- onStart
- onResume

切换到其他Fragment:

- onPause
- onStop
- onDestroyView

切换回本身的Fragment:

- onCreateView
- onActivityCreated
- onStart
- onResume

回到桌面

- onPause
- onSaveInstanceState
- onStop

回到应用

- onStart
- onResume

退出应用

- onPause
- onStop
- onDestroyView
- onDestroy
- onDetach