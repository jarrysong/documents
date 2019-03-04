
#### 1. 数据库的操作类型有哪些，如何导入外部数据库？（Ricky）

##### 1）使用数据库的方式有哪些？
- （1）openOrCreateDatabase(String path);
- （2）继承SqliteOpenHelper类对数据库及其版本进行管理(onCreate,onUpgrade)
当在程序当中调用这个类的方法getWritableDatabase()或者getReadableDatabase();的时候才会打开数据库。如果当时没有数据库文件的时候，系统就会自动生成一个数据库。


##### 2）操作的类型：增删改查CRUD
直接操作SQL语句：SQliteDatabase.execSQL(sql);
面向对象的操作方式：SQLiteDatabase.insert(table, nullColumnHack, ContentValues);


##### 3）如何导入外部数据库？
一般外部数据库文件可能放在SD卡或者res/raw或者assets目录下面。
写一个DBManager的类来管理，数据库文件搬家，先把数据库文件复制到”/data/data/包名/databases/”目录下面，然后通过db.openOrCreateDatabase(db文件),打开数据库使用。

我上一个项目就是这么做的，由于app上架之前就有一些初始数据需要内置，也会碰到数据的升级等问题，我是这么做的…… 同时我碰到最有意思的问题就是关于数据库并发操作的问题，比如：多线程操作数据库的时候，我采取的是封装使用互斥锁来解决……



#### 2. 是否使用过本地广播，和全局广播有什么差别？（Ricky）
- 1）正在发送的广播不会脱离应用程序，不用担心app的数据泄露；
- 2）其他的程序无法发送到我的应用程序内部，不担心安全漏洞。（比如：如何做一个杀不死的服务---监听火的app 比如微信、友盟、极光的广播，来启动自己。）
- 3）	发送本地广播比发送全局的广播高效。（全局广播要维护的广播集合表 效率更低。全局广播，意味着可以跨进程，就需要底层的支持。）

- 4）本地广播不能用静态注册。----静态注册：可以做到程序停止后还能监听。
使用：
```
（1）注册
LocalBroadcastManager.getInstance(this).registerReceiver(new XXXBroadCastReceiver(), new IntentFilter(action));
（2）取消注册：
LocalBroadcastManager.getInstance(this).unregisterReceiver(receiver)
```


#### 3. 是否使用过 IntentService，作用是什么， AIDL 解决了什么问题？ (小米) （Ricky）
如果有一个任务，可以分成很多个子任务，需要按照顺序来完成，如果需要放到一个服务中完成，那么使用IntentService是最好的选择。

一般我们所使用的Service是运行在主线程当中的，所以在service里面编写耗时的操作代码，则会卡主线程会ANR。为了解决这样的问题，谷歌引入了IntentService.

###### IntentService的优点：
- (1)它创建一个独立的工作线程来处理所有一个一个intent。
- (2)创建了一个工作队列，来逐个发送intent给onHandleIntent()
- (3)不需要主动调用stopSelf()来结束服务，因为源码里面自己实现了自动关闭。
- (4)默认实现了onBind()返回的null。
- (5)默认实现的onStartCommand()的目的是将intent插入到工作队列。
- 
总结：使用IntentService的好处有哪些。
- 首先，省去了手动开线程的麻烦；
- 第二，不用手动停止service；
- 第三，由于设计了工作队列，可以启动多次---startService(),但是只有一个service实例和一个工作线程。一个一个顺序地执行。

###### AIDL 解决了什么问题？
AIDL的全称：Android Interface Definition Language，安卓接口定义语言。

由于Android系统中的进程之间不能共享内存，所以需要提供一些机制在不同的进程之间进行数据通信。

- 远程过程调用：RPC—Remote Procedure Call。  安卓就是提供了一种IDL的解决方案来公开自己的服务接口。
- AIDL:可以理解为双方的一个协议合同。双方都要持有这份协议---文本协议 xxx.aidl文件（安卓内部编译的时候会将aidl协议翻译生成一个xxx.java文件---代理模式：Binder驱动有关的，Linux底层通讯有关的。）

在系统源码里面有大量用到aidl，比如系统服务。
电视机顶盒系统开发。你的服务要暴露给别的开发者来使用。
讲解Binder机制。




#### 4. Activity、 Window、 View 三者的差别， fragment 的特点？（360）（Ricky）
##### (1)Activity、 Window、 View 三者如何协同显示界面的。
考点：显示的过程(view 绘制流程)源码的熟悉度。
- Activity剪窗花的人（控制的）；
- Window窗户（承载的一个模型）；
- View窗花（要显示的视图View）；
- LayoutInflater剪刀---将布局（图纸）剪成窗花。

