## offsetWidth 与 clientWidth、scrollWidth
* offsetWidth/offsetHeight返回值包含content + padding + border，效果与e.getBoundingClientRect()相同
* clientWidth/clientHeight返回值只包含content + padding，如果有滚动条，也不包含滚动条
* scrollWidth/scrollHeight返回值包含content + padding + 溢出内容的尺寸
* offsetLeft = 相对于父元素，在x方向上的偏移量
* scrollLeft = x方向上的滚动

  * clientWidth=5+200+5
  * scrollWidth=5+实际宽度+5
  * offsetWidth=5+5+200+5+5

## 窗口大小
* screenLeft 窗口左边位置
* 浏览器窗口位置都是以screen为前缀的 screenLeft screenX
* 窗口大小 innerHeight innerWidth

## 获取元素距离页面的top、left值(兼容ie)

```
function getRect(ele) {
  let _t = document.documentElement.clientTop,
      _l = document.documentElement.clientLeft,
      rect = ele.getBoundingClientRect();
      return {
        top: rect.top - _t,
        left: rect.left - _l,
        bottom: rect.bottom - _t,
        right: rect.right - _l,
      }
}
```

## import 和 require的区别；import {} 和没有{}的区别
* 遵循规范：require是amd规范 import是es6规范
* 调用时间：require是运行时调用，可以放在代码的任意地方，import是编译时调用，放在文件开头

本质：require是赋值过程，import是解构过程，浏览器需要对其进行bable转换
https://segmentfault.com/a/1190000010426778

exports = module.exports = {};指向同一块内存
其实require导出的内容是module.exports的指向的内存块内容，并不是exports的。
简而言之，区分他们之间的区别就是 exports 只是 module.exports的引用，辅助后者添加内容用的。

module.experts默认值为空对象{}，

require()返回的是module.experts而不是experts,

如果给module.experts重新赋值了，为了让两者关联需要experts = module.experts = somtObj;

export与export default均可用于导出常量、函数、文件、模块等
在一个文件或模块中，export、import可以有多个，export default仅有一个通过export方式导出，在导入时要加{ }，export default则不需要export能直接导出变量表达式，export default不行。

## ajax原理
```
new xmlHttpRequest()
open('POST', url, false)
xmlhttp.onreadystatechange = function () {
        if (xmlhttp.readyState == 4) {
            document.getElementById("user1").innerHTML = "数据正在加载...";
            if (xmlhttp.status == 200) {
                document.write(xmlhttp.responseText);
            }
        }
    }
    xmlhttp.send();
}
```
缺点：

* ajax干掉了back按钮，即对浏览器后退机制的破坏
* 对搜索引擎的支持比较弱
* 安全问题 sql注入攻击

## 发布者订阅模式
订阅发布模式定义了一种一对多的依赖关系，让多个订阅者对象同时监听某一个主题对象。这个主题对象在自身状态变化时，会通知所有订阅者对象，使它们能够自动更新自己的状态。

```
var showObj = {};
showObj.list = [];
showObj.listen = function(fn) {
  this.list.push(fn);
};
showObj.trigger = function() {
  var i=0;
  return function() {
    let fn = showObj.list[i];
    fn.apply(this, arguments);
    i++;
  }
}
showObj.listen(function(color, size){
  console.log(`颜色是：${color}`)
  console.log(`颜色是：${size}`)
})
showObj.listen(function(color, size) {
  console.log(`改变后的颜色是：${color}`)
  console.log(`改变后的颜色是：${color}`)
})
let stack = showObj.trigger();
stack('红色', 20);
stack('绿色', 30);
```

# 知识点
* call 和 apply应用后会直接进行函数调用，而bind返回的是这个函数,是函数的方法
* 实例的原型就是构造函数的原型对象

```
myinstantce._proto_ = myconstructor.prototype;
myconstructor.prototype.constructor = myconstructor;
```
* new 操作符作用

```
创造了一个新的对象
this指向该实例
原型指向构造函数的原型对象
构造函数有返回就会替换new出来的对象，如果没有就是new 出来的对象
```

* 继承

```
function Animal(name){}
function Cat(){}

1.Cat.prototype = new Animal(); //原型链继承
2.function Cat(color){ //构造函数继承
  Animal.call(this);
  this.color = color;
}
3.function Cat(color) { //实例继承
  var animal = new Animal();
  animal.color = color;
  return animal;
}
4.function Cat(name){ //铐被继承
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}
5.es6继承
class ColorPoint extends Point {
  construtor(x, y, color) {
    super(x, y);
    this.color = color;
  }
}
```

* class

```
super作为方法在子类中必须调用一次，代表父类的构造函数，在constructor中调用，作为对象时则super指父类的原型对象。
静态方法
static 关键字是指该方法不会被实例继承，直接通过类来调用，这就是‘静态方法’；
class Parent{
  constructor(arg1) {}
  static tell (){
    console.log('tell')
  }
};
Parent.tell();
静态属性
指定义在class本身上的属性，而不是定义在实例对象this上的属性
如Parent.tell = 'hello world';
```

