# 清除inline-block元素之间的空格间隙
2017-04-27

终于不用兼容ie678了, 兴高采烈的用着inline-block, 哐当一下给我来了个下马威, 怎么不能对齐, 好像inline-block之间有间隙...   
仔细一看, 明白了原来是元素之间的换行符...

![](https://raw.githubusercontent.com/383514580/Notes/master/image/1.png)

### 开始清除
百度了下, 找到一个终极办法, 设置父元素的font-size: 0, 问题解决了. (舒口气, 吓死朕了)

![](https://raw.githubusercontent.com/383514580/Notes/master/image/2.png)


### 2017-06-01补充
偶然看到张大神的文章, 提供了好多另类的方法, 特别转过来分享下: 
http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/