##### (2)fragment 的特点？（你用fragment有没有领略到一些乐趣，或者有没有踩过什么坑？）
fragment的设计主要是把Activity界面包括其逻辑打碎成很多个独立的模块，这样便于模块的重用和更灵活地组装呈现多样的界面。

- 1）	Fragment可以作为Activity界面的一个部分组成；
- 2）	可以在一个Activity里面出现多个Fragment，并且一个fragment可以在多个Activity中使用；
- 3）	在Activity运行中，可以动态地添加、删除、替换Fragment。
- 4）	Fragment有自己的生命周期的，可以响应输入事件。

踩过的坑：
- 1.重叠；
- 2.注解newAPI（兼容包解决）；
- 3.Setarguement()初始化数据;
- 4.不能在onsave...（）方法后，commit; 
- 5.入栈出栈问题; --事务。像Activity跳转一样的效果，同时返回的时候还能回到之前的页面(fragment)并且状态都还在。
- 6.replace(f1,f2)严重影响生命周期。解决方案：add()+show+hide

#### 5. 描述一次网络请求的流程（新浪）（Jason）
HTTP通信机制是在一次完整的HTTP通信过程中，Web浏览器与Web服务器之间将完成下列7个步骤：
###### 1. 建立TCP连接：
在HTTP工作开始之前，Web浏览器首先要通过网络与Web服务器建立连接，该连接是通过TCP来完成的，该协议与IP协议共同构建Internet，即著名的TCP/IP协议族，因此Internet又被称作是TCP/IP网络。HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能进行更高层协议的连接，因此，首先要建立TCP连接，一般TCP连接的端口号是80。
######  2. Web浏览器向Web服务器发送请求命令： 
一旦建立了TCP连接，Web浏览器就会向Web服务器发送请求命令。例如：GET/sample/hello.jsp HTTP/1.1。
######  3. Web浏览器发送请求头信息 ：
浏览器发送其请求命令之后，还要以头信息的形式向Web服务器发送一些别的信息，之后浏览器发送了一空白行来通知服务器，它已经结束了该头信息的发送。
######  4. Web服务器应答 ：
客户机向服务器发出请求后，服务器会客户机回送应答， HTTP/1.1 200 OK ，应答的第一部分是协议的版本号和应答状态码。
######  5. Web服务器发送应答头信息： 
正如客户端会随同请求发送关于自身的信息一样，服务器也会随同应答向用户发送关于它自己的数据及被请求的文档。
###### 6. Web服务器向浏览器发送数据： 
Web服务器向浏览器发送头信息后，它会发送一个空白行来表示头信息的发送到此为结束，接着，它就以Content-Type应答头信息所描述的格式发送用户所请求的实际数据。
######  7. Web服务器关闭TCP连接 ：
一般情况下，一旦Web服务器向浏览器发送了请求数据，它就要关闭TCP连接，然后如果浏览器或者服务器在其头信息加入了这行代码：Connection:keep-alive；TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽。




#### 6. Handler、 Thread 和 HandlerThread 的差别（小米）（Jason）
Handler会关联一个单独的线程和消息队列，Handler默认关联主线程，如果要在其他线程执行，可以使用HandlerThread。
HandlerThread继承于Thread，所以它本质就是个Thread。与普通Thread的差别就在于，主要的作用是建立了一个线程，并且创立了消息队列，有来自己的looper,可以让我们在自己的线程中分发和处理消息。


#### 7. 低版本 SDK 实现高版本 api（小米）（Ricky）
两种情况：
- 1）	一般很多高版本的新的API都会在兼容包里面找到替代的实现。比如fragment。
Notification，在v4兼容包里面有NotificationCompat类。5.0+出现的backgroundTint，minSdk小于5.0的话会包检测错误，v4兼容包DrawableCompat类。
- 2）	没有替代实现就自己手动实现。比如：控件的水波纹效果—第三方实现。
或者直接在低版本去除这个效果。
- 3）	补充:如果设置了minSDK但是代码里面使用了高版本的API，会出现检测错误。需要在代码里面使用声明编译检测策略，比如：@SuppressLint和@TargetApi注解提示编译器编译的规则。@SuppressLint是忽略检测；@TargetApi=23，会根据你函数里面使用的API，严格地匹配SDK版本，给出相应的编译错误提示。
- 4）	为了避免位置的错误，最好不要使用废弃api。（一般情况下不会有兼容性问题，后面可能会随时删除这个API方法；性能方面的问题。）
- 5）http://chinagdg.org/2016/01/picking-your-compilesdkversion-minsdkversion-targetsdkversion/

