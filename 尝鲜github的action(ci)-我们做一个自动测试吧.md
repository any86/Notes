# github新出的action是什么? 用他做自动测试?

## 体验分享
本文一个尝鲜的体验分享, 并没有太复杂的技巧, 做了一个最少代码的例子展示, 让每个人都可以把action用起来, 如果路过的大牛有高级技巧请留言分享, 我会补充. 下面正文开始.

## 是什么?
是一个**免费**的操作系统容器(Linux/Windows/macOS), 我们可以让他预装开发环境(node/php/python...).
**注:** 后面的文章假设我们选了一台装有**nodejs的linux**服务器.

## 有什么用?
我们可以上传(git push)代码, 然后在他的nodejs中执行, 如果我们写的代码中有测试脚本, 那么他执行完毕后会给我们一个图标反馈到github的提交记录, 如下图:
![](https://ws1.sinaimg.cn/large/005IQkzXly1g5yxyb07p9j30ub04hdgk.jpg)
如果代码执行出现错误, 会反馈一个**红色**的"x"图标.

## 怎么用?

### 进入action页面
现在任何仓库都多了一个**action**按钮, 如图:
![](https://ws1.sinaimg.cn/large/005IQkzXly1g5yya6q3i4j30tz04h3ze.jpg)

### 选择需要的环境
第一次进入会让我们选择开发环境, 这里我选择了nodejs, 点击对应的"Set up this workflow"按钮, 如图:

![](https://ws1.sinaimg.cn/large/005IQkzXly1g5yyc4j0ufj315s0mygpr.jpg)

### 告诉action你要干什么
点击后我们进入了编辑界面, 在这里我们要**告诉"action"他要做什么**, 如图:
![](https://ws1.sinaimg.cn/large/005IQkzXly1g5yye17bamj30pl0gcdhk.jpg)
如果仔细观察你会发现: 这个编辑界面对应的是一个文件, 我们根目录下多了一个".github/workflows/nodejs.yml", 我们对**action**的设置都会存储在这里, 下次修改我们直接编辑这个文件即可.

### 解释下配置文件
``` shell
name: Node CI

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@master
    - name: Use Node.js 10.x
      uses: actions/setup-node@v1
    - name: test
      run: |
        npm i
        npm run test:rules

```
#### name
显示标题, 运行时显示, 没太大意义.

#### on
看到**on**就想到事件触发, 是的他可以注册**对git动作的监视**, 比如监视仓库的push/pull_request等动作, 想了解更多动作解释看[文档](https://help.github.com/en/articles/events-that-trigger-workflows)

比如设置监视多个动作:
``` shell
on: [push, pull_request]
```

还可以针对分支来监控
``` shell
on:
  push:
    branches:
    - develop
```

#### jobs
这个是核心功能了, 在这里我们要告诉**action**做什么, 


##### jobs.id
其下的 **"build"** 字段暂时可理解成id, 我们可以改成其他名字比如"test", 如果有多个可以让多个job并行, 但是id不能相同.
**注:** 文档中有个**needs**字段可设置依赖执行, 我还没实践他, 如果这篇看的人多, 我研究下然后在写第二篇补充下 😋)

##### jobs.id.run-on
表示运行的操作系统, **ubuntu-latest**代表最新版本的Ubuntu, 也可以指定版本号, 根据文档提示**action**支持如下系统:
- ubuntu-latest, ubuntu-18.04, or ubuntu-16.04
- windows-latest, windows-2019, or windows-2016
- macOS-latest or macOS-10.14

##### jobs.id.steps 
设置动作, 也就是action的核心功能.

##### jobs.id.steps.name
用来设置每步动作的显示标题, 运行时显示, 可以随意写.

##### jobs.id.steps.uses
可以执行一些action封装好的动作:
1. **uses: actions/checkout@master**, 拉取代码.
2. **actions/setup-node@v1**, 初始化node环境.

##### jobs.id.steps.run
执行命令
1. 安装包: **npm run test:rules**
2. 执行我们写好的测试命令 **npm run test:rules**


## 执行结果
![](https://ws1.sinaimg.cn/large/005IQkzXgy1g5z0fnd086j31ha0czdh7.jpg)
在action中我们可以看到我们写的脚本被执行了, 如果执行没有报错那么就会提示我们"成功", 用"绿色"表示.

[查看真实项目](https://github.com/any86/any-rule/commit/2ab7e59b08d54a917e771b53c2af0ca021feba39/checks)

## 总结
好了就写这么多吧, 也是初用, 写的时候也是战战兢兢, 怕发布的时候被大牛喷, 不过真的很喜欢action, 还是想写个文章推广下, 抛砖引玉. 谢谢大家的阅读.

