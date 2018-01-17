---
layout: post
title: "Android面试题"
date: 2018-01-08 16:00:00 +0800
categories: Android笔记
tag: 面试
---
* content
{:toc}

### 1. String、StringBuffer和StringBuilder区别
简单说：就是一个变量和常量的关系。StringBuffer对象的内容可以修改；而String对象一旦产生就不可以被修改（不可改变的字符串具有一个很大的优点:编译器可以把字符串设为共享的），重新赋值其实是两个对象。StringBuffer是线程安全的。StringBuilder不是线程安全的，是StringBuffer的简易版，在单线程中使用更快。

### 2. 线程有几种状态，分别是哪些？（调用run()和start()的区别）
	- 新建状态（New）：新创建了一个线程对象。

	- 就绪状态（Runnable）：线程对象创建后，其他线程调用了该对象的start()方法。该状态的线程位于“可运行线程池”中，变的可运行，只等待获取CPU的使用权限。即在就绪状态的进程除CPU之外，其它的运行所需资源都已全部获得。

	- 运行状态（Running）：就绪状态的线程获取了CPU，执行程序代码，开始运行run函数当中的代码。

	- 阻塞状态（Blocked）：阻塞状态是线程因为某种原因放弃CPU使用权，暂时体制运行。直到线程进入就绪状态，才有机会

>阻塞的情况分三种
>- 等待阻塞：运行的线程执行wait()方法，该线程会释放占用的所有资源，JVM会把该线程放入“等待池”中，进入这个状态后，是不能自动唤醒的，必须依靠其他线程调用notify()或notifyAll()方法才能被唤醒
>- 同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放进“锁池”中
>- 其他阻塞：运行的线程执行sleep()或者join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。

	- 死亡状态（Dead）：线程执行完了或者因异常退出了run()方法，该线程结束生命周期。


> start()启动线程，使线程处于就绪状态，并没有运行。然后此调用方法run()来运行操作，run()方法结束，此线程终止。然后CPU再调用其他线程

### 3. sleep()与wait()的区别
对于sleep()方法是属于Thread类中的。而wait()方法，则属于Object类。

sleep()方法导致了程序暂停执行指定的时间，但是他的监控状态依然保持着，当指定的时间到了又会自动恢复运行状态。在调用sleep()方法的过程中，线程不会释放对象锁。而且sleep()只能自己到点了醒来，不能被唤醒。

wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()或notifyAll方法后本线程才进入对象锁定池，准备获取对象锁进入运行状态。

### 4. 常用的排序算法时间复杂度，实现思路
- ***冒泡排序***：通过交换相邻的两个数变成小数在前大数在后，这个每次遍历后，最大的数就“沉”到最后面了。重复N次即可以使数组有序。平均时间复杂度O(n^2^)，最好O(n),最坏O(n^2^)。，空间复杂度O(1)。

- ***选择排序***：数组分成有序区和无序区，初始时整个数组都是无序区，每次遍历都从无序区选择一个最小的元素直接放在有序区最后，直到排序完成。平均时间复杂度O(n^2^)，最好O(n),最坏O(n^2^)。，空间复杂度O(1)。

- ***插入排序***：每次将一个待排序的数组元素，插入到前面已排序的序列中这个元素应该在的位置，直到数据插入完成。平均时间复杂度O(n^2^)，最好O(n),最坏O(n^2^)。，空间复杂度O(1)。

- ***快速排序***：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。平均时间复杂度O(n*log~2~N)，最好O(n*log~2~N),最坏O(n^2^)。，空间复杂度O(n*log~2~N) ~ O(n)。

- ***希尔排序***：先将整个待排序元素序列分成若干个子序列（由相隔某个“增量”的元素组成的）分别进行直接插入排序，然后依次缩减增量再进行排序，待“增量”为1时，进行直接插入排序。平均时间复杂度O(n^1.3^)，最好O(n),最坏O(n^2^)。，空间复杂度O(1)。

