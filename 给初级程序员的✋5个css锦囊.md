# 给初级程序员的✋5个救命的🚀mixin(scss)

## 封装成mixin复用
在写🔥css的时候, 很多样式都是**很常用但是写起来很麻烦**, 虽然现在有很多成熟的ui框架, 但是我们也不能一个**简单的活动页**也引入那么大个框架吧?

在工作中我也攒下了不少常用css, 我把他们封装成了**mixin**, 挑选了✋5个分享给大家, 希望大家喜欢.

## 溢出显示省略号

参过参数可以只是单/多行.
```scss
/**
* 溢出省略号
* @param {Number} 行数
*/
@mixin ellipsis($rowCount: 1) {
  @if $rowCount <=1 {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  } @else {
    min-width: 0;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: $rowCount;
    -webkit-box-orient: vertical;
  }
}
```

### 真.1px边框
移动端由于像素密度比的问题, 实际的1px边框看起来比较粗, 如果想要更"细"可以使用下面的代码. **不同像素密度比的设备都可以兼容(pc/手机), 还支持任意数量圆角**.
```scss
/**
* 真.1px边框
* {List}: 多个方向边框, 默认值为bottom, 你可以根据需要传入(top, left, bottom, right) 4个方向;
* {Color} 边框的颜色, 默认#ccc;
* {List} 4个圆角半径, 默认0;
* {String} 一个高级设置, 一般都不需要改动, 由于细边框的实现使用了css的伪类, 所以为了规避可能出现的样式冲突, 我们可以自己指定使用:after还是:before, 默认after;
*/
@mixin thinBorder(
  $directionMaps: bottom,
  $color: #ccc,
  $radius: (
    0,
    0,
    0,
    0
  ),
  $position: after
) {
  // 是否只有一个方向
  $isOnlyOneDir: string==type-of($directionMaps);

  @if ($isOnlyOneDir) {
    $directionMaps: ($directionMaps);
  }

  @each $directionMap in $directionMaps {
    border-#{$directionMap}: 1px solid $color;
  }

  // 判断圆角是list还是number
  @if (list==type-of($radius)) {
    border-radius: nth($radius, 1)
      nth($radius, 2)
      nth($radius, 3)
      nth($radius, 4);
  } @else {
    border-radius: $radius;
  }

  @media only screen and (-webkit-min-device-pixel-ratio: 2) {
    & {
      position: relative;

      // 删除1像素密度比下的边框
      @each $directionMap in $directionMaps {
        border-#{$directionMap}: none;
      }
    }

    &:#{$position} {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      display: block;
      width: 200%;
      height: 200%;
      transform: scale(0.5);
      box-sizing: border-box;
      padding: 1px;
      transform-origin: 0 0;
      pointer-events: none;
      border: 0 solid $color;

      @each $directionMap in $directionMaps {
        border-#{$directionMap}-width: 1px;
      }

      // 判断圆角是list还是number
      @if (list==type-of($radius)) {
        border-radius: nth($radius, 1) *
          2
          nth($radius, 2) *
          2
          nth($radius, 3) *
          2
          nth($radius, 4) *
          2;
      } @else {
        border-radius: $radius * 2;
      }
    }
  }

  @media only screen and (-webkit-min-device-pixel-ratio: 3) {
    &:#{$position} {
      // 判断圆角是list还是number
      @if (list==type-of($radius)) {
        border-radius: nth($radius, 1) *
          3
          nth($radius, 2) *
          3
          nth($radius, 3) *
          3
          nth($radius, 4) *
          3;
      } @else {
        border-radius: $radius * 3;
      }

      width: 300%;
      height: 300%;
      transform: scale(0.3333);
    }
  }
}
```

### 等边三角形

常用来做下拉菜单的方向指示, 如果你做的页面就是个简单的活动页, 引入"饿了么"一类的ui就有些大材小用了, 借助"三角形"你可以自己做一个简单的.

```scss
/**
* 等边三角形
* @param {String} 尺寸
* @param {Color} 颜色
* @param {String} 方向
*/
@mixin triangle($size: 5px, $color: rgba(0, 0, 0, 0.6), $dir: bottom) {
  width: 0;
  height: 0;
  border-style: solid;

  @if (bottom==$dir) {
    border-width: $size $size 0 $size;
    border-color: $color transparent transparent transparent;
  } @else if (top==$dir) {
    border-width: 0 $size $size $size;
    border-color: transparent transparent $color transparent;
  } @else if (right==$dir) {
    border-width: $size 0 $size $size;
    border-color: transparent transparent transparent $color;
  } @else if (left==$dir) {
    border-width: $size $size $size 0;
    border-color: transparent $color transparent transparent;
  }
}
```


### loading

![undefined](http://ww1.sinaimg.cn/large/005IQkzXly1g96k3mj697g301j01pq2t.gif)
这是一个"半圆边框"旋转的loading, 你可以根据业务需求自己指定圆的半径.

```scss
@mixin loading-half-circle($width: 1em) {
  display: inline-block;
  height: $width;
  width: $width;
  border-radius: $width;
  border-style: solid;
  border-width: $width/10;
  border-color: transparent currentColor transparent transparent;
  animation: rotate 0.6s linear infinite;
  vertical-align: middle;
}
```

### 棋盘
如果你做一些界面生成器工具(类易企秀)你会用到.

![1.png](http://ww1.sinaimg.cn/large/005IQkzXly1g96k2cabqlj30ac05bgm3.jpg)
```scss
/**
* 棋盘背景
* @param {Color} 背景色
*/
@mixin bgChessboard($color: #aaa) {
  background-image: linear-gradient(
      45deg,
      $color 25%,
      transparent 25%,
      transparent 75%,
      $color 75%,
      $color
    ),
    linear-gradient(
      45deg,
      $color 26%,
      transparent 26%,
      transparent 74%,
      $color 74%,
      $color
    );
  background-size: 10vw 10vw;
  background-position: 0 0, 5vw 5vw;
}
```

## 总结

上面的代码我放github了, 源码: https://github.com/any86/5a.css/blob/develop/src/_mixins.scss

就总结了这么多, 希望对大家有用, 写的不一定全面, 抛砖引玉, 还请大家多多包涵, 感谢大家的阅读.

