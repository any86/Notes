# ES6提升效率的几个小技巧.md

以下代码均可在这里实时测试 => http://babeljs.io/repl/   

### 展开数组/赋值
```javascript
var array = [1, 2, ...[3, 4]];
console.log(array) // [1, 2, 3, 4]
```

**还可以这样**
```javascript
var [a, b, c, d] = [1, 2, ...[3, 4]];
console.log(a, b, c, d) // 1, 2, 3, 4
```


### 展开对象/赋值
```javascript
var object = {a:1 , ...{b: 2, c: 3}, d: 4};
console.log(object) // {a:1 , b: 2, c: 3, d: 4}
```

**还可以这样**
```javascript
// 只去a和d的值
var {a, d} = {a:1 , b: 2, c: 3, d: 4};
console.log(a, d) // 1, 4
```
想想ajax返回数据的时候用这个**赋值**是不是很惬意...

### 合并对象
```javascript
var objA = {a: 1 , c: 3};
var objB = {b: 2, c: 4};
var objAB = {...objA, ...objB}; //  {a: 1, b: 2, c: 4}
```

### 给函数传值
```javascript
var obj = {x: 1, y: 2};
function add({x, y}){
  return x + y;
}
add(obj); // 1 + 2 = 3;
```

### 更简单的定义函数
```javascript
var fn = () => {} // 等于 var  fn = function(){}

var fn = (a, b) => a + b // 等于 var fn = function(){return a + b;}

```
**注意:** 如果用了"=>"来表示函数, 那么函数内部的this为undefined, 这样我们就不用想以前那样写什么"_this"了.
```javascript
var fn = () => {
	console.log(this); // undefined
}
```
以上代码均可在这里实时测试 => http://babeljs.io/repl/   


更多ES6的内容请看阮一峰大神的文档 => http://es6.ruanyifeng.com/#README
