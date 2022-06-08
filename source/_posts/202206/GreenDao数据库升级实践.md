---
title: GreenDao数据库升级实践
date: 2022-06-08 14:24:04
tags: [Android,GreenDao,Sqlite]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/06/08/16546694444.html
top_img: https://tmx.fishpi.cn/img/town-7015699_1920.jpg
cover: https://tmx.fishpi.cn/img/town-7015699_1920.jpg
---

GreenDao是Android中常用的数据库管理框架
使用起来雀食方便高效
但是他有一个问题，就是在升级数据库的时候
比如加个字段，加个表之类的
他会删除原来的数据，删表重建
所以一般的解决方案都是创建一个临时表储存数据
升级完了再覆盖回去
虽然可行，但是操作实在过于麻烦

既然原生的SQL可以实现不删库加字段
按理说GreenDao应该不支持
毕竟使用量这么大的库，这个问题肯定有考虑的
在参考前人的文章后，终于找到合适的方案
不删库升级数据库，增加字段

首先，我们来看为什么会删库
在GreenDao make project后生成的`DaoMaster.java`里
有一个`DevOpenHelper`
```java
/** WARNING: Drops all table on Upgrade! Use only during development. */
public static class DevOpenHelper extends OpenHelper {
    public DevOpenHelper(Context context, String name) {
        super(context, name);
    }

    public DevOpenHelper(Context context, String name, CursorFactory factory) {
        super(context, name, factory);
    }

    @Override
    public void onUpgrade(Database db, int oldVersion, int newVersion) {
        Log.i("greenDAO", "Upgrading schema from version " + oldVersion + " to " + newVersion + " by dropping all tables");
        dropAllTables(db, true);
        onCreate(db);
    }
}
```
升级的时候其实是调用它的`onUpgrade`
然后在这里面，我们可以看到有一个`dropAllTables`的操作
然后又重新创建表，所以升级完数据都么了

原因找到了，解决方案就来了
我们不用它的升级就完事了hhhh

具体操作
1. 在对应的实体类增加字段或者新加实体类（加表）
2. make project
3. 修改GreenDao的schemaVersion
4. 修改升级方法，具体如下
```java
DaoMaster.DevOpenHelper helper = new DaoMaster.DevOpenHelper(this, "data-db"){
        @Override
        public void onUpgrade(Database db, int oldVersion, int newVersion) {
            //注释掉这个，不使用官方的升级
            //super.onUpgrade(db, oldVersion, newVersion);
            int currentVersion = oldVersion;
            if(currentVersion == 1){
                //可以直接用Dao建表
                OrderDetailDao.createTable(db,true);
                //增加字段
                db.execSQL("alter table ORDER_DETAIL add TYPE int");
                //这两句之后数据库版本来到了2
                currentVersion = 2;
            }
            if(currentVersion == 2){
                //可以直接用Dao建表
                ProductDao.createTable(db,true);
                //这句之后数据库版本来到了3
                currentVersion = 3;
            }
            if (currentVersion ==3){
                //增加字段
                db.execSQL("alter table ORDER_DETAIL add STATUS int");
                //这句之后数据库版本来到了4
                currentVersion = 4;
            }
            //理论上走完之后，当前版本已经等于新版本了
            //所以这个if并不会走进去
            if(currentVersion != newVersion){
                super.onUpgrade(db,oldVersion,newVersion);
            }

        }
    };
    Database db = helper.getWritableDb();
    daoSession = new DaoMaster(db).newSession();
```
至此，升级完成，数据也没丢，也不需要大改动


参考链接 -> [GreenDAO数据库升级](https://blog.csdn.net/qq_29924041/article/details/86584702)