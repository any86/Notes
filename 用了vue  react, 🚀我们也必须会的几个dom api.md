# vue / react的UI库都在用的几个DOM API🚀

虽然vue/react帮我们实现了操作数据映射到dom操作, 但是**还是有很多不得不用DOM API的场景**, 下面我就给大家列出**一些UI库中经常出现的DOM API**(写业务代码也可事半功倍).

**注**: 本文是系列文章会持续更新, 大家可收藏保持关注. 也可关注[我github上的笔记](https://github.com/any86/Notes), 在这里发布的文章的第一版我都会存在[https://github.com/any86/Notes](https://github.com/any86/Notes)上.

## MutationObserver
监视dom元素的变化触发回调, 可监视的变化有:属性(attribute) / 文本(characterData), 同时支持监视子孙节点(childList/subtree), 

### 简单举例
```javascript
// 注册监视器, 一旦发生变化会alert
let observer = new MutationObserver(()=>{
    alert('change')
});
// 开始监视
observer.observe(el, { childList: true, subtree: true });
// 停止监视, 释放资源
observer.disconnect()
```



### 常用在哪里?
一般用在滚动加载, 用来监视元素是否已经加入父元素, 加入成功后触发回调, 比如**饿了么UI**就用来注册滚动加载成功后的回调:

#### 饿了么UI

![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6b217b6b5?w=749&h=126&f=jpeg&s=57292)

https://github.com/ElemeFE/element/blob/dev/packages/infinite-scroll/src/main.js#L134

#### any-scroll
之前自己写过scroll插件, 用他来监视容器内元素的增加变化

![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e1c15d1e011b?w=1100&h=213&f=jpeg&s=94163)

https://github.com/any86/any-scroll/blob/develop/src/components/Core.vue#L378

更多说明, 还请参考[MDN - MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

### 兼容性
![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6b263ef10?w=1261&h=441&f=jpeg&s=310534)

## node.contains(otherNode) 
返回的是一个布尔值，来表示传入的节点(otherNode)是否为该节点(node)的子孙节点.

### 简单举例
```javascript
// 判断元素是否body元素且是否是body的子孙元素.
function isInPage(node) {
  return (node === document.body) ? false : document.body.contains(node);
}
```

### 常用在哪里?

#### 饿了么UI
一般用在**弹出菜单**的关闭, 通过`contains`判断点击元素是否是菜单本身或在菜单内, 如果不在其内那么表示要关闭菜单, 比如饿了么UI的popover组件:

![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6b67b372a?w=614&h=170&f=jpeg&s=60463)

https://github.com/ElemeFE/element/blob/dev/packages/popover/src/main.vue#L200


### 兼容性
![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6b7ce626b?w=1266&h=336&f=jpeg&s=199262)

### 注意
虽然canisuse上提醒移动端很多是unknown的状态, 但是"有赞的同学"提醒说**有赞移动端UI**一直在使用`contains`, 经过长时间线上实践, 并未发现不兼容, 所以我们可以在移动端/pc放心使用.

这里特别对有赞的"**陈嘉涵**"同学表示感谢. 👏

![](https://user-gold-cdn.xitu.io/2019/9/18/16d432a2a8497977?w=681&h=165&f=jpeg&s=81665)

源码地址: https://github.com/youzan/vant/blob/dev/src/mixins/click-outside.ts#L23

## element.getBoundingClientRect():rect
获取元素**相对浏览器左上角**的偏移量以及元素尺寸信息, **返回值**是一个`rect`对象, 其中包括:
`left`: **元素左上角**距离浏览器左上角的X轴偏移.
`top`: **元素左上角**距离浏览器左上角的Y轴偏移.
`width`: 元素宽度.
`height`: 元素高度.
`right`: **元素右下角**距离浏览器左上角的X轴偏移.
`bottom`: **元素右下角**距离浏览器左上角的Y轴偏移.
`x`: 同`left`.
`y`: 同`top`.

### 常用在哪里?

#### vue-lazyload
一般用来实现"图片懒加载", 比如**vue-lazyload**用他来监测当前图片是否在可视区:

![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6b7dd0687?w=1116&h=160&f=jpeg&s=86538)

https://github.com/hilongjw/vue-lazyload/blob/master/src/lazy-image.js#L72

### 兼容性
![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6e7fb04ad?w=1257&h=352&f=jpeg&s=205569)

### 注意
`getBoundingClientRect`会受到`transform`的影响, 比如你的元素设置了`transform:scale(2)`, 那么`getBoundingClientRect`返回的width会是元素实际宽度的2倍, `top`等位置信息也会因为元素尺寸变化而发生变化.


## insertAdjacentElement
可以通过不同的参数实现**jQuery**的`append` | `prepend` | `after` | `before`.


### 简单实现
下面我举例对比说明,  先看下dom结构:
```html
<div id="parent"></div>
```
#### 实现append(beforeend), 插入到指定元素内部的尾部

```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).append(node);
parent.insertAdjacentElement('beforeend', node);
```

#### 实现prepend(afterbegin), 插入到指定元素内部的头部

```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).prepend(node);
parent.insertAdjacentElement('afterbegin', node);
```


#### 实现after(afterend), 插入到指定元素后面

```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).after(node);
parent.insertAdjacentElement('afterend', node);
```

#### 实现before(beforebegin), 插入到指定元素前面
```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).after(node);
parent.insertAdjacentElement('beforebegin', node);
```

### 常用在哪里?

#### vue-create-root
比如对话框组件为了不受到父元素`overflow:hidden`的影响而被遮挡, 都会把组件移动到body的尾部, 我之前为了解决这个问题, 写过一个vue的插件, 可以把任意组件移动到body的任意位置:

![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6e9928583?w=947&h=98&f=jpeg&s=76538)

https://github.com/any86/vue-create-root/blob/master/src/createRoot.ts#L22


### 兼容性
![](https://user-gold-cdn.xitu.io/2019/9/17/16d3e0b6f1b44b67?w=1269&h=314&f=jpeg&s=217690)


## 总结
其实还有很多DOM API也是组件开发中常用的, 为了不让内容太长不利于学习, 我会拆成多篇文章来讲解, 后续可能还有2-3篇文章, 等都更新完, 我会出一个汇总的文章方便大家查阅.

感谢大家的阅读, 如有疑问可以加**qq**群🚀

![](https://user-gold-cdn.xitu.io/2019/9/19/16d473b3f57615d9?w=540&h=740&f=jpeg&s=54564)

也可加我微信, 我拉你进入**微信群**(由于腾讯对微信群的100人限制, 超过100人后必须由我拉进去)

![](https://user-gold-cdn.xitu.io/2019/9/19/16d474d245b69492?w=512&h=512&f=jpeg&s=27137)

为了学习组件开发自己也造了个轮子🚀, 仅供娱乐.
https://github.com/any86/any-ui