* Promise

```
缺点：无法取消Promise，一旦新建就会立即执行，中途就无法取消，如果没有设置回调会导致内部错误无法被捕捉到，不能反应到外部，当处于pending时无法得知目前到哪一个状态了（刚刚开始还是即将完成）。
如果某个事件不断发生，使用stream比promise好。
Promise.all([p1,p2,p3]).then((data) => {
  console.log(data);
})
只有全部promise实例的状态fullfilled,P的状态才变为fullfilled，p返回值为由每个实例返回值组成的数组；
只要有一个promise实例被rejected，则P就被rejected，该promise实例的rejected返回值会传递给P的回调

Promise.race([p1,p2,p3]).then((data) => {
  console.log(data);
});
只要有一个的状态改变则整体的状态就会发生改变，该值传递给P的回调，如果在指定时间内没哟获得结果就会将P的状态变为rejected，否则是resolved;
promise中断后续调用:return Promise.reject('中断')
```

* Iterator 接口(可遍历的接口)

```
// 手写iterator接口
Array.prototype[Symbol.iterator] = function() {
  let i = 0;
  console.log(this[i]);
	let self = this;
  return {
    next() {
      return {
        value: self[i++],
        done: 	!self[i]
      };
    }
  }
}
var it = [1,2,3,4,5][Symbol.iterator]();
it.next();
```

* 获取元素的最终bgcolor:
`getComputedStyle($('#el')).getPropertyValue('background-color');
`

* 懒加载与预加载：

```
懒加载的特点：按需加载
将image标签的src置空，标签数据data-orignal为真实地址，在需要的时候讲这个地址赋值给src。
<img/>
预加载：提升用户体验
图片在使用之前提取请求
资源在后续使用中从缓存中拿取；
实现是：用Image对象来实现。
```

* 跨域解决方案

```
1. iframe
  window.name 进行跨域通信（只需要是同一个窗口就行）
  window.name = data;
  data = document.getElementById('myFrame').contentWindow.name;
  postMessage() localStorage;

2. ajax
  1.代理服务器nginx(常见配置记住)
  2.JSONP
  原理：ajax请求受同源策略影响，不允许进行跨域请求，而script标签src属性中的链接却可以访问跨域的js脚本，利用这个特性，服务端不再返回JSON格式的数据，而是返回一段调用某个函数的js代码，在src中进行了调用，这样实现了跨域。
  3.websocket
  WebSocket是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。
  4.cors
  CORS是跨源资源分享（Cross-Origin Resource Sharing）的缩写。它是W3C标准，是跨源AJAX请求的根本解决方法。相比JSONP只能发GET请求，CORS允许任何类型的请求。
  原理：在响应头设置Access-Control-Allow-Origin，这样浏览器检测到header中的Access-Control-Allow-Origin，则就可以跨域操作了。CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
  （1）简单请求
  1.1 方法
  包含以下之一：HEAD GET POST
  1.2 header字段
  Accept Accept-Language Content-Language
  Last-Event-ID Content-Type：（application/x-www-form-urlencoded、multipart/form-data、text/plain）
  浏览器在请求中添加一个origin字段，说明请求的源。服务器不包含该源则请求会返回一个正常的http回应，但没有包含Access-Control-Allow-Origin字段， 因此包含一个XMLHTTPRequest错误。包含服务器则会返回正确的信息，但会多加几个头信息字段
  Access-Control-Allow-Origin: http://api.bob.com
  Access-Control-Allow-Credentials: true
  Access-Control-Expose-Headers: FooBar
  Content-Type: text/html; charset=utf-8
  与cors相关的字段都以access-control开头
  （2）非简单请求
  非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json
  非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。
  浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。
```

* 通用的事件侦听器

```
markyun.Event = {
        // 页面加载完成后
        readyEvent : function(fn) {
            if (fn==null) {
                fn=document;
            }
            var oldonload = window.onload;
            if (typeof window.onload != 'function') {
                window.onload = fn;
            } else {
                window.onload = function() {
                    oldonload();
                    fn();
                };
            }
        },
        // 视能力分别使用dom0||dom2||IE方式 来绑定事件
        // 参数： 操作的元素,事件名称 ,事件处理程序
        addEvent : function(element, type, handler) {
            if (element.addEventListener) {
                //事件类型、需要执行的函数、是否捕捉
                element.addEventListener(type, handler, false);
            } else if (element.attachEvent) {
                element.attachEvent('on' + type, function() {
                    handler.call(element);
                });
            } else {
                element['on' + type] = handler;
            }
        },
        // 移除事件
        removeEvent : function(element, type, handler) {
            if (element.removeEventListener) {
                element.removeEventListener(type, handler, false);
            } else if (element.datachEvent) {
                element.detachEvent('on' + type, handler);
            } else {
                element['on' + type] = null;
            }
        },
        // 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
        stopPropagation : function(ev) {
            if (ev.stopPropagation) {
                ev.stopPropagation();
            } else {
                ev.cancelBubble = true;
            }
        },
        // 取消事件的默认行为
        preventDefault : function(event) {
            if (event.preventDefault) {
                event.preventDefault();
            } else {
                event.returnValue = false;
            }
        },
        // 获取事件目标
        getTarget : function(event) {
            return event.target || event.srcElement;
        },
        // 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
        getEvent : function(e) {
            var ev = e || window.event;
            if (!ev) {
                var c = this.getEvent.caller;
                while (c) {
                    ev = c.arguments[0];
                    if (ev && Event == ev.constructor) {
                        break;
                    }
                    c = c.caller;
                }
            }
            return ev;
        }
    };
```

