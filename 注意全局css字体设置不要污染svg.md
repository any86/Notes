# 注意全局css字体设置不要污染svg

### 情况
在做svg中的字体样式切换的功能, 发现怎么切换都不好使, 字体在`<text>`标签中, 发现把生成svg占到任意网页svg内部的字体都会生效.

### 发现
原来`<text>`虽然通过属性设置了font-family, 如`<text font-family="xxx">`, 但是由于网页的全局css中, 我定义了一个`*{font-family:yyy;}`, 这样svg中字体居然是优先选择了css中的字体设置, 完全出乎我们的意料

### 解决
办法比较粗暴, 就是对全局的字体css设置加上`:not(text, tspan, textPath)`, 排除我们要通过属性设置字体的元素
