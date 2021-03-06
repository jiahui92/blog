---
title: https
toc: true
date: 2020-03-04 00:00:03
tags:
---


* 信息加密传输
* 配备数字证书
* 具有校验机制，一旦被篡改，通信双方会立刻发现


## SSL握手
![](/img/Snip20200304_14.png)
【[参考资料: wireshark抓包](https://razeencheng.com/post/ssl-handshake-detail.html)】
* 每个https网站会提供一个数字证书，浏览器会确认该证书是托管在受信的第三方机构的且没有遭到篡改
* 数字证书里包含有当前网站的域名和公钥
* SSL握手阶段期间需要协商客户端和服务端都可用的加密方式
* 经过SSL握手阶段后，服务器和客户端双方得到对称密钥，然后用对称密钥加密信息



## 浏览器验证网站数字证书的流程
上级颁发证书时会用私钥加密该证书的散列值来生成数字签名，然后浏览器用其上级的公钥对该数字签名进行RSA解密并校验散列值。这些上级的证书一般都是预存在浏览器内的。【[参考资料](https://blog.csdn.net/hejjiiee/article/details/53443357)】
![](/img/Snip20200304_15.png)


## Charles https代理的原理
Charles会拦截服务器返回的证书，并替换为自己的证书；然后客户端使用信任过的charles证书的公钥进行加密，charles收到客户端信息后，使用私钥解密，然后继续用真正的服务器证书的公钥加密，再发送给服务器；其实就相当于`中间人攻击`。【[参考资料](https://zhuanlan.zhihu.com/p/67199487】


## ssh原理
[链接](/wiki/0.计算机基础/Linux/command#原理)


## TLS和SSL的区别
TLS 1.0通常被标示为SSL 3.1，TLS 1.1为SSL 3.2，TLS 1.2为SSL 3.3。不同的版本，对应着支持不同的加密算法；
