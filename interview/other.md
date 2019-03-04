
#### 1.	死锁的四个必要条件（Jason）
- 1.互斥条件，即某个资源在一段时间内只能由一个线程占有，不能同时被两个或两个以上的线程占有

- 2.不可抢占条件，线程所获得的资源在未使用完毕之前，资源申请者不能强行地从资源占有者手中夺取资源，而只能由该资源的占有者线程自行释放

- 3.占有且申请条件，线程至少已经占有一个资源，但又申请新的资源；由于该资源已被另外线程占有，此时该线程阻塞；但是，它在等待新资源之时，仍继续占用已占有的资源。

- 4.循环等待条件，存在一个线程等待序列{P1，P2，...，Pn}，其中P1等待P2所占有的某一资源，P2等待P3所占有的某一源，......，而Pn等待P1所占有的的某一资源，形成一个线程循环等待环

解决死锁的办法：加锁顺序，死锁检测   
方式：
- 1.避免使用嵌套枷锁；
- 2.让线程按照顺序执行；

#### 2. 常见编码方式； utf-8 编码中的中文占几个字节；数字几个字节（Jason）
一个utf8数字占1个字节，一个utf8英文字母占1个字节，少数是汉字每个占用3个字节，多数占用4个字节。

#### 3.	实现一个 Json 解析器(可以通过正则提高速度) （Jason）
```java
String json = "{name:\"jason\",father:\"jason\",age:18}";
		
//name:"jason"
//age:18
//\"\\w+\" 字符串属性
Pattern p = Pattern.compile("\\w+:(\"\\w+\"|\\d*)");
Matcher m = p.matcher(json);
while(m.find()){
	String text = m.group();
	int dotPos= text.indexOf(":");
	String key = text.substring(0, dotPos);
	String value = text.substring(dotPos+1, text.length());
	//替换字符串的开始结束的双引号
	value = value.replaceAll("^\\\"|\\\"$", "");
	System.out.println(key);
	System.out.println(value);
}
```
#### 4. Android App 的设计架构： MVC,MVP,MVVM 与架构经验谈（搜狐）（Jason）

#### 5. 写出观察者模式的代码（Jason）
触发联动
#### 6. [TCP 的 3 次握手和四次挥手](http://blog.csdn.net/whuslei/article/details/6667471)； TCP 与 UDP 的区别（Jason）
建立TCP需要三次握手才能建立，而断开连接则需要四次握手。（工具：Wireshark）


