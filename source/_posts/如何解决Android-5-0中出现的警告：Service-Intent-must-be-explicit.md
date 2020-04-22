title: "如何解决Android 5.0中出现的警告：Service Intent must be explicit"
date: 2015-06-27 13:49:05
categories: android
tags: [android5.0,service]
copyright: true
---

有些时候我们使用Service的时需要采用隐私启动的方式，但是Android 5.0一出来后，其中有个特性就是`Service Intent  must be explitict`，也就是说从`Lollipop`开始，`service`服务必须采用显示方式启动。
而android源码是这样写的
>源码位置：sdk/sources/android-21/android/app/ContextImpl.java

```java
private void validateServiceIntent(Intent service) {
        if (service.getComponent() == null && service.getPackage() == null) {
            if (getApplicationInfo().targetSdkVersion >= Build.VERSION_CODES.LOLLIPOP) {
                IllegalArgumentException ex = new IllegalArgumentException(
                        "Service Intent must be explicit: " + service);
                throw ex;
            } else {
                Log.w(TAG, "Implicit intents with startService are not safe: " + service
                        + " " + Debug.getCallers(2, 3));
            }
        }
    }
```

既然，源码里是这样写的，那么这里有两种解决方法：

- 设置Action和packageName：

>谷歌推荐方法

```java
Intent mIntent = new Intent();
mIntent.setAction("XXX.XXX.XXX");//你定义的service的action
mIntent.setPackage(getPackageName());//这里你需要设置你应用的包名
context.startService(mIntent);
```

- 将隐式启动转换为显示启动：

```java
public static Intent getExplicitIntent(Context context, Intent implicitIntent) {
        // Retrieve all services that can match the given intent
        PackageManager pm = context.getPackageManager();
        List<ResolveInfo> resolveInfo = pm.queryIntentServices(implicitIntent, 0);
        // Make sure only one match was found
        if (resolveInfo == null || resolveInfo.size() != 1) {
            return null;
        }
        // Get component info and create ComponentName
        ResolveInfo serviceInfo = resolveInfo.get(0);
        String packageName = serviceInfo.serviceInfo.packageName;
        String className = serviceInfo.serviceInfo.name;
        ComponentName component = new ComponentName(packageName, className);
        // Create a new intent. Use the old one for extras and such reuse
        Intent explicitIntent = new Intent(implicitIntent);
        // Set the component to be explicit
        explicitIntent.setComponent(component);
        return explicitIntent;
    }
```
调用方式如下：

```java
Intent mIntent = new Intent();
mIntent.setAction("XXX.XXX.XXX");
Intent eintent = new Intent(getExplicitIntent(mContext,mIntent));
context.startService(eintent);
```