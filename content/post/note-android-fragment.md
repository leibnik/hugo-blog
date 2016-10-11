---
date: 2015-05-31
categories: android
tags: [android,note]
title: 【笔记】Android Fragment
layout: post
---

《Android第一行代码》的学习笔记

#### **一、动态添加碎片的步骤**

1. 创建待添加的碎片实例。
2. 获取到FragmentManager，在活动中可以直接调用getFragmentMananager()方法得到。
3. 通过调用beginTransaction()开启事务
4. 向容器内加入碎片，一般使用replace()方法实现，需要传入容器的id和待添加的碎片实例
5. 提交事务，调用commit()方法来完成

实现的代码

```java
@Override
public void onClick(View v) {
    switch (v.getId()){
        case R.id.button:
            AnotherRightFragment fragment = new AnotherRightFragment();
            FragmentManager fragmentManager = getFragmentManager();
            FragmentTransaction transaction = fragmentManager.beginTransaction();
            transaction.replace(R.id.right_layout,fragment);
            transaction.commit();
            break;
        default:
            break;
    }
}
```

<!-- more -->

当被另一碎片替换后希望能返回上一碎片，可以在`transaction.commit()`之前执行` transaction.addToBackStack(null);`
当按下Back键时程序直接退出，可以使用FragmentTransaction中提供的addToBackStack()方法，添加到返回栈，这样按下Back键是程序会返回上一个Fragment

#### **二、Fragment与Activity之间的通信**

在活动想获得碎片的实例可以调用FragmentManager提供的一个类似findViewById()的方法——findFragmentById()来获取

```
RightFragment rightFragment = (RightFragment)getFragmentManager().findFragmentById(R.id.right_fragment);
```
而在碎片中要想获得活动或者说是Context对象可以使用activity的getActivity()方法

``` 
MainActivity activity = (MainActivity)getActivity();
```
此外，碎片与碎片之间进行通信只需参考上面的两个方法，即先获得其中一个碎片的活动实例，再通过该实例获得另一碎片的实例。

#### **三、Fragment的生命周期**

**碎片的状态**

1. 运行状态 当一个碎片是可见的，并且它所关联的活动正处于运行状态时，该碎片也处于运行状态。
2. 暂停状态 当一个活动进入暂停状态时（由于另一个未占满屏幕的活动被添加到了栈顶），与它关联的可见碎片就会进入暂停状态。
3. 停止状态 当一个活动进入停止状态时，与它关联的碎片就会进入到停止状态。或者通过调用FragmentTransaction的remove()、replace()方法将碎片从活动中移除，但有在事务提交之前调用addToBackStack()方法，这时的碎片也会进入到停止状态。总的来说，进入停止状态的碎片对用户来说是完全不可见的，有可能会被系统回收。
4. 销毁状态 碎片总是依附于活动而存在的，因此当活动被销毁时，与它相关联的碎片就会进入到销毁状态。或者通过调用FragmentTransaction的remove()、replace()方法将碎片从活动中移除，但在事务提交之前并没有调用addToBackStack()方法，这时碎片也会进入到销毁状态。

**碎片除了活动有的毁掉方法外还提供了附加的毁掉方法**

1. onAttach():当碎片和活动建立关联时调用
2. onCreateView():为碎片创建视图（加载布局）时调用
3. onActivityCreated():确保与碎片相关联的活动一定已经创建完毕的时候调用
4. onDestroyView():当与碎片关联的视图被移除的时候调用
5. onDetach():当碎片和活动解除关联的时候调用
**fragment的生命周期**

![fragment](http://ww3.sinaimg.cn/mw690/b5405c76gw1esnkg0irhfj208t0njjt2.jpg)

**Activity与Fragment的比较**

![frag&acti](http://ww2.sinaimg.cn/mw690/b5405c76gw1esnkfzqujwj209g0irwfg.jpg)

**log展示**

程序开始运行时

```
05-23 14:59:06.118  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onAttach
05-23 14:59:06.118  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onCreate
05-23 14:59:06.118  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onCreateView
05-23 14:59:06.128  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onActivityCreated
05-23 14:59:06.148  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onStart
05-23 14:59:06.148  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onResume
```
没有执行`transaction.addToBackStack(null);`点击按钮替换为另一Fragment，按返回键程序直接退出

```
05-23 15:04:35.459  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onPause
05-23 15:04:35.469  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onStop
05-23 15:04:35.469  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onDestroyView
05-23 15:04:35.469  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onDestroy
05-23 15:04:35.469  14247-14247/droidroid.com.fragmenttest D/RightFragment﹕ onDetach
```
执行了`transaction.addToBackStack(null);`点击按钮替换为另一Fragment

```
05-23 15:17:41.436  21532-21532/droidroid.com.fragmenttest D/RightFragment﹕ onPause
05-23 15:17:41.436  21532-21532/droidroid.com.fragmenttest D/RightFragment﹕ onStop
05-23 15:17:41.436  21532-21532/droidroid.com.fragmenttest D/RightFragment﹕ onDestroyView
```
点击返回键，恢复上一Fragment

```
05-23 15:18:02.606  21532-21532/droidroid.com.fragmenttest D/RightFragment﹕ onActivityCreated
05-23 15:18:02.606  21532-21532/droidroid.com.fragmenttest D/RightFragment﹕ onStart
05-23 15:18:02.606  21532-21532/droidroid.com.fragmenttest D/RightFragment﹕ onResume
```
到桌面键

```
05-31 17:24:52.718  13870-13870/droidroid.com.fragmenttest D/RightFragment﹕ onPause
05-31 17:24:53.018  13870-13870/droidroid.com.fragmenttest D/RightFragment﹕ onStop
```
回到应用

```
05-31 17:25:13.608  13870-13870/droidroid.com.fragmenttest D/RightFragment﹕ onStart
05-31 17:25:13.608  13870-13870/droidroid.com.fragmenttest D/RightFragment﹕ onResume
```
#### **四、动态加载布局的技巧**
使用限定符

![限定符](http://ww2.sinaimg.cn/mw690/b5405c76gw1esnkrvli9gj20ec0bgq4q.jpg)

比如在res下建立一个新的布局文件夹`layout-large`，并且分别在`layout`和`layout-large`中添加布局文件，目标手机系统自行判断，大屏幕手机自动加载`layout-large`里的布局文件
使用最小限定符
在res下新建`layout-sw600dp`，当程序运行在屏幕宽度大于600dp的设备上时，会加载`layout-sw600dp`里的布局文件。

