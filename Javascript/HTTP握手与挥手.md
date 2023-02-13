### HTTP三次握手

三次握手是指客户端在和服务端建立tcp连接的时候需要进行三次通信，然后才能完成连接。

#### 步骤：

#### 1、客户端向服务端发送一个数据包，告诉服务端需要建立连接;


#### 2、服务端收到客户端发送的数据包之后，会返回一个数据包，通知客户端，我已经收到你的连接请求;


#### 3、客户端收到服务端返回的信息后，知道服务端已经准备好建立连接，但是还需要再发送一个数据包给服务端，用于告诉服务端我已经收到你的回复了。

这个时候，连接就已经建立了。双方可以进行数据传输了。

![握手](https://upload-images.jianshu.io/upload_images/2179059-169363549c8e9f57.png?imageMogr2/auto-orient/strip|imageView2/2/w/1040/format/webp)

### HTTP四次挥手

四次挥手是指断开TCP连接时，需要客户端和服务端总共发送4次数据包以确认连接的断开

#### 步骤：

### 1、第一次挥手：client发送一个FIN，用来关闭client到server的数据传输，client进入FIN_WAIT_1状态；

### 1、第二次挥手：server收到FIN后，发送一个ACK给client，确认序号为收到序号+1，server进入CLOSE_WAIT状态
；
### 1、第三次挥手：server发送一个FIN，用来关闭server到client的数据传输，server进入LAST_ACK状态；

### 1、第四次挥手：client收到FIN后，client进入TIME_WAIT状态，接着发送一个ACK给server，确认序号为收到序号+1，server进入CLOSE状态。

![挥手](https://upload-images.jianshu.io/upload_images/2179059-9b09667fe6b27de1.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)