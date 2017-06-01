# 鼠标事件的event对象
## 2017-06-01
欢快的儿童节, 欢快的转载, 看到一篇全面的介绍, 搬过来
http://www.cnblogs.com/zxktxj/archive/2012/02/26/2369176.html


### Event属性和方法：

1. type：事件的类型，如onlick中的click；

2. srcElement/target：事件源，就是发生事件的元素；

3. button：声明被按下的鼠标键，整数，1代表左键，2代表右键，4代表中键，如果按下多个键，酒把这些值加起来，所以3就代表左右键同时按下；（firefox中 0代表左键，1代表中间键，2代表右键）

4. clientX/clientY：事件发生的时候，鼠标相对于浏览器窗口可视文档区域的左上角的位置；(在DOM标准中，这两个属性值都不考虑文档的滚动情况，也就是说，无论文档滚动到哪里，只要事件发生在窗口左上角，clientX和clientY都是 0，所以在IE中，要想得到事件发生的坐标相对于文档开头的位置，要加上
document.body.scrollLeft和 document.body.scrollTop)

5. offsetX,offsetY/layerX,layerY：事件发生的时候，鼠标相对于源元素左上角的位置；

6. x,y/pageX,pageY：检索相对于父要素鼠标水平坐标的整数；

7. altKey,ctrlKey,shiftKey等：返回一个布尔值；

8. keyCode：返回keydown何keyup事件发生的时候按键的代码，以及keypress 事件的Unicode字符；(firefox2不支持 event.keycode，可以用 event.which替代 )

9. fromElement,toElement：前者是指代mouseover事件中鼠标移动过的文档元素，后者指代mouseout事件中鼠标移动到的文档元素；

10. cancelBubble：一个布尔属性，把它设置为true的时候，将停止事件进一步起泡到包容层次的元素；(e.cancelBubble = true; 相当于 e.stopPropagation();)

11. returnValue：一个布尔属性，设置为false的时候可以组织浏览器执行默认的事件动作；(e.returnValue = false; 相当于 e.preventDefault();)

12. attachEvent(),detachEvent()/addEventListener(),removeEventListener：为制定 DOM对象事件类型注册多个事件处理函数的方法，它们有两个参数，第一个是事件类型，第二个是事件处理函数。在
attachEvent()事件执行的时候，this关键字指向的是window对象，而不是发生事件的那个元素；

13. screenX、screenY：鼠标指针相对于显示器左上角的位置，如果你想打开新的窗口，这两个属性很重要；


### 一些说明：

1.  event代表事件的状态，例如触发event对象的元素、鼠标的位置及状态、按下的键等等；

2.  event对象只在事件发生的过程中才有效。
firefox里的event跟IE里的不同，IE里的是全局变量，随时可用；firefox里的要用参数引导才能用，是运行时的临时变量。
在IE/Opera中是window.event，在Firefox中是event；而事件的对象，在IE中是 window.event.srcElement，在Firefox中是event.target，Opera中两者都可用。

3.  下面两句效果相同
var evt = (evt) ? evt : ((window.event) ? window.event : null);
var evt = evt || window.event; // firefox下window.event为null, IE下event为null

4.  IE中事件的起泡
IE中事件可以沿着包容层次一点点起泡到上层，也就是说，下层的DOM节点定义的事件处理函数，到了上层的节点如果还有和下层相同事件类型的事件处理函数，那么上层的事件处理函数也会执行。例如， div 标签包含了 a ，如果这两个标签都有onclick事件的处理函数，那么执行的情况就是先执行标签 a 的onclick事件处理函数，再执行 div 的事件处理函数。如果希望的事件处理函数执行完毕之后，不希望执行上层的 div 的onclick的事件处理函数了，那么就把cancelBubble设置为true即可。