#### 8. launch mode 应用场景（百度、小米、乐视）（Ricky）
栈：先进后出
- 标准模式
- SingleTop：使用场景：浏览器的书签；通讯消息聊天界面。
- SingleTask：使用场景：某个Activity当做主界面的时候。
- SingleInstance：使用场景：比如浏览器BrowserActivity很耗内存，很多app都会要调用它，这样就可以把该Activity设置成单例模式。比如：闹钟闹铃。

#### 9. touch 事件传递流程（小米）（Ricky）
#### 10. view 绘制流程（百度）（Ricky）
- Measure：测量，测量自己。如果是ViewGroup就需要测量里面的所有childview.
测量的结果怎么办？

```
setMeasuredDimension(resolveSizeAndState(maxWidth, widthMeasureSpec, childState), heightSizeAndState);//设置自己的大小。
```
- Layout: 摆放，把自己摆放在哪个位置。如果是ViewGroup就需要发放里面的所有childview.
	怎么去具体摆放呢？
- Draw:绘制
```
        /*
         * Draw traversal performs several drawing steps which must be executed
         * in the appropriate order:
         *
         *      1. Draw the background
         *      2. If necessary, save the canvas' layers to prepare for fading
         *      3. Draw view's content
         *      4. Draw children
         *      5. If necessary, draw the fading edges and restore layers
         *      6. Draw decorations (scrollbars for instance)
         */
```

#### 11. 什么情况导致内存泄漏（美团）（Ricky）
#### 12. ANR 定位和修正（Ricky）

#### 13. 什么情况导致 oom（乐视、美团）（Ricky）
http://blog.csdn.net/qq_34378183/article/details/52785819

OOM产生的原因：内存不足，android系统为每一个应用程序都设置了一个硬性的条件：DalvikHeapSize最大阀值64M/48M/24M.如果你的应用程序内存占用接近这个阀值，此时如果再尝试内存分配的时候就会造成OOM。

- 1)内存泄露多了就容易导致OOM
- 2)大图的处理。压缩图片。平时开发就要注意对象的频繁创建和回收。
- 3）可以适当的检测：ActivityManager.getMemoryClass()可以用来查询当前应用的HeapSize阀值。可以通过命名adb shellgetProp | grep dalvik.vm.heapxxxlimit查看。

##### 如何避免内存泄露：
###### 1.减小对象的内存占用：
- a)	使用更加轻量级的数据结构：
考虑适当的情况下替代HashMap等传统数据结构而使用安卓专门为手机研发的数据结构类ArrayMap/SparseArray。SparseLongMap/SparseIntMap/SparseBoolMap更加高效。
HashMap.put(string,Object);Object o = map.get(string);会导致一些没必要的自动装箱和拆箱。
- b)	适当的避免在android中使用Enum枚举，替代使用普通的static常量。（一般还是提倡多用枚举---软件的架构设计方面；如果碰到这个枚举需要大量使用的时候就应该更加倾向于解决性能问题。）。
- c)	较少Bitmap对象的内存占用。
使用inSampleSize:计算图片压缩比例进行图片压缩，可以避免大图加载造成OOM; decodeformat：图片的解码格式选择，ARGB_8888/RGB_565/ARGB_4444/ALPHA_8,还可以使用WebP。
- d)	使用更小的图片
资源图片里面，是否存在还可以继续压缩的空间。

###### 2.内存对象的重复利用:
使用对象池技术，两种：
- 1.自己写；
- 2.利用系统既有的对象池机制。比如LRU(Last Recently Use)算法。

例子:
- a)	ListView/GridView源码可以看到重用的情况ConvertView的复用。RecyclerView中Recycler源码。
- b)	Bitmap的复用<br>
Listview等要显示大量图片。需要使用LRU缓存机制来复用图片。
- C)	避免在onDraw方法里面执行对象的创建，要复用。避免内存抖动。
- D) 常见的java基础问题---StringBuilder等

###### 3.避免对象的内存泄露：
###### 4.使用一些内存的优化策略：
看文档






#### 14. Android Service 与 Activity 之间通信的几种方式（Ricky）
- 1）通过Binder
- 2）通过广播


