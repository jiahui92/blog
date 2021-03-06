---
title: IP, ARP, DHCP, TCP/UDP
toc: true
date: 2020-03-03 00:00:02
tags:
---

## IP
数据报表中主要包含 【[参考资料](https://www.kancloud.cn/lifei6671/tcp-ip/139886)】
* 源IP
* 目的IP
* 数据: 存储上层协议的数据，比如TCP

![](/img/Snip20200304_7.png)
![](/img/Snip20200304_8.png)

### ipv4
### ipv6
* ipv4/ipv6双协议栈技术
* 隧道技术


## ARP 地址解析协议
当主机根据IP查找不到对应MAC地址时，会全网广播询问（广播后，谁先单播回应，则用谁的MAC地址，这也是ARP被诟病的原因之一）【[参考资料](https://zhuanlan.zhihu.com/p/28771785)】

## RARP
* 主机在局域网内广播自己的MAC地址，请求分配一个IP
* 主机在局域网内广播一个MAC地址，请求返回其IP

## DHCP
DHCP是RARP的升级版，所以两者才会这么像；
* DHCP是基于UDP和MAC
* 通常用于路由器的动态管理ip地址：当主机连接路由器时，路由器会分配一个新的局域网ip给你，并且附带上子网掩码和默认网关等信息
* 当默认网关发生修改时，也会在局域网内广播


## TCP/UDP
【[参考资料](https://zhuanlan.zhihu.com/p/24860273)】
* TCP保证数据正确性、数据顺序；UDP不保证，可能会丢包
* UDP数据简单，但不能分帧，只能一次发送所有数据
* UDP传输数据不建立连接，因此也就不需要维护连接状态，吞吐量不受拥挤控制算法的调节

### TCP的三次握手、四次挥手
![](/img/Snip20200304_10.png)

为什么握手是三次，不是两次或四次？【[参考资料](https://github.com/jawil/blog/issues/14)】
* 考虑握手请求可能会被网络延迟的情况，一个被延迟的请求后来又跑到了服务器，那么因为是两次握手，所以服务器就会发送请求给客户端，之后就直接进入ESTABLISHED阶段了，等待客户端发送数据，但客户端这时候又不处理超时请求，这样会造成服务器资源浪费
* 四次的话，应该是为了保证客户端收到两次服务端返回的信息，才进入ESTABLISHED阶段，防止后面收到服务端的延时请求，造成的客户端等待资源浪费。因为是客户端，所以没太大关系了。不用太追求严谨。

### TCP keep-alive
注意和http的keep-alive区分。基本原理是，隔一段时间给连接对端发送一个探测包，如果收到对方回应的 ACK，则认为连接还是存活的，在超过一定重试次数之后还是没有收到对方的回应，则丢弃该 TCP 连接。而http的keep-alive则是复用TCP连接来下载资源【[参考资料](https://stackoverflow.com/questions/9334401/http-keep-alive-and-tcp-keep-alive)】


### TCP 流量控制和拥塞控制
【[参考资料](https://zhuanlan.zhihu.com/p/37379780)】
* `流量控制／滑动窗口`：根本目的是为了防止数据丢失；双方会互相沟通接下来要传的窗口块序号，一块一块传输，确保没有丢失；
* `拥塞控制`：防止发送方发送太快，导致接收方负载过大；
  * `慢启动`：从一个较小的窗口开始指数增大；
  * `拥塞避免`：增大到一定值后，开始改为加法增大；一旦阻塞，就乘法减小到一半；
  * `快速重传`
  * `快速恢复`

![](/img/Snip20200304_11.png)

![](https://pic3.zhimg.com/80/v2-c72fce5494ca8ee12244189430f12cea_720w.jpg)

![](https://pic4.zhimg.com/80/v2-5f4034bc11c3a48a1d1a115f9ee0259b_720w.jpg)
