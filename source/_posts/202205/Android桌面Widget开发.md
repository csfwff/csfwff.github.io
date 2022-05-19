---
title: Android桌面Widget开发
date: 2022-05-18 18:59:31
tags: [Android,widget]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/05/18/1652871571111.html
top_img: https://tmx.fishpi.cn/img/avocado-6931344_1920.jpg
cover: https://tmx.fishpi.cn/img/avocado-6931344_1920.jpg
---

## 00
桌面widget指的是放在桌面的小组件（你这不是废话么……）
举个栗子，就是常见时间天气组件，网易云快捷播放的组件
甚至连我的领克app，也有快捷控制车窗，解锁的组件
![LynkCo](https://tmx.fishpi.cn/img/Snipaste_2022-05-19_11-22-10.jpg)
iOS在iOS 14的时候也推出了桌面组件
然而，Android在1.5的时候就已经有了

## 01
怎么写一个呢，最简单的当然是百度一下
其实看官方的文档就可以了-> [传送门](https://developer.android.com/guide/topics/appwidgets/overview?hl=zh-cn)
就是这该死的机翻，我感觉还不如看英文的
然后过一遍官方的demo就行了-> [传送门](https://android.googlesource.com/platform/development/+/master/samples/ApiDemos/src/com/example/android/apis/appwidget)

## 02
接下来实践一下，光看是不行的
首先要有一个idea
这里我的idea是显示一个日期倒计时
比如距离端午节还有多少天
大概就这样
![栗子](https://tmx.fishpi.cn/img/Snipaste_2022-05-19_10-26-54.jpg)
新建个工程开始吧~

## 03
首先，一个widget有3部分
1. AppWidgetProviderInfo 定义在xml中，是一个xml文件，放在res/xml下
2. AppWidgetProvider widget的具体实现，kotlin或java都行
3. 布局文件 丢到layout下就行，需要注意的是只支持指定的组件，并不是啥都能用

然后在ManiFest清单文件中注册
```xml
<receiver
    android:name=".LeftWidgetProvider"
    android:exported="true">
    <intent-filter>
        <!-- 这个必须声明 -->
        <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
    </intent-filter>
    <!-- 指定AppWidgetProviderInfo资源XML文件 -->
    <meta-data
        android:name="android.appwidget.provider"
        android:resource="@xml/left_day_widget_provider" />
</receiver>
```
其实就是一个receiver
再看一下AppWidgetProviderInfo的内容，也就是xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
    android:minWidth="110dp"
    android:minHeight="40dp"
    android:updatePeriodMillis="1800000"
    android:previewImage="@drawable/ic_launcher_foreground"
    android:initialLayout="@layout/widget_left_day"
    android:resizeMode="horizontal|vertical"
    android:configure="com.xiamo.myapplication.LeftDayConfigActivity"
    android:widgetCategory="home_screen">

</appwidget-provider>
```
其中各项属性说明，直接翻前面给的官方文档就行了，我就不来抄一遍了
因为我们可能添加多个widget，而每个的时间可能不一样的，所以配置了一个configure用的Acitivy
每次新增widget的时候，会启动这个acitivity，我们需要在这个activity中完成配置，然后保存，刷新widget

## 04
然后是AppWidgetProvider的具体实现，建议看一下官方的demo
```kotlin
class LeftWidgetProvider : AppWidgetProvider() {

    override fun onReceive(context: Context?, intent: Intent?) {
        super.onReceive(context, intent)
    }

    //到时间更新的时候会调用
    override fun onUpdate(
        context: Context,
        appWidgetManager: AppWidgetManager,
        appWidgetIds: IntArray
    ) {
        super.onUpdate(context, appWidgetManager, appWidgetIds)
        var appWidgetManager = AppWidgetManager.getInstance(context)
        appWidgetIds.forEach {
            //更新widget就行
        }

    }
    //widget被删除会调用，删除保存的配置
    override fun onDeleted(context: Context, appWidgetIds: IntArray) {
        super.onDeleted(context, appWidgetIds)
        appWidgetIds.forEach {
            //删除对应的配置
        }
    }
    //widget配置改变的时候调用，例如尺寸发生改变
    override fun onAppWidgetOptionsChanged(
        context: Context,
        appWidgetManager: AppWidgetManager,
        appWidgetId: Int,
        newOptions: Bundle?
    ) {
        super.onAppWidgetOptionsChanged(context, appWidgetManager, appWidgetId, newOptions)
          //更新widget就行
    }

}

```
更新widget的时候，需要先创建RemoteViews，然后通过提供的方法来更新
最后调用`appWidgetManager.updateAppWidget(appWidgetId, remoteViews)`刷新
```kotlin
    val remoteViews = RemoteViews(context!!.packageName, R.layout.widget_left_day)
    remoteViews.setTextViewText(R.id.contentTv, info)
    appWidgetManager.updateAppWidget(appWidgetId, remoteViews)
```

## 05
关于配置用的Acitivy，需要在清单文件中增加配置
```xml
<activity
    android:name=".LeftDayConfigActivity"
    android:exported="true" >
    <intent-filter>
        <action android:name="android.appwidget.action.APPWIDGET_CONFIGURE"/>
    </intent-filter>
</activity>
```
然后再onCreate中通过intent获取对应的widgetId
```kotlin
var appWidgetId = intent?.extras?.getInt(
    AppWidgetManager.EXTRA_APPWIDGET_ID,
    AppWidgetManager.INVALID_APPWIDGET_ID
) ?: AppWidgetManager.INVALID_APPWIDGET_ID
```
信息配置完成后，将信息保存，注意绑定widgetId，存SharedPreferences或者别的啥的
然后调用appWidgetManager完成界面刷新，因为有配置界面的widget添加的时候不会调用onUpdate，需要自己完成第一次的更新操作
然后设置result，最后finish掉就行了
```kotlin
val appWidgetManager = AppWidgetManager.getInstance(this)
//这个是刷新用的静态方法
//其实是appWidgetManager.updateAppWidget(appWidgetId, remoteViews)
LeftWidgetProvider.updateWidget(this,appWidgetId,appWidgetManager)
val resultValue = Intent()
resultValue.putExtra(AppWidgetManager.EXTRA_APPWIDGET_ID, appWidgetId)
setResult(RESULT_OK, resultValue)
finish()
```

## 06
至此，一个简单的widget就完成了
当然还有很多细节没有完成
比如配置预览图，突破限制的30分钟更新时间
还有列表啥的
不过这些官方文档都有写
我这也只是简单学习下桌面组件的做法
emm，就先这样吧