- ***归并排序***：当一个数组左边有序，右边也有序，合并这两个有序数组就完成了排序。其实现原理，递归下去，合并上来就是归并排序。平均时间复杂度O(n*log~2~N)，最好O(n*log~2~N),最坏O(n*log~2~N)。，空间复杂度O(n)。

- ***堆排序***：

- ***基数排序***:

### 5. 广播如何调用？
1. 广播接收者BroadcastReceiver通过Binder机制向AMS（Activity Mananger Service）进行注册；
2. 广播发送者通过binder机制向AMS发送广播；
3. AMS查找符合相应条件（IntentFilter/Permission等）的BroadcastReceiver，将广播发送到BroadcastReceiver（一般情况下是Activity）相应的消息队列中；
4. 消息循环执行拿到此广播，回调BroadcastReceiver中的onReceive()方法。

### 6. 什么是ANR，如何避免它？

在Android上，如果你的应用程序有一段时间相应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应用程序无响应（ANR：Application Not Responding）对话框。
Activity的最长执行时间是5s，BroadcastReceiver的最长执行时间则是10s，Android应用程序通常是运行在一个单独的线程（例如，main）里，因此，运行在主线程里的任何方法都尽可能少做事情。潜在耗时或高耗时的操作放到子线程中来完成。主线程为子线程提供一个Handler，以便完成时能够提交给主线程。

### 7. Handler消息机制

开发中，当我们再子线程做了一些操作后需要更新UI，由于Android不允许子线程中访问UI控件，所以我们一般都会使用Handler来实现。

Handler的机制需要MessageQueue、Looper和Message的支持。他们在消息机制中各扮演了不同的角色

Handler：负责消息的发送和接收处理

MessageQueue：消息队列，一个消息存储单位，经常需要进行增减，内部使用的单链表的结构

Looper：消息循环。会不停地从MessageQueue中获取消息，如果有新消息就会立刻处理，否则就一直阻塞在那里

Message：消息载体

**执行流程**
1. 首先调用Looper.prepare()方法，会创建一个Looper实例，该实例包含一个MessageQueue，并将该实例保存在当前线程中ThreadLocal
2. 调用Looper.loop()开始消息循环，不断地向MessageQueue中读取消息，并调用msg.target.dispatchMessage(msg)；来处理消息
3. 构建Handler的时候，会先获取到当前Handler所在线程的Looper并得到其中的MessageQueue
4. 使用Handler发送消息的时候，会将一个Message保存到当前线程Looper中的MessageQueue
5. 当Looper.loop()获取到消息的时候msg.target.dispatchMessage(msg)来处理消息，其实msg.target = handler。也就是调用Handler的dispatchMessage来处理
6. Handler的dispatchMessage最终回去调用handlerMessage()方法。

> Handler的handler在哪条线程执行，取决于构建Handler时所使用的是哪条线程保存的Looper，因为handlerMessage其实是Looper去调用的。

### 8. listview recycleview分页加载实现思路

通过监听Listview Or Recycleview的滑动事件，判读是否达到最后一条，如果达到最后一条则加载新数据，刷新adapter

### 9. 加载大图片时如何防止内存溢出
- 只得到图片的宽高 ： BitmapFactory.Options这个类，有个字段叫做 inJustDecodeBounds 。如果我们把它设为true，那么BitmapFactory.decodeFile(String path,Options opt)并不会真的返回一个Bitmap，它仅仅会把它的宽高给你，并不会占用太多内存。
` BitmapFactory.Options options = new BitmapFactory.Options();
  options.inJustDecodeBounds = true;
  Bitmap bmp = BitmapFactory.decodeFile(path, options);    /* 这里返回的bmp是null */`

- 获取需要绘制的原图 ： 通过BitmapFactory.decodeStream方法，等比例创建出一个bitmap
 ` InputStream is = this.getResources().openRawResource(R.drawable.pic1);
  BitmapFactory.Options options=new BitmapFactory.Options();
  options.inJustDecodeBounds = false;
  options.inSampleSize = 10;   //width，hight设为原来的十分一
  Bitmap btp =BitmapFactory.decodeStream(is,null,options);`
