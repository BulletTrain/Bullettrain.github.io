title: "ld: library not found for -lPods出现错误"

date: 2015-07-07 10:46:54

categories: iOS

tags: iOS

copyright: true

------

若出现`ld: library not found for -lPods` 类似的错误，

设置 `Project` -> `Pods` 下所有第三方库的 `Build Active Architecture Only` 为 `NO`