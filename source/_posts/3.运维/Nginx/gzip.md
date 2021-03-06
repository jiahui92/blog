---
title: gzip
toc: true
date: 2020-03-02 00:00:03
tags:
---


通常gzip压缩的是浏览器本地缓存的资源；切忌给高并发的资源开启，这样会导致CPU高负载【[参考资料](https://www.cnblogs.com/Renyi-Fan/p/11047490.html)】

```nginx
gzip on;
gzip_comp_level 1;
gzip_min_length 1k; # 太小的文件无需压缩
gzip_types text/css application/javascript application/json; # 图片、视频类资源压缩比不大；text/html默认会有；

gzip_vary on; # Vary: Accept-Encoding
gzip_proxied expired no-cache public;
gzip_disable "MSIE [1-6]\."; # IE6不支持

```

## gzip_proxied
Nginx作为反向代理`proxy_pass`时可以使用，意为header中包含该值时才启用压缩；之所以要该设置项，是因为`proxy_pass`对应的资源可能是动态的；比如`html`文件，对于后端渲染的页面或json接口来说，它可能是经常变动的，所以利用header里的缓存标志来判断该资源是否经常变动，从而决定是否要开启压缩；
```nginx
gzip_proxied off | expired | no-cache | no-store | private | no_last_modified | no_etag | auth | any ...;
```
* off: 默认值，只要是`proxy_pass`都不压缩
* any: 无条件压缩所有数据
* expired: header中包含此值时压缩

## gzip_comp_level
* 级别越大，压缩越好，但CPU占用越多；综合看来1为最优；
* 自己的试验是36kb 压缩成 6kb（可能是因为json里重复的"key"太多了，所以效果很好）；在请求时，确实会导致CPU会有百分之几的占用，当快到达瓶颈时需要关闭动态资源的压缩；
```s
gzip_comp_level 0:    0, 94840 kb,  63 ms
gzip_comp_level 1: 2.43, 39005 kb, 248 ms
gzip_comp_level 2: 2.51, 37743 kb, 273 ms
gzip_comp_level 3; 2.57, 36849 kb, 327 ms
gzip_comp_level 4; 2.73, 34807 kb, 370 ms
gzip_comp_level 5; 2.80, 33898 kb, 491 ms
gzip_comp_level 6; 2.82, 33686 kb, 604 ms
gzip_comp_level 7; 2.82, 33626 kb, 659 ms
gzip_comp_level 8; 2.82, 33626 kb, 698 ms
gzip_comp_level 9; 2.82, 33626 kb, 698 ms
```
