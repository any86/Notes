# 鼠标时间的event对象
## 2017-06-01
欢快的儿童节, 欢快的转载, 看到一篇全面的介绍, 搬过来
http://www.cnblogs.com/zxktxj/archive/2012/02/26/2369176.html




Event属性和方法：

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


一些说明：

1.  event代表事件的状态，例如触发event对象的元素、鼠标的位置及状态、按下的键等等；

2.  event对象只在事件发生的过程中才有效。
firefox里的event跟IE里的不同，IE里的是全局变量，随时可用；firefox里的要用参数引导才能用，是运行时的临时变量。
在IE/Opera中是window.event，在Firefox中是event；而事件的对象，在IE中是 window.event.srcElement，在Firefox中是event.target，Opera中两者都可用。

3.  下面两句效果相同
var evt = (evt) ? evt : ((window.event) ? window.event : null);
var evt = evt || window.event; // firefox下window.event为null, IE下event为null

4.  IE中事件的起泡
IE中事件可以沿着包容层次一点点起泡到上层，也就是说，下层的DOM节点定义的事件处理函数，到了上层的节点如果还有和下层相同事件类型的事件处理函数，那么上层的事件处理函数也会执行。例如， div 标签包含了 a ，如果这两个标签都有onclick事件的处理函数，那么执行的情况就是先执行标签 a 的onclick事件处理函数，再执行 div 的事件处理函数。如果希望的事件处理函数执行完毕之后，不希望执行上层的 div 的onclick的事件处理函数了，那么就把cancelBubble设置为true即可。
js event.keyCode对应的键码：

keycode 8 = BackSpace BackSpace 
keycode 9 = Tab Tab 
keycode 12 = Clear 
keycode 13 = Enter 
keycode 16 = Shift_L 
keycode 17 = Control_L 
keycode 18 = Alt_L 
keycode 19 = Pause 
keycode 20 = Caps_Lock 
keycode 27 = Escape Escape 
keycode 32 = space space 
keycode 33 = Prior 
keycode 34 = Next 
keycode 35 = End 
keycode 36 = Home 
keycode 37 = Left 
keycode 38 = Up 
keycode 39 = Right 
keycode 40 = Down 
keycode 41 = Select 
keycode 42 = Print 
keycode 43 = Execute 
keycode 45 = Insert 
keycode 46 = Delete 
keycode 47 = Help 
keycode 48 = 0 equal braceright 
keycode 49 = 1 exclam onesuperior 
keycode 50 = 2 quotedbl twosuperior 
keycode 51 = 3 section threesuperior 
keycode 52 = 4 dollar 
keycode 53 = 5 percent 
keycode 54 = 6 ampersand 
keycode 55 = 7 slash braceleft 
keycode 56 = 8 parenleft bracketleft 
keycode 57 = 9 parenright bracketright 
keycode 65 = a A 
keycode 66 = b B 
keycode 67 = c C 
keycode 68 = d D 
keycode 69 = e E EuroSign 
keycode 70 = f F

keycode 71 = g G 
keycode 72 = h H 
keycode 73 = i I 
keycode 74 = j J 
keycode 75 = k K 
keycode 76 = l L 
keycode 77 = m M mu 
keycode 78 = n N 
keycode 79 = o O 
keycode 80 = p P 
keycode 81 = q Q at 
keycode 82 = r R 
keycode 83 = s S 
keycode 84 = t T 
keycode 85 = u U 
keycode 86 = v V 
keycode 87 = w W 
keycode 88 = x X 
keycode 89 = y Y 
keycode 90 = z Z 
keycode 96 = KP_0 KP_0 
keycode 97 = KP_1 KP_1 
keycode 98 = KP_2 KP_2 
keycode 99 = KP_3 KP_3 
keycode 100 = KP_4 KP_4 
keycode 101 = KP_5 KP_5 
keycode 102 = KP_6 KP_6 
keycode 103 = KP_7 KP_7 
keycode 104 = KP_8 KP_8 
keycode 105 = KP_9 KP_9 
keycode 106 = KP_Multiply KP_Multiply 
keycode 107 = KP_Add KP_Add

