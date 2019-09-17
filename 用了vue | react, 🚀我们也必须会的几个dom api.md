# vue/react的UI库作者必用的几个DOM API, 🚀写业务代码也可事半功倍.

虽然vue/react帮我们实现了操作数据映射到dom操作, 但是**还是有很多不得不用DOM API的场景**, 下面我就给大家列出**一些UI库中经常出现的DOM API**(写业务代码也可事半功倍).

**注**: 本文是系列文章会持续更新, 大家可收藏保持关注.

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

![](https://ws1.sinaimg.cn/large/005IQkzXly1g72av3nethj30kt03iglz.jpg)

https://github.com/ElemeFE/element/blob/dev/packages/infinite-scroll/src/main.js#L134


之前自己写过scroll插件, 用他来监视容器内元素的增加变化

![](https://ws1.sinaimg.cn/large/005IQkzXly1g72jlbw63xj30uk05xt9l.jpg)

https://github.com/any86/any-scroll/blob/develop/src/components/Core.vue#L378

更多说明, 还请参考[MDN - MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

### 兼容性
![](https://ws1.sinaimg.cn/large/005IQkzXly1g72a9fn1srj30z10c940n.jpg)

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
一般用在**弹出菜单**的关闭, 通过`contains`判断点击元素是否是菜单本身或在菜单内, 如果不在其内那么表示要关闭菜单, 比如饿了么UI的popover组件:

![](https://ws1.sinaimg.cn/large/005IQkzXly1g72i84s49vj30h204qq3d.jpg)

https://github.com/ElemeFE/element/blob/dev/packages/popover/src/main.vue#L200


### 兼容性
![](https://ws1.sinaimg.cn/large/005IQkzXly1g72b7mwq9fj30z609c3zx.jpg)

### 注意(node.compareDocumentPosition)
我们看到了兼容性在pc还可以, 但是在移动端基本"全军覆没", 在我写本文的时候发现了这个[compareDocumentPosition](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/compareDocumentPosition), 他的兼容性就比contains好很多, 但是由于我还没有实践, 稍后我测试后, 会更新到本文.
![](https://ws1.sinaimg.cn/large/005IQkzXly1g72c494n95j30xt0dn76f.jpg)


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
一般用来实现"图片懒加载", 比如**vue-lazyload**用他来监测当前图片是否在可视区:

![](https://ws1.sinaimg.cn/large/005IQkzXly1g729ylv518j30v004g0tg.jpg)

https://github.com/hilongjw/vue-lazyload/blob/master/src/lazy-image.js#L72

### 兼容性
![](https://ws1.sinaimg.cn/large/005IQkzXly1g72a8m6o7mj30yx09sq48.jpg)

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
parent.insertAdjacentElement('afterend', node);
```

### 常用在哪里?
比如对话框组件为了不受到父元素`overflow:hidden`的影响而被遮挡, 都会把组件移动到body的尾部, 我之前为了解决这个问题, 写过一个vue的插件, 可以把任意组件移动到body的任意位置:

![](https://ws1.sinaimg.cn/large/005IQkzXly1g72icub9ntj30qb02q0tc.jpg)

https://github.com/any86/vue-create-root/blob/master/src/createRoot.ts#L22


### 兼容性
![](https://ws1.sinaimg.cn/large/005IQkzXly1g6vixkn5p1j30z908qdhd.jpg)


## 总结
其实还有很多DOM API也是组件开发中常用的, 为了不让内容太长不利于学习, 我会拆成多篇文章来讲解, 后续可能还有2-3篇文章, 等都更新完, 我会出一个汇总的文章方便大家查阅.

感谢大家的阅读, 如有疑问可以加群🚀

**注**: 由于群已超过100人需要邀请加入, 可通过加我微信, 我拉你进群.

![](https://ws1.sinaimg.cn/large/005IQkzXly1g72iwfr0llj30e80e8gm2.jpg)
