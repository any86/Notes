#http1下谷歌可以同时发起几个请求?.md
## 2017-09-22

### 参考

针对每个源(域名)同时可以发起6个

https://developers.google.com/web/tools/chrome-devtools/network-performance/understanding-resource-timing

![image](https://user-images.githubusercontent.com/8264787/30724868-6f6a5308-9f06-11e7-96eb-a7d8e9e20570.png)

### 总结
也就是如果有12个资源, A服务器6个,B6个, 那么浏览器可以同时请求12个资源
