# vue-cli配置成多页模式(dev阶段配置)
本文内容只设计`npm run dev`相应webpack代码的修改, `npm run build`会在后续文章中追加.

来, 没病和我走5步

### 第一步, 建立html模板
默认vue-cli是制作单页应用吗, html模板当然就只有一个index.html, 现在我们有几个页面就建立几个html, 比如: detail.html
```
src/
    index.html
    detail.html
    ...
```

### 第二部, 建立入口js文件
删除main.js, 建立index.js/detail.js
```
src/
    index.js
    detail.js
    ...
```

### 第三部, 修改webpack.base.config.js中的entry配置
```javascript
...
module.exports = {
  entry: {
    index: './src/index.js',
    detail: './src/detail.js'
  },
  ...
```

### 第四部, 修改webpack.dev.config.js中的HtmlWebpackPlugin配置
有多少个页面就向plugins中添加多少个new HtmlWebpackPlugin(), 注意**chunks**的配置, 里面是你要引入的入口文件(entry, 第三步你自己配置的哦)的名字, 这样最终生成的页面中就会自动引入对应的entry文件js
```javascript
...
plugins: [
    ...
    new HtmlWebpackPlugin({
      filename: 'list.html',
      template: 'list.html',
      chunks: ['list'], 
      inject: true
    }),

    new HtmlWebpackPlugin({
      filename: 'detail.html',
      template: 'detail.html',
      chunks: ['detail'], 
      inject: true
    }),    
    ...
  ...

```

### 第五步
好像没有第五步了, 再见.
