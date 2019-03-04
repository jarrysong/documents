
[Android 必须知道2018年流行的框架库及开发语言](https://blog.csdn.net/csdn_aiyang/article/details/56016649)

[2017年Android百大框架排行榜](https://www.cnblogs.com/jincheng-yangchaofan/articles/7018780.html)

[2018 年初值得关注的 25 个新 Android 库和项目](https://www.oschina.net/translate/25-new-android-libraries-and-projects-2018)

[Android 15个流行网络框架](https://www.cnblogs.com/onone/articles/6606448.html)

![image.png](https://upload-images.jianshu.io/upload_images/1128757-e004faf45b701a9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.图片加载库
- Universal-Image-Loader，早期广泛被用的一个可重复使用的仪器为异步图像加载、缓存、显示。作者已经停止维护。
- Picasso，谐音"毕加索",听起来就很艺术，是 Square开源的项目，主导者是是Android大神JakeWharton。
- Glide，是google员工在Picasso基础上进行优化，总体比Picasso更优秀，在Google很多项目在用。
- Fresco，FaceBook的明星项目，也是去年最火的项目之一，匿名共享缓存等机制保证低端机表现极佳，但是源代码基于C/C++。
![image.png](https://upload-images.jianshu.io/upload_images/1128757-55c7802b7895632e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.图片处理
- Picasso-transformations：基于Picasso的图片处理库，包括图片裁剪等
- Glide-transformations：基于Glide的transformation库，拥有裁剪，着色，模糊，滤镜等多种转换效果
- Android-gpuimage 

#### 3.异步分发通信库
- EventBus ，是一个发布、订阅的轻量级事件总线框架，基于观察者模式的实现的线程通信框架。
- RxJava， 一个在 Java VM 上使用可观测的序列来组成异步的、基于观察者模式的实现的库。
- RxAndroid，函数响应式编程， 把 RxJava 带到 Android 环境中。很多时候，编写 Android 程序，你也可以看成是数据的处理和流动，换一种思想编程，曾经看起来很棘手的问题，瞬间就很优雅的解决了，相信你会被这种build模式的开发会越来越爱。
- RxBinding，是 Jake Wharton 的一个开源库，它提供了一套在 Android 平台上的基于 RxJava的 Binding API。所谓 Binding，就是类似设置 OnClickListener 、设置 TextWatcher 这样的注册绑定对象的 API。
- Otto

#### 4.注入注解框架
- Dagger2，与Spring 的IOC差不多吧。这个框架它的好处是它没有采用反射技术（Spring是用反射的）,而是用预编译技术，因为基于反射的DI非常地耗用资源（空间，时间）。
- Butterknife，出自大神JakeWharton，绑定视图和回调字段和方法。例如，减少了findViewById()的繁琐操作。
- AndroidAnotations
- RoboGuice：方便findViewById在XML中查找一个View，并将其强制转换到所需类型

#### 5.UI框架
- BaseRecyclerViewAdapterHelper使用——RecyclerView万能适配器。
- PinnedSectionItemDecoration：强大的粘性标签库
- EasyRefreshLayout：    轻松实现下拉刷新和上拉更多
- EasySwipeMenuLayout：仿IOS侧滑删除
- SmartRefreshLayout：下拉刷新、上拉加载、二级刷新、淘宝二楼、RefreshLayout、OverScroll，Android智能下拉刷新框架，支持越界回弹、越界拖动，具有极强的扩展性，集成了几十种炫酷的Header和 Footer。 也吸取了现在流行的各种刷新布局的优点，包括谷歌官方的 SwipeRefreshLayout，其他第三方的 Ultra-Pull-To-Refresh、TwinklingRefreshLayout 。还集成了各种炫酷的 Header 和 Footer。
- android-gif-drawable：用于在Android上显示动画GIF的视图和Drawable。
- PhotoView：用于在Android上通过各种触摸手势实现支持缩放的图片的框架。

#### 6.网络请求库
- okhttp，在Android开发中，它已经成为眼下最火的http请求框架了。
- Retrofit，与okhttp共同出自于Square公司，retrofit就是对okhttp做了一层封装。把网络请求都交给给了Okhttp，我们只需要通过简单的配置就能使用retrofit来进行网络请求了，其主要作者也是Android大神JakeWharton。
- Android Async HTTP
- AndroidAsync
- Volley

#### 7.日志打印库
- logger，简单,漂亮的android和强大的记录器。
- Hugo
 -Timber

#### 8.权限请求库
- RxPermissions，API23以上Android 6.0项目分为普通权限和危险权限，该库在项目运行时动态进行权限请求，支持RxJava2。

#### 9.SQLite数据库
- LitePal，一个Android库,使得开发人员使用SQLite数据库非常容易。
- OrmLite(Object Relational Mapping)
- SugarORM  
- GreenDAO
- ActiveAndroid
- SQLBrite
- Realm  相关内容，查看官方文档

#### 10.性能优化
- 内存泄漏检测（LeakCanary）
- 奔溃报告（ARCA Application Report For Android）：当你的程序奔溃时，会给开发者发送一个Report。即使程序没有奔溃，也会发送错误Report

#### 11.调试框架
- Stetho(Facebook开发的工具)

#### 12.测试框架
- Mockito
- Robotium
- Robolectric

#### 13.后台处理
- Tape：一个轻快的、事务性的、基于文件的FIFO的库
- Android Priority JobQueue

#### 14.图标
- WilliamChart：画图控件
- HelloCharts：一个用来生成统计图表的三方库，目前支持折线图、柱状图和饼状图等常见图表。支持缩放、滑动和动画效果。是一个非常实用的Android平台的图标库
- MPAndroidCharts：支持线状图、柱状图、散点图、烛状图、气泡图、饼状图和蜘蛛网状图，支持缩放、拖动(平移)、选择和动画，适用于 Android 2.2 ( API 8 ) 和以上

#### 16.缓存
- DiskLruCache(Lru磁盘缓存)

#### 16.网络解析
- Gson
- Jackson
- Fastjson ：Alibaba开发的一个框架
- HtmlParser
- Jsoup：是一款比较好的Java版HTML解析器

#### 17.新技术语言
- Kotlin
- React Native
- flutter
- Sky
- Hybrid
- Python


