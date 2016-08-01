---
title: Android实践系列之Activity管理器
comments: true
date: 2016-07-14 16:28:47
update: 2016-07-14 16:28:47
categories: Android
tags: ['Android', 'Activity', '项目实践']
---

上一篇文章：[Android实践系列之项目基础配置](http://dkylin.com/archives/2016/android-base-config.html)

在进行Android开发的时候，我们需要对Activity进行各种操作，各种跳转。如果不对这些Activity进行有效的管理，将会出现各种我们不希望看见的结果。此时我们需要一个Activity管理器帮助我们有效的管理Activity。

<!-- 比如有时候我们可能会有这样的需求，一个只有登录之后才可以使用的App，在用户登录成功进入主界面后，登录界面会被销毁。此时用户进入主界面 -> 个人信息界面 -> 设置界面。点击退出登录，重新进入登录界面。此时按返回键应该提示用户退出应用，而不是回到之前的设置界面。如果用户在这里退出应用，那么我们应该销毁所有的Activity。针对这个问题，也有一些处理手段，但是我觉得加入一个Activity管理器将会更加方便快捷的处理这种问题。并且其作用也并非止于此。 -->

<!-- more -->

## 1. 创建Activity管理器

创建一个MyActivityManager类对Activity进行管理。代码如下：

```java
/**
 * Activity管理器
 *
 * Created by Kylin on 2016-07-14.
 */
public class MyActivityManager {

    private static List<Activity> activityList = new ArrayList<Activity>();
    private static MyActivityManager instance;

    public static MyActivityManager getInstance(){
        if(instance == null){
            instance = new MyActivityManager();
        }
        return instance;
    }

    /**
     * 添加 Activity 到列表
     * @param activity activity
     */
    public static void addActivity(Activity activity){
        if(activityList == null){
            activityList = new ArrayList<Activity>();
        }
        activityList.add(activity);
    }

    /**
     * 获取界面数量
     * @return activity size
     */
    public static int getActivitySize(){
        if(activityList != null){
            return activityList.size();
        }
        return 0;
    }

    /**
     * 获取当前 Activity - 堆栈中最后一个压入的
     * @return current Activity
     */
    public static Activity getCurrentActivity(){
        if(activityList != null && activityList.size() > 0){
            Activity activity = activityList.get(activityList.size()-1);
            return activity;
        }
        return null;
    }

    /**
     * 获取指定类名的 Activity
     *
     * @param cls 指定的类
     * @return Activity
     */
    public static Activity getActivity(Class<?> cls){
        if(activityList == null){
            return null;
        }
        for (Activity activity : activityList) {
            if(activity.getClass().equals(cls)){
                return activity;
            }
        }
        return null;
    }

    /**
     * 结束指定的 Activity
     * @param activity Activity
     */
    public static void removeActivity(Activity activity){
        if(activity != null){
            activityList.remove(activity);
        }
    }

    /**
     * 结束指定类名的 Activity
     * @param cls 指定的类
     */
    public static void removeActivity(Class<?> cls){
        if(activityList == null){
            return;
        }
        for (Activity activity : activityList) {
            if(activity.getClass().equals(cls)){
                activityList.remove(activity);
            }
        }
    }

    /**
     * 结束所有Activity
     */
    public static void finishAllActivity(){
        if(activityList == null){
            return;
        }
        int size = activityList.size();
        for (int i = 0; i < size; i++){
            if (null != activityList.get(i)){
                activityList.get(i).finish();
            }
        }
        activityList.clear();
    }

    /**
     * 结束其他所有的Activity
     * @param activity 不需要销毁的Activity
     */
    public static void finishOtherAllActivity(Activity activity){
        if(activityList == null){
            return;
        }
        for (int i = 0, size = activityList.size(); i < size; i++){
            if (activity != activityList.get(i)){
                activityList.get(i).finish();
                activityList.remove(i);
            }
        }
    }
}
```

## 2. 创建BaseActivity

现在有了Activity管理器，但是我不想再每个界面都需要把Activity添加到列表中，在每次销毁的时候再将其移除，太麻烦了。那么现在我们来写一个BaseActivity，把操作放在这里。这样让所有的Activity继承BaseActivity就可以了。代码如下：

```java
/**
 * BaseActivity
 *
 * Created by Kylin on 2016-07-14.
 */
public class BaseActivity extends Activity{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        MyActivityManager.addActivity(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        MyActivityManager.removeActivity(this);
    }
}
```

这样只需要每个Activity继承这个BaseActivity就可以了。在Activity onCreate()的时候将Activity添加到全局的列表中。在onDestroy()的时候将其移除就可以了。

## 3. 优雅的退出程序

在主界面以及登录界面，有时按返回键是需要退出应用的，这时我们可以使用MyActivityManager里面的finishAllActivity()方法销毁所有界面，退出应用。代码如下：

```java
 @Override
 public boolean onKeyDown(int keyCode, KeyEvent event) {
     if (keyCode == KeyEvent.KEYCODE_BACK && event.getAction() == KeyEvent.ACTION_DOWN) {
         //两秒之内按返回键就会退出
         if ((System.currentTimeMillis() - exitTime) > 2000) {
             T.showShort(this, "再按一次退出程序");
             exitTime = System.currentTimeMillis();
         } else {
             MyActivityManager.finishAllActivity();
//           System.exit(0); // Kill当前应用的进程,不建议使用.
         }
         return true;
     }
     return super.onKeyDown(keyCode, event);
 }
```

## 4. FragmentActivity中使用

在FragmentActivity中使用可以创建一个BaseFragmentActivity。也可以在FragmentActivity中直接使用。方法与上面相同，不再赘述。

## 5. 其他用法

* finishOtherAllActivity()：除了指定的界面销毁其他所有界面
* getCurrentActivity()：获取当前界面
* getActivitySize()：获取栈中Activity的数量

## 6. 结语

源代码已上传Github，在Github上可以查看。

Github地址：[https://github.com/dkylin/AndroidDemo/tree/ActivityManager](https://github.com/dkylin/AndroidDemo/tree/ActivityManager)

目前我的项目中用的就是这种方式，可以很方便的管理Activity。还在等什么，快来试试吧！

