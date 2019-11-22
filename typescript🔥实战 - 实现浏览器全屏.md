## 学习一门新的语言, 🚀最快速的方法还是看实战代码!

初学ts的时候, 大家都会问"**有什么实际项目可以参考吗?**".

好了! 满足大家的需求, 我计划**定期**用ts做一些业务常用小插件, **代码量尽量小**(控制在1kb以内), 方便大家阅读源码, 也方便大家**有时间**去实现一遍.


## 浏览器全屏
![1.png](https://user-gold-cdn.xitu.io/2019/11/19/16e812aa1e280c7d?w=434&h=61&f=jpeg&s=15340)

最近后台项目需要一个"**全屏**"的按钮, github了下, 发现都仅仅支持"**开启全屏**", 而没有"**切换**"/"**监听全屏状态**"等功能, 所以打算自己写一个(主要代码量不大, 嘿嘿).

## 写代码之前说说逻辑
所有现代浏览器(**>IE11**)都提供了"全屏"的api,只是不同浏览器有不同的api(基本就是前缀不同), 所以我们要做的就是判断浏览器, 然后执行正确的api.

1. 判断当前浏览器支持的前缀, 比如"webkit".
2. 根据前缀得出我们需要的4个api的名字.
3. 通过api实现"全屏"/"退出"/"切换"/"监听".

## 代码

源码: https://github.com/any86/be-full

<img src="https://img.shields.io/github/stars/any86/be-full?style=social"/>

首先我发现ts自带的声明中, 对`webkit`或`moz`开头的这种api并没有声明类型, **所以我们需要自己补充一下**.
不然ts会提示找不到对应的方法.
![](https://user-gold-cdn.xitu.io/2019/11/19/16e8275b5f5ccc1d?w=855&h=53&f=png&s=15950)

**🔥 知识点**: 在自己的`.d.ts`文件中可以对ts系统自带的`interface`进行**声明合并**. 

```typescript
// global.d.ts
interface HTMLElement {
    // 进入全屏
    webkitRequestFullscreen(options?: FullscreenOptions): Promise<void>;
    webkitRequestFullScreen(options?: FullscreenOptions): Promise<void>;
    msRequestFullscreen(options?: FullscreenOptions): Promise<void>;
    mozRequestFullScreen(options?: FullscreenOptions): Promise<void>;

    // 监听全屏
    onwebkitfullscreenchange: ((this: Element, ev: Event) => any) | null;
    onmozfullscreenchange: ((this: Element, ev: Event) => any) | null;
    MSFullscreenChange: ((this: Element, ev: Event) => any) | null;
}

interface Document {
    // 当前全屏的元素
    readonly webkitFullscreenElement: Element | null;
    readonly msFullscreenElement: Element | null;
    readonly mozFullScreenElement: Element | null;

    // 退出全屏
    webkitExitFullscreen(): Promise<void>;
    msExitFullscreen(): Promise<void>;
    mozCancelFullScreen(): Promise<void>;
}
```
**提示**: `requestFullscreen(options?: FullscreenOptions): Promise<void>;`可以在**node_modules/typescript/lib/lib.dom.d.ts**中找到.

#### 🔥 功能实现
功能其实很简单, 我都写了注释, 就不过分解读了,  好了看**实现代码**:
```typescript
type RFSMethodName = 'webkitRequestFullScreen' | 'requestFullscreen' | 'msRequestFullscreen' | 'mozRequestFullScreen';
type EFSMethodName = 'webkitExitFullscreen' | 'msExitFullscreen' | 'mozCancelFullScreen' | 'exitFullscreen';
type FSEPropName = 'webkitFullscreenElement' | 'msFullscreenElement' | 'mozFullScreenElement' | 'fullscreenElement';
type ONFSCPropName = 'onfullscreenchange' | 'onwebkitfullscreenchange' | 'onmozfullscreenchange' | 'MSFullscreenChange';

/**
 * caniuse
 * https://caniuse.com/#search=Fullscreen
 * 参考 MDN, 并不确定是否有o前缀的, 暂时不加入
 * https://developer.mozilla.org/zh-CN/docs/Web/API/Element/requestFullscreen
 * 各个浏览器
 * https://www.wikimoe.com/?post=82
 */
const DOC_EL = document.documentElement;

let RFC_METHOD_NAME: RFSMethodName = 'requestFullscreen';
let EFS_METHOD_NAME: EFSMethodName = 'exitFullscreen';
let FSE_PROP_NAME: FSEPropName = 'fullscreenElement';
let ON_FSC_PROP_NAME: ONFSCPropName = 'onfullscreenchange';

if (`webkitRequestFullScreen` in DOC_EL) {
    RFC_METHOD_NAME = 'webkitRequestFullScreen';
    EFS_METHOD_NAME = 'webkitExitFullscreen';
    FSE_PROP_NAME = 'webkitFullscreenElement';
    ON_FSC_PROP_NAME = 'onwebkitfullscreenchange';
} else if (`msRequestFullscreen` in DOC_EL) {
    RFC_METHOD_NAME = 'msRequestFullscreen';
    EFS_METHOD_NAME = 'msExitFullscreen';
    FSE_PROP_NAME = 'msFullscreenElement';
    ON_FSC_PROP_NAME = 'MSFullscreenChange';
} else if (`mozRequestFullScreen` in DOC_EL) {
    RFC_METHOD_NAME = 'mozRequestFullScreen';
    EFS_METHOD_NAME = 'mozCancelFullScreen';
    FSE_PROP_NAME = 'mozFullScreenElement';
    ON_FSC_PROP_NAME = 'onmozfullscreenchange';
} else if (!(`requestFullscreen` in DOC_EL)) {
    throw `当前浏览器不支持Fullscreen API !`;
}

/**
 * 启用全屏
 * @param  {HTMLElement} 元素
 * @param  {FullscreenOptions} 选项
 * @returns {Promise}
 */
export function beFull(el: HTMLElement = DOC_EL, options?: FullscreenOptions): Promise<void> {
    return el[RFC_METHOD_NAME](options);
}

/**
 * 退出全屏
 */
export function exitFull(): Promise<void> {
    return document[EFS_METHOD_NAME]();
}

/**
 * 元素是否全屏
 * @param {HTMLElement}
 */
export function isFull(el: HTMLElement | EventTarget): boolean {
    return el === document[FSE_PROP_NAME]
}

/**
 * 切换全屏/关闭
 * @param  {HTMLElement} 目标元素
 * @returns {Promise}
 */
export function toggleFull(el: HTMLElement = DOC_EL, options?: FullscreenOptions): Promise<void> {
    if (isFull(el)) {
        return exitFull();
    } else {
        return beFull(el, options)
    }
}

/**
 * 当全屏/退出时触发
 * @param  {HTMLElement} 元素
 * @param  {(isFull: boolean) => void} 返回"是否全屏"
 */
export function watchFull(el: HTMLElement, callback: (isFull: boolean) => void) {
    const cancel = () => {
        el.onfullscreenchange = null;
    };

    const handler = (event: Event) => {
        if (null !== event.target) {
            callback(isFull(event.target));
        }
    }

    // 这里addEventListener不好使
    el[ON_FSC_PROP_NAME] = handler;

    return {
        cancel
    }
}
```
源码: https://github.com/any86/be-full


## 🚀 typescript系列课程
如果你对ts感兴趣了, 欢迎看看我的基础教程.

[第一课, 体验typescript](https://juejin.im/post/5d19ad6de51d451063431864)

[第二课, 基础类型和入门高级类型](https://juejin.im/post/5d1af3426fb9a07ed4411a9b)

[第三课, 泛型](https://juejin.im/post/5d27f160e51d45108223fcf9)

[第四课, 解读高级类型](https://juejin.im/post/5d3fe80fe51d456206115987)

[第五课, 命名空间(namespace)是什么](https://juejin.im/post/5d5d04dfe51d4561af16dd24)

[特别篇, 在vue3🔥源码中学会typescript🦕 - "is"](https://juejin.im/post/5da6d1aae51d4524ad10d1d8)

[第六课, 什么是声明文件(declare)? 🦕 - 全局声明篇](https://juejin.im/post/5dcbc9e2e51d451bcb39f123)

## 总结
多写多练, 很快就上手, 放几个我用ts写的项目当做参考, 抛砖引玉, 加油!

**✋ 移动/pc端手势库, 支持: tap/press/pan/swipe/rotate/pinch**

https://github.com/any86/any-touch

<img src="https://img.shields.io/github/stars/any86/any-touch?style=social"/> 

**🍭 把vue组件变成this.$xxx这样的命令**

https://github.com/any86/vue-create-root

<img src="https://img.shields.io/github/stars/any86/vue-create-root?style=social"/>

**🍔 任意元素全屏显示, 支持PC/移动端, 不到1kb**

https://github.com/any86/be-full

<img src="https://img.shields.io/github/stars/any86/be-full?style=social"/>

## 微信群
感谢大家的阅读, 如有疑问可以加我微信, 我拉你进入**微信群**(由于腾讯对微信群的100人限制, 超过100人后必须由群成员拉入)

![](https://user-gold-cdn.xitu.io/2019/9/19/16d474d245b69492?w=512&h=512&f=jpeg&s=27137)
