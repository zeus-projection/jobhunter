# Http

#####TCP3次握手

client          server

SYN seq = x —>

<— SYN seq=y, ACK=x+1

ACK = y + 1 —>



#####Http 和 Https的一些区别：

1. Https协议需要到CA申请证书，一般免费证书很少，需要缴费。

2. HTTP协议运行于TCP之上，所有传输内容都是明文，HTTPS运行在SSL／TLS上，SSL／TLS运行在TCP上，所有的传输内容都是经过加密的。

3. HTTP和HTTPS使用的是完全不同的链接方式，用的端口也不一样，前者80，后者是443.

4. HTTPS可以有效防止运行商劫持。

   ​

#####SPDY

主要解决的问题：

1. 降低延迟，通过多路复用（multiplexing）。多路复用通过多个请求stream共享一个tcp链接的方式，解决了HOL blocking的问题，降低了延迟同时提高了带宽的利用率。
2. 请求优先级（request prioritization）。主要解决多路复用带来的关键请求被block的问题。给每个请求单独设置优先级。
3. header压缩。消灭掉HTTP1.x里面多余的header数据。
4. 基于HTTPS的加密协议传输。
5. 服务端推送。

HTTP —>SPDY —>SSL —>TCP



#####HTTP2.0

和SPDY的区别

1. HTTP2.0 支持明文HTTP传输，而SPDY强制使用HTTPS
2. HTTP2.0消息头的压缩算法采用 HPACK，而是不是SPDY的DEFLATE

新特性

1. 新的二进制格式，HTTP1.x 是基于文本的，二进制格式优于文本格式。
2. 多路复用，即连接共享，即每一个request都作为连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的id将request再归属到不同的服务端请求里面。（一个连接，多个请求）。
3. header 压缩，通讯双方个CACHE一份header field表，既避免了重复的header传输，又减小了需要传输的大小。
4. 服务端推送。

HTTP2.0 性能增强的核心：**二进制分帧**



##### SSL握手

​	**目的**：

	1.	客户端与服务器需要就一组用于**保护数据的算法达成一致**；
	2.	它们需要确立一组由这些算法所使用的**加密密钥**；
	3.	握手还可以**选择对客户端进行认证**；



​	**过程：**

1.  客户端将它所支持的**算法列表**和一个用作产生密钥的**随机数**发送给服务器；
2.  服务器从算法列表中选择一种**加密算法**，并将它和一份包含服务器**公用密钥的证书**发送给客户端；该证书还包含了用于认证目的的**服务器标识**，服务器同时还提供一个用作产生密钥的**随机数**。
3.  客户端对服务器证书进行验证，并抽取服务器的公用密钥；然后再产生一个称为 **pre_master_secret**的随机密码串，并使用服务器的公用密钥对其进行加密，并将加密后的信息发送给服务器；
4.  客户端和服务器根据pre_master_secret以及客户端与服务器的随机数值，**独立计算出加密和MAC密钥**；
5.  客户端将所有握手消息的MAC值发送给服务器；
6.  服务器将所有握手消息的MAC值发送给客户端；



clientHello —> 

<— severHello

<— Certificate

<— ServerHelloDone

ClientKeyExchange—>

Finished —>

<— Finished