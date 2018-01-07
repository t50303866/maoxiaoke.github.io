---
layout: post
title: "Activity的生命周期"
date: 2018-01-07 19:00:00 +0800 
categories: Android笔记
tag: Android
---
* content
{:toc}

##典型情况下的生命周期##
###正常情况下###
- `onCreate`: Acitivity正在被创建，可以做一些初始化工作，如：setContentView加载布局资源、初始化Activity所需的数据等。

-`onRestart`: Activity正在重启，当当前Activity从不可见重新变为可见状态时，onRestart就会被启用。一般是**用户行为**所导致。

-`onStart`: Activity正在被启动，这时Activity已经可见了，但是还没有出现在前台，还无法和用户交互。**Activity已经显示出来了，但是我们看不到**。

-`onResume`: Activity已经可见，并且出现在前台并开始活动。

-`onPause`: Activity正在停止，正常情况下，紧接着onStop就会被调用。此时，可以做一些存储数据、停止动画等工作，不能太耗时。**因为onPause必须先执行，新Activity的onResume才会执行**。

-`onStop`: Activity即将停止，可以做些稍重量的回收工作，不能太耗时

-`onDestory`: Activity即将销毁，Activity生命周期的最后一个回掉，可以做些回收工作和最终的资源释放。

>Activity生命周期切换过程说明

1. 针对一个特定的Activity，第一次启动，回调如下：
	onCreate → onStart → onResume 。

2. 当用户打开新的Activity或者切换到桌面的时候，回调如下：
	onPause → onStop 。**如果新Activity采用透明主题，当前Activity不会调用onStop** 。

3. 当用户再次回到原Activity时，回调如下：
	onRestart → onStart → onResume 。

4. 当用户按Back键回退时，回调如下：
	onPause → onStop → onDestroy 。

5. 当Activity被系统回收后再次打开，生命周期方法回调过程和1一样，	**注意只是生命周期方法一样，不代表所有过程都一样**

6. 从整个生命周期来说，onCreate和onDestory是配对的，分别标识着	Activity的创建和销毁，并且只可能有一次调用。从Activity是否可见来说，onStart和onStop是配对的，随着用户的操作或者设备屏幕的点亮和熄灭，这两个方法可能被调用多次；从Activity是否在前台来说，onResume和onPause是配对的，随着用户操作或者设配屏幕的点亮和熄灭，这两个方法有可能被调用多次。