keycode 108 = KP_Separator KP_Separator 
keycode 109 = KP_Subtract KP_Subtract 
keycode 110 = KP_Decimal KP_Decimal 
keycode 111 = KP_Divide KP_Divide 
keycode 112 = F1 
keycode 113 = F2 
keycode 114 = F3 
keycode 115 = F4 
keycode 116 = F5 
keycode 117 = F6 
keycode 118 = F7 
keycode 119 = F8 
keycode 120 = F9 
keycode 121 = F10 
keycode 122 = F11 
keycode 123 = F12 
keycode 124 = F13 
keycode 125 = F14 
keycode 126 = F15 
keycode 127 = F16 
keycode 128 = F17 
keycode 129 = F18 
keycode 130 = F19 
keycode 131 = F20 
keycode 132 = F21 
keycode 133 = F22 
keycode 134 = F23 
keycode 135 = F24 
keycode 136 = Num_Lock 
keycode 137 = Scroll_Lock 
keycode 187 = acute grave 
keycode 188 = comma semicolon 
keycode 189 = minus underscore 
keycode 190 = period colon 
keycode 192 = numbersign apostrophe 
keycode 210 = plusminus hyphen macron 
keycode 211 = 
keycode 212 = copyright registered 
keycode 213 = guillemotleft guillemotright 
keycode 214 = masculine ordfeminine 
keycode 215 = ae AE 
keycode 216 = cent yen 
keycode 217 = questiondown exclamdown 
keycode 218 = onequarter onehalf threequarters 
keycode 220 = less greater bar 
keycode 221 = plus asterisk asciitilde 
keycode 227 = multiply division

keycode 228 = acircumflex Acircumflex 
keycode 229 = ecircumflex Ecircumflex 
keycode 230 = icircumflex Icircumflex 
keycode 231 = ocircumflex Ocircumflex 
keycode 232 = ucircumflex Ucircumflex 
keycode 233 = ntilde Ntilde 
keycode 234 = yacute Yacute 
keycode 235 = oslash Ooblique 
keycode 236 = aring Aring 
keycode 237 = ccedilla Ccedilla 
keycode 238 = thorn THORN 
keycode 239 = eth ETH 
keycode 240 = diaeresis cedilla currency 
keycode 241 = agrave Agrave atilde Atilde 
keycode 242 = egrave Egrave 
keycode 243 = igrave Igrave 
keycode 244 = ograve Ograve otilde Otilde 
keycode 245 = ugrave Ugrave 
keycode 246 = adiaeresis Adiaeresis 
keycode 247 = ediaeresis Ediaeresis 
keycode 248 = idiaeresis Idiaeresis 
keycode 249 = odiaeresis Odiaeresis 
keycode 250 = udiaeresis Udiaeresis 
keycode 251 = ssharp question backslash 
keycode 252 = asciicircum degree 
keycode 253 = 3 sterling 
keycode 254 = Mode_switch

键值对应表
A　　0X65 　U 　　0X85
B　　0X66　 V　　 0X86
C　　0X67　 W　　 0X87
D　　0X68　 X 　　0X88
E　　0X69　 Y　　 0X89
F　　0X70　 Z　　 0X90
G　　0X71　 0　　 0X48
H　　0X72　 1　　 0X49
I　　0X73　 2　　 0X50
J　　0X74　 3 　　0X51
K　　0X75　 4 　　0X52
L　　0X76　 5 　　0X53
M　　0X77　 6　　 0X54
N　　0X78 　7 　　0X55
O　　0X79 　8 　　0X56
P　　0X80 　9 　　0X57
Q　　0X81　ESC　　0X1B
R　　0X82　CTRL 　0X11
S　　0X83　SHIFT　0X10
T　　0X84　ENTER　0XD

如果要使用组合键，则可以利用event.ctrlKey，event.shiftKey，event .altKey判断是否按下了ctrl键、shift键以及alt键.