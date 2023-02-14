## HTTP

### 概念

HTTP是Hyper Text Transfer Protocol（超文本传输协议）的缩写。它是一个应用层协议，由请求和响应构成，是一个标准的客户端服务器模型。HTTP是一个无状态的协议。
### 特点
#### 1、无连接

无连接的含义是限制每次连接只处理一个请求.服务器处理完客户端的请求,然后响应,并收到应答之后,就断开连接.这种方式可以节省传输时间

### 2、无状态

HTTP协议是无状态协议.无状态是指协议 对于事务处理没有记忆能力.这种方式的一个坏处就是,如果后续的处理需要用到之前的信息,则必须要重传,这样就导致了每次连接传输的数据量增大.好处就是,如果后续的连接不需要之前提供的信息,响应就会比较快.而为了解决HTTP的无状态特性,出现了Cookie和Session技术.

#### 3、简单快速

客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快

#### 4、灵活

HTTP允许传输任意类型的数据对象.正在传输的类型由Content-Type加以标记

![Content-Type](https://upload-images.jianshu.io/upload_images/2905385-34c79757a37ffd1c.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

### 地址栏输入url的过程

![URL](https://upload-images.jianshu.io/upload_images/2905385-8e33a9df92437c9b.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

### HTTP 方法

#### GET

获取资源，会被缓存

#### POST

传输资源，比get传输更多，在request body

#### PUT

更新资源

#### DELETE

删除资源

#### HEAD

获取报文头部

### HTTP状态码

https://www.runoob.com/http/http-status-codes.html

### HTTP持久连接（HTTP1.1）

HTTP协议采用“请求-应答”模式，并且HTTP是基于TCP进行连接的。普通模式（非keep-alive）时，每个请求或应答都需要建立一个连接，完成之后立即断开。

当使用Conection: keep-alive模式（又称持久连接、连接重用）时，keep-alive使客户端道服务器端连接持续有效，即不关闭底层的TCP连接，当出现对服务器的后继请求时，keep-alive功能避免重新建立连接。


### HTTP数据协商
#### Request

##### Accept: 在请求中使用Accept可申明想要的数据格式
##### Accept-Encoding:可接受的响应内容的编码方式；告诉服务端使用什么的方式来进行压缩 比如gzip、deflate、br

#### Response
#####  Content-Type:对应Accept，从请求中的Accept支持的数据格式中选一种来返回
#####  Content-Encoding: 对应 Accept-Encoding，指服务端到底使用的是那种压缩方式