#### 15. Android 各个版本 API 的区别（Ricky）
把几个关键版本的特性记住：3.0/4.0、4.4、5.0、6.0/7.0

#### 16. 如何保证一个后台服务不被杀死,比较省电的方式是什么？（百度）（Ricky）
看文档

#### 17. Requestlayout， onlayout， onDraw， DrawChild 区别与联系（猎豹）（Ricky）
- RequestLayout()方法：会导致调用Measure()方法和layout()。将会根据标志位判断是否需要onDraw();
- onLayout()：摆放viewGroup里面的子控件
- onDraw()：绘制视图本身；（ViewGroup还需要绘制里面的所有子控件）
- drawChild(): 重新回调每一个子视图的draw方法。child.draw(canvas, this, drawingTime);



#### 18. invalidate()和postInvalidate()的区别及使用（百度）（Ricky）
invalidate()得在UI线程中被调动，在工作者线程中可以通过Handler来通知UI线程进行界面更新。

- invalidate()：在主线程当中刷新；
- postInvalidate()：在子线程当中刷新；其实最终调用的就是invalidate，原理依然是通过工作线程向主线程发送消息这一机制。
```
    public void postInvalidate() {
        postInvalidateDelayed(0);
    }
    public void postInvalidateDelayed(long delayMilliseconds) {
        // We try only with the AttachInfo because there's no point in invalidating
        // if we are not attached to our window
        final AttachInfo attachInfo = mAttachInfo;
        if (attachInfo != null) {
            attachInfo.mViewRootImpl.dispatchInvalidateDelayed(this, delayMilliseconds);
        }
    }

    public void dispatchInvalidateDelayed(View view, long delayMilliseconds) {
        Message msg = mHandler.obtainMessage(MSG_INVALIDATE, view);
        mHandler.sendMessageDelayed(msg, delayMilliseconds);
    }
public void handleMessage(Message msg) {
            switch (msg.what) {
            case MSG_INVALIDATE:
                ((View) msg.obj).invalidate();
                break;
```

#### 19. Android 动画框架实现原理（Ricky）
传统的动画框架：View.startAnimation();

弊端：移动后不能点击。原因？跟实现机制有关系。

所有的透明度、旋转、平移、缩放动画，都是在view不断刷新调用draw的情况下实现的。
调用的canvas.translate(xxx),canvas.scaleX(xxx)…. Xxx:matrix像素矩阵来控制动画的数据。记得看源码，结合多只缩放的demo看源码。


#### 20. Android 为每个应用程序分配的内存大小是多少？（美团）（Ricky）
Android应用程序的默认最大内存值为16M，不同的手机版本和型号有所不同(我的三星galaxy s3的是256M)

看具体的手机平台，常见的有：64M/32M等


#### 21. Android View 刷新机制（百度、美团）（Ricky）

#### 22. LinearLayout 对比 RelativeLayout（百度）（Ricky）
性能对比：LinearLayout的性能要比RelativeLayout好。

因为RelativeLayout会测量两次。而默认情况下（没有设置weight）LinearLayout只会测量一次。

为什么RelativeLayout会测量两次？首先RelativeLayout中的子view排列方式是基于彼此依赖的关系，而这个依赖可能和布局中view的顺序无关，在确定每一个子view的位置的时候，就需要先给每一个子view排一下序。又因为RelativeLayout允许横向和纵向相互依赖，所以需要横向纵向分别进行一次排序测量。



#### 23. 优化自定义 view（百度、乐视、小米）（Ricky）
http://www.jcodecraeer.com/a/anzhuokaifa/developer/2013/0203/834.html
- 1）减少在onDraw里面大量计算和对象创建和大量内存分配。
- 2）应该尽量少用invalidate()次数。
- 3）view里面耗时的操作layout。减少requestLayout（）避免让UI系统重新遍历整棵树。Mearsure。
- 4）如果你有一个很复杂的布局，不如将这个复杂的布局直接使用你自己的写的ViewGroup来实现。减少了一个树的层次关系 全部都是自己测量和layout，达到优化的目的。（Facebook就经常这么干）



#### 24. ContentProvider（乐视）（Ricky）
提示：跨进程通信。进程之间进行数据交互共享。；源码来一剁。

#### 25. fragment 生命周期（Ricky）
#### 26. volley 解析（美团、乐视）（Ricky）
#### 27. Android Glide 源码解析（Ricky）
#### 28. Android 属性动画特性（乐视、小米）（Ricky）
#### 29. Handler 机制及底层实现（Danny）
#### 30. Binder 机制及底层实现（Danny）