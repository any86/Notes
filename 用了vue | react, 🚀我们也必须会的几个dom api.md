# 用了vue | react, 🚀我们也必须会的几个dom api

以前用jq主要是为了兼容浏览器, 随着时间的推移兼容ie低版本的需求已经变得很少了, 尤其移动端的项目我们可以完全放心使用使用原生api, 今天我就给大家介绍2个最常用的api.

## insertAdjacentElement

## document.querySelector

## MutationObserver

## classList toggle

## getAttribute/setAttribute

## getScrollHeight/getOffsetHeight/getClientHeight




在js中如果你想用🚀原生api实现jq的`append`和`prepend`以及`after`和`before`, 原来你可能会说用`appendChild`和`insertBefore`, 但是19年的今天"兼容ie"这种要求已经很少见, 我们还有必要用这么蹩脚的api去实现吗?

![](https://ws1.sinaimg.cn/large/005IQkzXly1g6vixkn5p1j30z908qdhd.jpg)

答案当然是否定的, 其实大部分浏览器早早已经实现类似的api, 就是`targetElement.insertAdjacentElement(position, element)`. 通过`position`的取值(beforebegin | afterbegin | beforeend | afterend)不同可以很简单的实现上述jq的4个api.
为了加强记忆,下面我举例对比说明,  先看下dom结构:
```html
<div id="parent"></div>
```

## 实现append(beforeend), 插入到指定元素内部的尾部

```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).append(node);
parent.insertAdjacentElement('beforeend', node);
```

## 实现prepend(afterbegin), 插入到指定元素内部的头部

```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).prepend(node);
parent.insertAdjacentElement('afterbegin', node);
```


## 实现after(afterend), 插入到指定元素后面

```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).after(node);
parent.insertAdjacentElement('afterend', node);
```

## 实现before(beforebegin), 插入到指定元素前面
```javascript
let parent = document.getElementById('parent');
let node = document.createElement('span');
// 等价于 $(parent).after(node);
parent.insertAdjacentElement('afterend', node);
```


## 兼容性
看完上面你一定有个疑问, 都没见过他, 是不是兼容不好啊?

## 如何记忆

## 实例


## insertAdjacentText和富文本编辑器