![TCP的三次握手.png](https://upload-images.jianshu.io/upload_images/1128757-b6ed444c8d6d74b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 三次握手：

TCP是面向连接的，无论哪一方向另一方发送数据之前，都必须先在双方之间建立一条连接。在TCP/IP协议中，TCP协议提供可靠的连接服务，连接是通过三次握手进行初始化的。三次握手的目的是同步连接双方的序列号和确认号并交换 TCP窗口大小信息。

- 第一次握手：建立连接。客户端发送连接请求报文段，将SYN位置为1，Sequence Number为x；然后，客户端进入SYN_SEND状态，等待服务器的确认；
- 第二次握手：服务器收到SYN报文段。服务器收到客户端的SYN报文段，需要对这个SYN报文段进行确认，设置Acknowledgment Number为x+1(Sequence Number+1)；同时，自己自己还要发送SYN请求信息，将SYN位置为1，Sequence Number为y；服务器端将上述所有信息放到一个报文段（即SYN+ACK报文段）中，一并发送给客户端，此时服务器进入SYN_RECV状态；
- 第三次握手：客户端收到服务器的SYN+ACK报文段。然后将Acknowledgment Number设置为y+1，向服务器发送ACK报文段，这个报文段发送完毕以后，客户端和服务器端都进入ESTABLISHED状态，完成TCP三次握手。

##### 四次分手：

当客户端和服务器通过三次握手建立了TCP连接以后，当数据传送完毕，肯定是要断开TCP连接的啊。那对于TCP的断开连接，这里就有了神秘的“四次分手”。

- 第一次分手：主机1（可以使客户端，也可以是服务器端），设置Sequence Number和Acknowledgment Number，向主机2发送一个FIN报文段；此时，主机1进入FIN_WAIT_1状态；这表示主机1没有数据要发送给主机2了；
- 第二次分手：主机2收到了主机1发送的FIN报文段，向主机1回一个ACK报文段，Acknowledgment Number为Sequence Number加1；主机1进入FIN_WAIT_2状态；主机2告诉主机1，我“同意”你的关闭请求；
- 第三次分手：主机2向主机1发送FIN报文段，请求关闭连接，同时主机2进入LAST_ACK状态；
- 第四次分手：主机1收到主机2发送的FIN报文段，向主机2发送ACK报文段，然后主机1进入TIME_WAIT状态；主机2收到主机1的ACK报文段以后，就关闭连接；此时，主机1等待2MSL后依然没有收到回复，则证明Server端已正常关闭，那好，主机1也可以关闭连接了。



##### TCP 与 UDP 的区别
- 1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接
- 2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付
- 3、TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流;UDP是面向报文的
UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）
- 4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
- 5、TCP首部开销20字节;UDP的首部开销小，只有8个字节
- 6、TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道

#### 7. HTTP 协议； HTTP1.0 与 2.0 的区别；HTTP 报文结构（Jason）

##### 1.HTTP协议
即超文本传输协议(Hypertext transfer protocol)。是一种详细规定了浏览器和万维网(WWW = World Wide Web)服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议。

HTTP协议是用于从WWW服务器传输超文本到本地浏览器的传送协议。它可以使浏览器更加高效，使网络传输减少。它不仅保证计算机正确快速地传输超文本文档，还确定传输文档中的哪一部分，以及哪部分内容首先显示(如文本先于图形)等。

HTTP是一个应用层协议，由请求和响应构成，是一个标准的客户端服务器模型。HTTP是一个无状态的协议。

##### 2.HTTP 报文结构
HTTP 请求报文
HTTP 请求报文由请求行、请求头部、空行和请求包体4个部分组成，如下图所示：

![http请求报文.png](https://upload-images.jianshu.io/upload_images/1128757-a04d132fc78b88ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
GET /search?hl=zh-CN&source=hp&q=domety&aq=f&oq= HTTP/1.1  
Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/vnd.ms-excel, application/vnd.ms-powerpoint, 
application/msword, application/x-silverlight, application/x-shockwave-flash, */*  
Referer: <a href="http://www.google.cn/">http://www.google.cn/</a>  
Accept-Language: zh-cn  
Accept-Encoding: gzip, deflate  
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; TheWorld)  
Host: <a href="http://www.google.cn">www.google.cn</a>  
Connection: Keep-Alive  
Cookie: PREF=ID=80a06da87be9ae3c:U=f7167333e2c3b714:NW=1:TM=1261551909:LM=1261551917:S=ybYcq2wpfefs4V9g; 
NID=31=ojj8d-IygaEtSxLgaJmqSjVhCspkviJrB6omjamNrSm8lZhKy_yMfO2M4QMRKcH1g0iQv9u-2hfBW7bUFwVh7pGaRUb0RnHcJU37y-
FxlRugatx63JLv7CWMD6UB_O_r
```

##### 3.HTTP 响应报文
HTTP 响应报文由状态行、响应头部、空行和响应包体4个部分组成，如下图所示：
![Http响应报文.png](https://upload-images.jianshu.io/upload_images/1128757-10112de60ab98309.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 4.HTTP1.0 与 2.0 的区别
###### （1）性能提升

HTTP 2.0 的出现，相比于 HTTP 1.x，大幅度的提升了web性能。在与 HTTP/1.1完全语义兼容的基础上，进一步减少了网络延迟。

HTTP/2: [the Future of the Internet](https://http2.akamai.com/demo),这是Akamai公司建立的一个官方的演示，用以说明 HTTP/2 相比于之前的 HTTP/1.1 在性能上的大幅度提升。 同时请求 379 张图片，从Load time 的对比可以看出 HTTP/2 在速度上的优势。


![Http1.1和Http2请求速度对比.png](https://upload-images.jianshu.io/upload_images/1128757-a84be972dab24d51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###### （2）多路复用 (Multiplexing)
多路复用允许同时通过单一的HTTP/2连接发起多重的请求-响应消息。

众所周知 ，在 HTTP/1.1 协议中 「浏览器客户端在同一时间，针对同一域名下的请求有一定数量限制。超过限制数目的请求会被阻塞」。
该图总结了不同浏览器对该限制的数目。

![浏览器请求限制的数目.png](https://upload-images.jianshu.io/upload_images/1128757-9702907a5cdacd09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这也是为何一些站点会有多个静态资源 CDN 域名的原因之一，拿 Twitter 为例，http://twimg.com，目的就是变相的解决浏览器针对同一域名的请求限制阻塞问题。
而 HTTP/2 的多路复用(Multiplexing) 则允许同时通过单一的 HTTP/2 连接发起多重的请求-响应消息。


![Http2多路复用.png](https://upload-images.jianshu.io/upload_images/1128757-089f6fe72d4522b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


因此 HTTP/2 可以很容易的去实现多流并行而不用依赖建立多个 TCP 连接，HTTP/2 把 HTTP 协议通信的基本单位缩小为一个一个的帧，这些帧对应着逻辑流中的消息。并行地在同一个 TCP 连接上双向交换消息。

**二进制分帧**

在不改动 HTTP/1.x 的语义、方法、状态码、URI 以及首部字段….. 的情况下, HTTP/2 是如何做到「突破 HTTP1.1 的性能限制，改进传输性能，实现低延迟和高吞吐量」的 ?
关键之一就是在 应用层(HTTP/2)和传输层(TCP or UDP)之间增加一个二进制分帧层。

![二进制分桢.png](https://upload-images.jianshu.io/upload_images/1128757-5371c6d9f31ff9eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在二进制分帧层中，HTTP/2会将所有传输的信息分割为更小的消息和帧（frame）,并对它们采用二进制格式的编码 ，其中 HTTP1.x 的首部信息会被封装到 HEADER frame，而相应的 Request Body 则封装到 DATA frame 里面。

HTTP/2 通信都在一个连接上完成，这个连接可以承载任意数量的双向数据流。

在过去， HTTP 性能优化的关键并不在于高带宽，而是低延迟。TCP 连接会随着时间进行自我「调谐」，起初会限制连接的最大速度，如果数据成功传输，会随着时间的推移提高传输的速度。这种调谐则被称为 TCP 慢启动。由于这种原因，让原本就具有突发性和短时性的 HTTP 连接变的十分低效。

HTTP/2 通过让所有数据流共用同一个连接，可以更有效地使用 TCP 连接，让高带宽也能真正的服务于 HTTP 的性能提升。

**总结：**

单连接多资源的方式，减少服务端的链接压力,内存占用更少,连接吞吐量更大，由于 TCP 连接的减少而使网络拥塞状况得以改善，同时慢启动时间的减少,使拥塞和丢包恢复速度更快

###### （3）服务端推送（Server Push）
服务端推送是一种在客户端请求之前发送数据的机制。在 HTTP/2 中，服务器可以对客户端的一个请求发送多个响应。Server Push 让 HTTP1.x 时代使用内嵌资源的优化手段变得没有意义；如果一个请求是由你的主页发起的，服务器很可能会响应主页内容、logo 以及样式表，因为它知道客户端会用到这些东西。这相当于在一个 HTML 文档内集合了所有的资源，不过与之相比，服务器推送还有一个很大的优势：可以缓存！也让在遵循同源的情况下，不同页面之间可以共享缓存资源成为可能。





#### 8. HTTP 与 HTTPS 的区别以及如何实现安全性（Jason）
###### 区别：

- 1.HTTPS加密传输协议HTTP名文传输协议;
- 2.HTTPS需要用SSL证书HTTP用;
- 3.HTTPS比HTTP更加安全、搜索引擎更友;
- 4.HTTPS标准端口443，HTTP标准端口80;
- 5.HTTPS基于传输层HTTP基于应用层;
- 6.HTTPS浏览器显示绿色安全锁HTTP没显示;

###### 如何实现安全性

HTTPS协议是标准的HTTP协议架在SSL/TLS协议之上的一种结构，HTTPS实现安全性的几个主要机制：
1. 证书：通过第三方权威证书颁发机构（如VeriSign）验证和担保网站的身份，防止他人伪造网站身份与不知情的用户建立加密连接。
2. 密钥交换：通过公钥（非对称）加密在网站服务器和用户之间协商生成一个共同的会话密钥。
3. 会话加密：通过机制(2)协商的会话密钥，用对称加密算法对会话的内容进行加密。
4. 消息校验：通过消息校验算法来防止加密信息在传输过程中被篡改。


