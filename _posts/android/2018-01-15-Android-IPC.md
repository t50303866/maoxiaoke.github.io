---
layout: post
title: "Android IPC"
date: 2018-01-15 22:00:00 +0800
categories: Android笔记
tag: Android
---
* content
{:toc}

### IPC简介
IPC是Inter-Process Communication的缩写，含义位进程间通信或者跨进程通信，是指两个进程之间进行数据交换的过程。

- 线程是CPU调度的最小单位，同时线程是一种有限的系统资源。
- 进程一般指一个执行单元，在PC和移动设备上指一个程序或者一个应用。一个进程可以包含多个线程，进程和线程是包含与被包含的关系。

### 多进程模式
四大组件在AndroidMenifest中指定android:process属性.

SecendActivity和ThirdActivity的android:process属性分别为“:remote”和“com.tzl.chapter_2.remote”，两种方式的区别
1. “：”的含义是指要在当前的进程名前面附加上当前的包名，这是一种简写的方法，而对于ThirdActivity中的声明方式，它是一种完整的命名方式，不会附件包名信息；
2. 进程名以“：”开头的进程属于当前应用的私有进程，其他应用的组件不可以和它跑在同一个进程中，而进程名不以“：”开头的进程属于全局进程，其他应该可以通过ShareUID方式可以和它袍子同一个进程中。

> 当应该开启了多进程之后，各种奇怪的现象都出现了
1. 静态成员和单利模式完全失效
2. 线程同步机制完全失效
3. SharedPreferences的可靠性下降
4. Application会多次创建

## Android中的IPC方式

### 使用Bundle
Activity、Servicce、Receiver都是支持在Intent中传递Bundle数据，ContentProvider天生支持跨进程访问。
### 使用文件共享
通过文件交换数据很好使用，除了可以交换一些文本信息外，我们还可以序列化一个对象到文件系统中的同时从另一个进程中恢复这个对象。
> 有局限性：并非读写问题。文件共享方式适合在对数据同步要求不高的进程之间进行通信，并且要妥善处理并发读/写的问题。
### 使用Messenger
Messenger是一种轻量级的IPC方案，它的底层实现是AIDL。

1. 服务端进程

	我们需要在服务端创建一个Service来处理客户端的连接请求，同时创建一个Handler并通过它来创建一个Messenger对象，然后在Service的onBind中返回这个Messenger对象底层的Binder即可。

2. 客户端进程

	首先绑定服务端的Service，绑定成功后服务端返回的IBinder对象创建一个Messenger，通过这个Messenger就可以向服务器发送消息了，发送消息类型为Message对象。如果需要服务端能够回应客户端，就和服务端一样，我们还需要创建一个Handler并创建一个新的Messenger，并把这个Messenger对象通过Message的replyTo参数传递给服务端，服务端通过这个replyTo参数就可以回应客户端。

> 在Messenger中进行数据传递必须将数据放入Message中。Messenger是以串行的方式处理客户端发来的消息

### 使用AIDL
