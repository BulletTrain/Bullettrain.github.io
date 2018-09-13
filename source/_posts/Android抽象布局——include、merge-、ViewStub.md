title: "Android抽象布局——include、merge 、ViewStub"
date: 2015-05-04 09:54:52
categories: android
tags: [抽象布局,inlude,merge,ViewStub] 

---

在布局优化中，Androi的官方提到了这三种布局<include />、<merge />、<ViewStub />，并介绍了这三种布局各有的优势，下面也是简单说一下他们的优势，以及怎么使用:

##布局重用`<include />`

`<include />`标签能够重用布局文件，简单的使用如下：

```
<LinearLayoutxmlns:android"http://schemas.android.com/apk/res/android"
    android:orientation"vertical"
    android:layout_width=”match_parent”  
    android:layout_height=”match_parent”  
    android:background"@color/app_bg"
    android:gravity"center_horizontal"
    includelayout"@layout/titlebar"/>
    TextViewandroid:layout_width=”match_parent”  
              android:layout_height"wrap_content"
              android:text"@string/hello"
              android:padding"10dp"/>
</LinearLayout
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" 
    android:layout_width=”match_parent”
    android:layout_height=”match_parent”
    android:background="@color/app_bg"
    android:gravity="center_horizontal">

    <include layout="@layout/titlebar"/>

    <TextView android:layout_width=”match_parent”
              android:layout_height="wrap_content"
              android:text="@string/hello"
              android:padding="10dp" />

    ...

</LinearLayout>
```
1)<include />标签可以使用单独的layout属性，这个也是必须使用的。

2)可以使用其他属性。<include />标签若指定了属性，而你的layoutlayout会被覆盖，解决方案。

3)include标签中所有的android:layout_*都是有效的，前提是必须要写layout_widthlayout_height

4)布局中可以包含两个相同的include标签，引用时可以使用如下方法解决（参考）:

```
bookmarks_container_2findViewById(R.id.bookmarks_favourite);   
bookmarks_container_2.findViewById(R.id.bookmarks_list);  
View bookmarks_container_2 = findViewById(R.id.bookmarks_favourite); 

bookmarks_container_2.findViewById(R.id.bookmarks_list);
```

##减少视图层级`<merge />`

`<merge/>`标签在UI的结构优化中起着非常重要的作用，它可以删减多余的层级，优化UI。`<merge/>`多用于替换FrameLayout或者当一个布局包含另一个时，`<merge/>`标签消除视图层次结构中多余的视图组。例如你的主布局文件是垂直布局，引入了一个垂直布局的include，这是如果include布局使用的LinearLayout就没意义了，使用的话反而减慢你的UI表现。这时可以使用`<merge/>`标签优化。

```
<merge xmlns:android"http://schemas.android.com/apk/res/android"
    Button
        android:layout_width"fill_parent"
        android:layout_height"wrap_content"
        android:text"@string/add"/>
    Button
        android:layout_width"fill_parent"
        android:layout_height"wrap_content"
        android:text"@string/delete"/>
</merge
<merge xmlns:android="http://schemas.android.com/apk/res/android">

    <Button
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:text="@string/add"/>

    <Button
        android:layout_width="fill_parent" 
        android:layout_height="wrap_content"
        android:text="@string/delete"/>

</merge>
```

现在，当你添加该布局文件时(使用`<include />`标签)，系统忽略`<merge />`节点并且直接添加两个Button。更多详细请参考 [官方文档](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)


##需要时使用`<ViewStub />`

`<ViewStub />`标签最大的优点是当你需要时才会加载，使用他并不会影响UI初始化时的性能。各种不常用的布局想进度条、显示错误消息等可以使用`<ViewStub />`标签，以减少内存使用量，加快渲染速度。`<ViewStub />`是一个不可见的，大小为View。`<ViewStub />`标签使用如下：

```
<ViewStub
    android:id"@+id/stub_import"
    android:inflatedId"@+id/panel_import"
    android:layout"@layout/progress_overlay"
    android:layout_width"fill_parent"
    android:layout_height"wrap_content"
    android:layout_gravity"bottom"/>
<ViewStub
    android:id="@+id/stub_import"
    android:inflatedId="@+id/panel_import"
    android:layout="@layout/progress_overlay"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:layout_gravity="bottom" />
```
    
当你想加载布局时，可以使用下面其中一种方法：

```
((ViewStub) findViewById(R.id.stub_import)).setVisibility(View.VISIBLE);  
View importPanel = ((ViewStub) findViewById(R.id.stub_import)).inflate();  
((ViewStub) findViewById(R.id.stub_import)).setVisibility(View.VISIBLE);
// or
View importPanel = ((ViewStub) findViewById(R.id.stub_import)).inflate();
```

inflate()函数的时候，ViewStub被引用的资源替代，并且返回引用的这样程序可以直接得到引用的而不用再次调用函数findViewById()来查找了。
ViewStub目前有个缺陷就是还不支持 `<merge />`
更多介绍详细[官方介绍](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-with.html)