* 内存泄漏

```
内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。(引用计数)
垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，变量进入环境即将变量标记为‘进入环境’，逻辑上永远无法释放进入环境变量所占用的内存；当变量离开时，记‘离开环境’.然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了（标记清除）

setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
```

* in 操作符

```
可以区分不存在的属性和存在但是值为undefined的属性
let a = {b:undefined};
console.log('b' in a) //true
```

* typeof 返回类型字符串 全是小写字母

```
typeof(Function) // 'function'
typeof(new Function) // 'function'
typeof(Array) // 'function'
typeof(new Array) // 'object'
```

* 变量赋值的时候返回值

```
name = 456; // 456
var name = 456; // undefined
```

* hasOwnProperty

```
对象的hasOwnProperty用来检测对象的属性是否为自有属性，继承属性返回false
```

* 尾调用

```
函数的最后一步是调用另一个函数
function fn1(x){
  console.log(1);
  return fn2(x);
}
```
* 数组长度是根据下标的最大而确定的

```
var arr = [];
arr.aa = 1;
arr.length // 0  arr = [aa:1];

let obj = {
  1: 'a',
  2: 'b',
  length: 2
};
Array.prototype.push.call(obj, 'c');
obj //{1:'a',2:'c',length:3}
```

* delete 操作符

```
var a = 'aa'
delete window.a // false; 不可以删除
window.b = 'bb'
delete window.b // true; 可以删除
全局变量不可以删除， window对象上的变量可以直接删除
```


* classList api

```
ele.classList.remove('active');
ele.classList.add('active');
ele.classList.toggle('active');
ele.classList.contains('active');
```

* focus() 获得焦点 hasFocus() 判断是否获得焦点
* document.compatMode兼容模式‘CSS1Compat’

* 事件处理 true捕获 false冒泡

```
将时间处理程序添加到冒泡阶段能够最大限度的兼容各种浏览器
ele.addEventListener('click', callback, true);
ele.removeEventListener('click', callback, false);
// ie attach('onclick', callback) detach('onclick',callback)

html5事件：touchstart touchmove touchend touchcancel gesturestart
```

* window.requestAnimationFrame兼容处理

```
window._requestAnimationFrame = (function(){
  return window.requestAnimationFrame || function(callback){
    setTimeout(callback, 1000/60);
  };
})();
```

* 不同标签页的数据同步

```
每个标签页都在跑一个间隔执行的定时器，每次都去同步一次本次存储里的数据。如果检测到数据发生变化，就执行相应的操作来保证页面间的同步。本地存储的方式有很多。cookie、IE的userData、Flash的SharedObject、html5的localStorage。
```

* 纯函数

```
简单来说，一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。
const a = 1
const foo = (b) => a + b
foo(2) // => 3  非纯函数

const a = 1
const foo = (x, b) => x + b
foo(1, 2) // => 3 纯函数

const a = 1
const foo = (obj, b) => {
  obj.x = 2
  return obj.x + b
}
const counter = { x: 1 }
foo(counter, 2) // => 4
counter.x // => 2  非纯函数
```

* defer async 区别

```
script标签无defer和async属性时，浏览器会立即加载并执行脚本，不会等待后续的dom元素的载入。html解析->js下载->js执行->html解析
defer：dom元素的加载和js的加载是异步并行执行的，但是js的解析是在所有dom元素解析完成后，domcontentloaded之前执行的
html解析（并行js下载）->解析完成后->js执行
async：dom的加载和渲染和js的加载与执行是并行执行的。
html解析（并行js下载）->下载后直接执行js，执行期间block html解析->js执行结束后继续html解析
defer与async相同点是两者在下载js脚本是一样的，异步并行的，差别在执行时间，defer是html解析完后执行，async是下载完立即执行，defer按照加载顺序执行脚本的，async则乱序执行的，加载完就立即执行。
所有脚本都丢到 </body> 之前是最佳实践
```

* src 与href

```
src 即source，是引入，引入的文件会嵌入到当前标签所在的位置。常用的标签有iframe img script，当解析到该文件的时候浏览器会暂停页面的渲染，直到该资源加载完毕，这也是js脚本放在页面底部的原因。
href，即hyperText reference,是建立当前元素与文档之间的链接，常用的有link， a，
<link href="./index.scss" />
浏览器会识别该文档为scss文件，并行下载该文档，同时不会暂停对当前文档的处理，因此一般建议采用link而不是@import
总结：src是替换当前元素，href是建立当前文档与引用资源之间的关系。
```