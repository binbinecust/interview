## 矩阵转置

```
function changeArr(arr) {
  for(var i = 0;i< arr.length;i++) {
    for (var j = 0; j< i; j++) {
      [arr[i][j], arr[j][i]] = [arr[j][i], arr[i][j]];
    }
  }
  return arr;
}
```

## 冒泡排序

```
function sort(arr) {
  for (var i = 0,len = arr.length; i<len; i++) {
    for (var j = i+1;j<len;j++ ) {
      if (arr[i] > arr[j]) {
        [arr[i], arr[j]] = [arr[j], arr[i]]
      }
    }
  }
  return arr;
}
```

## 二分法查找数组位置
```
function twoFind(arr, target, startIndex, endIndex) {
  if (startIndex > endIndex) {
    console.log('找你妹啊');
    return;
  }
  var midIndex = Math.floor((startIndex + endIndex)/2);
  if (arr[midIndex] > target) {
    twoFind(arr, target, startIndex, midIndex -1);
  } else if (arr[midIndex] < target) {
    twoFind(arr, target, midIndex + 1, endIndex);
  } else {
    console.log('我找到了，位置是：' + midIndex)
  }
}
```

## sort排序
```
function compare(propertyName) {
  return function (obj1, obj2) {
    if (obj1[propertyName] > obj2[propertyName]){
      return 1;
    } else if (obj1[propertyName] < obj2[propertyName]) {
      return -1;
    } else {
      return 0;
    }
  }
}
[{name: 'aa',age: 20}, {name: 'bb', age: 10}].sort(compare('name')); // 按age升序排列；
```

## 闭包
```
result[i] = (function(num){
  return function(){
    return num;
  }
})(i)
```

## sleep函数(将程序挂起一段时间)
```
1>普通版
function sleep(time){
  for(var start = +new Date;+new Date-start<=time;){}
};
var t1 = +new Date()
sleep(3000)
var t2 = +new Date()
console.log(t2 - t1)
2>promise版本
function sleep(time) {
  return new Promise(res => setTimeout(res, time));
};
let t1 = + new Date()
sleep(3000).then(() => {
  let t2 = +new Date();
  console.log(t2 - t1);
})
3>async await 版本
function sleep(time) {
  return new Promise(res => setTimeout(res, time));
}
(async function(){
  let t1 = +new Date();
  await sleep(3000);
  let t2 = +new Date();
  console.log(t2 - t1);
})()
```

## 数字格式化
```
'123456789'.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
'12345678'.split('').reverse().reduce((acc, curV, curI) => {
  return (curI%3 ? curV : curV+',') + acc;
})
12345678..toLocaleString('en-US')
```

## 数字取整
```
~~72.2233 //72
~~2 //2
~~[] //0
~~NaN //0
```

## push pop creat改写
```
let obj = {
  1: 'a',
  2: 'b',
  length: 2
};
Array.prototype.push.call(obj, 'c');
obj //{1:'a',2:'c',length:3}

Array.prototype.push2 = function(){
  this.splice.apply(this, [this.length, 0].concat(Array.prototype.slice.apply(arguments)));
  return this.length;
}

Array.prototype.pop2 = function(){
  return this.splice(this.length - 1, 1)[0];
}

Object._create = (o) => {
  let fn = function(){};
  fn.prototype = o;
  return new fn;
}
```

## 函数柯理化
```
function curry(fn){
  let args = [];
  return function callback(){
    if (!arguments.length){
      return fn.apply(this, args);
    }
    args.push(...arguments);
    return callback;
  }
}

var count = curry(function(...rest){
  return rest.reduce((acc, curV) => acc+curV);
});

count(10)(20)(30)();
```


## 收集参数达到一定次数再执行
```
function fn(...rest){
  console.log(rest);
}

function after(fn, time=1) {
  let args = [];
  return function (){
    args.push(...arguments);
    if (!--time){
      fn.apply(this, args);
    }
  }
}

var newFn = after(fn, 3);
newFn(10);
newFn(20);
newFn(30);
```

## 节流函数
```
function throttle(fn, delay=50){
  let start = 0;
  return function (){
    let now = +new Date;
    if(now - start >= delay){
      fn.apply(this, arguments);
      start = now;
    }
  }
}

let my_throttle = throttle(() => console.log(1), 1000);

document.onscroll = function(){
  my_throttle();
}
```

## 深拷贝
```
function deepCopy(obj) {
    if (typeof obj !== 'object') return obj;
    if (typeof window !== 'undefined' && window.JSON) { // 浏览器环境下 并支持window.JSON 则使用 JSON
        return JSON.parse(JSON.stringify(obj));
    } else {
        let newObj = obj.constructor === Array ? [] : {};
        for(let key in obj) {
            newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key];
        }
        return newObj;
    }
}
```

## 克隆
```
var cloneDeep = (obj) => {
  if (typeof obj !== 'object' || obj === undefined || obj === null || obj !== obj) {
    return obj;
  }
  switch (Object.prototype.toString.call(obj)) {
    case '[object Array]':
      return obj.reduce((acc, cur) => acc.concat(cloneDeep(cur)), [])
      break;
    case '[object String]':
      return new String(obj);
      break;
    case '[object Date]':
      return new Date(obj);
      break;
    case '[object RegExp]':
      return new RegExp(obj);
      break;
    case '[object Number]':
      return new Number(obj);
      break;
    case '[object Object]':
      return Object.entries(obj).reduce((acc, [key, value]) => {
        return Object.assign(acc, {[key]: cloneDeep(value)});
      }, {});
      break;
  }
}
```

## bind pollfill
```
Function.prototype._bind = function(context){
  let args = [...arguments].slice(1, arguments.length);
  let _this = this;
  return function(){
    args.push(...arguments);
    _this.apply(context, args);
  }
}
```

## 函数联合串联compose
```
let fn1 = (a) => a + 1;
let fn2 = (b) => b + 2;
let fn3 = (c) => c + 3;

let funs = [fn1, fn2, fn3];

let compose = (func) => {
    return arg => func.reduceRight((composed, fn) => fn(composed), arg);
}
console.log(compose(funs)(100));
```

## promisefy调用文件
```
module.exports = {
  promisify(fn){
    return function(){
      let args = [...arguments];
      new Promise((res, rej) => {
        fn.apply(null, args.concat((err, data) =>{
          if (err){
            rej(err)
          } else{
            res(data)
          }
        }))
      })
    }
  }
}

const fs = require('fs');
const {promisify} = require('./promisefy.js');
const readFile = promisify(fs.readFile);
readFile('./test.js').then((data) => {
  console.log(data);
})
```

## 插入排序
```
function insertSort(arr) {
  let len = arr.length;
  let preIndex, current;
  for (let i = 1; i < len; i++) {
    preIndex = i - 1;
    current = arr[i]; // 当前项
    while (preIndex >= 0 && arr[preIndex] > current) {
      arr[preIndex + 1] = arr[preIndex]; // 如果前一项大于当前项 则把前一项往后挪一位
      preIndex-- // 用当前项继续和前面值进行比较
    }
    arr[preIndex + 1] = current; // 如果前一项小于当前项则 循环结束 则将当前项放到 前一项的后面
  }
  return arr;
}
```

## 选择排序
```
function selectSort(arr) {
  let len = arr.length;
  let temp = null;
  let minIndex = null;
  for (let i = 0; i < len - 1; i++) { // 把当前值的索引作为最小值的索引一次去比较
    minIndex = i; // 假设当前项索引 为最小值索引
    for (let j = i + 1; j < len; j++) { // 当前项后面向一次比小
      if (arr[j] < arr[minIndex]) { // 比假设的值还要小 则保留最小值索引
        minIndex = j; // 找到最小值的索引位置
      }
    }
    // 将当前值和比较出的最小值交换位置
    if (i !== minIndex) {
       temp = arr[i]
       arr[i] = arr[minIndex];
       arr[minIndex] = temp;
    }
  }
  return arr;
}
```
## 字符串去除相邻的重复项 如：'aabbccddeexxxxaa' => 'abcdexa'
```
function uniq(str){
  let arr = str.split('');
  return arr.reduce((acc, curV) => {
    if (curV !== acc[acc.length - 1]){
      acc.push(curV);
    }
    return acc;
  }, [arr[0]]).join('');
}
```

##求相邻两项最大和
```
function max(arr){
  let newArr = arr.reduce((acc, cur, curI) => {
    if (curI<arr.length-1){
      acc.push(cur + arr[curI+1]);
    }
    return acc;
  }, []);
  return Math.max(...newArr);
}
```
## 数组扁平化
```
const unzip = (arr) => {
  return arr.reduce((acc, cur) => {
          if (!Array.isArray(cur)) {
            return acc.concat(cur);
          } else {
            return acc.concat(unzip(cur));
          }
        }, []);
}
unzip([1,2,3,[2,3,[2,4]],[1] ]);
```

##深度控制扁平化数组
let flatten = (arr, depth=1) => {
  let i = 1;
  return arr.reduce((acc, cur) => {
    if (Array.isArray(cur) && i<depth) {
      i++;
      return acc.concat(flatten(cur));
    } else {
      return acc.concat(cur);
    }
  }, [])
}

## fibnaci 数列
```
const fibnac = (n) => {
  if (n<=2) {
    return 1;
  }
  return fibnac(n-1) + fibnac(n-2);
};

fibnac(20)
---------------------------------
优化版

function fibnacii(n) {
  let arr = [];
  (function fib(n) {
    debugger;
    if (n<=0) {
      return 0;
    }
    if (n === 1) return 1;
    arr[0] = fib(n-1);
    arr[1] = fib(n-2);
    return arr[0] + arr[1];
  })(n);
};

fibnacii(10)

function fibnaci(n) {
  if (n<=0) return 0;
  if (n==1) return 1;
  let pre2 = 0;
  let pre1 = 1;
  let res = 1;
  for (var i = 2; i<=n; i++) {
    res = pre2 + pre1;
    pre2 = pre1;
    pre1 = res;
  }
  return res;
}

function fibnac(n) {
  const arr = [];
  function fib(n) {
    if (n<=0) return 0;
    if (n==1) return 1;
    if (arr[n]) return arr[n];
    const v = fib(n-1) + fib(n-2);
    arr[n] = v;
    return v;
  }
  return fib(n);
}
fibnac(20)
```

##走楼梯
```
const goStair = (n) => {
  if (n===1) {
    return 1;
  } else if (n===2) {
    return 2;
  } else {
    return goStair(n-1) + goStair(n-2);
  }
};
goStair(20);
```

##求和
```
const add = (n) => {
  if (n<=0) {
    return 0;
  }
  return n + add(n-1);
}

add(50);
```

##求最大公约数
```
const num = (a,b) => {
  if (a===b) return a;
  return num(Math.min(a,b), Math.abs(a-b));
};

num(40,20);
```

##快速排序法
```
const quickSort = (arr) => {
  if (arr.length <= 1) return arr;
  let midIndex = Math.floor(arr.length/2);
  let midValue = arr.splice(midIndex, 1);
  const leftArr = [];
  const rightArr = [];
  for (let i=0; i<arr.length; i++) {
    if (arr[i]<= midValue) {
      leftArr.push(arr[i])
    } else {
      rightArr.push(arr[i]);
    }
  };

  return quickSort(leftArr).concat(midValue, quickSort(rightArr));
};

quickSort([1,3,6,8,9,7,5,4,3,3,2,5,6,8,9,0,7,4,3,44,5,6,76])
```

##求dom子节点
```
const allChildrenArr = (parent)  => {
  const allArr = [];
  const getChildren = (parent) => {
    if (parent.children.length) {
      [...parent.children].map(children => {
        if (children.children.length) {
          return getChildren(children);
        } else {
          return children;
        }
      });
    }
    allArr.push(parent);
    return allArr;
  };
  return getChildren(parent);
};

allChildrenArr(document.body);
```

##dom树遍历
```
var domLoop = (node) => {
  if (node.nodeType ===1) {
    console.log(node.tagName);
  };
  if (node.children.length) {
    [...node.children].forEach(son => {
      domLoop(son);
    })
  }
}
```

##dom树最大深度
```
let domDeepth = (node) => {
  let i=0;
  (function fn(node, i) {
    [...node.children].forEach((son) => {
        if (son.children.length) {
          fn(son, i);
        } else {
          i++;
        }
      }
    );
  })(node, i);
  return i;
}
```

##字符串回文
```
const isReverse = (str) => {
  return str.split('').reverse().join('') === str;
}

isReverse('abc');
```

##去重
```
[...new Set([1,2,4,1,2,4,5,6,4,3,5,6])];

const dedul = (arr) => {
  const obj = {};
  for (let i = 0; i< arr.length; i++) {
    if (!obj[arr[i]]) obj[arr[i]] = 'a';
  }
  return Object.keys(obj).map(x => +x);
}

dedul([1,2,3,4,5,3,43,4,3,2,2,2,2]);
```

##计数，输出最多的字符
```
const mostShow = (str) => {
  const strArr = str.split('');
  const charObj = {};
  for (let i = 0,len=strArr.length; i< len; i++) {
    if (!charObj[strArr[i]]) {
      charObj[strArr[i]] = 1;
    } else {
      charObj[strArr[i]] +=1;
    }
  }
  let maxCount = 1;
  let res;
  for (let key in charObj) {
    if (charObj[key] >= maxCount) {
      maxCount = charObj[key];
      res = key;
    }
  }
  return res;
};

mostShow('abbcccdddd11111111');
```

##最大差值
```
const maxdiff = (arr) => {
  return Math.max(...arr) - Math.min(...arr);
}

maxdiff([1,3,4,5,6,6,7,3,34,5,5,3])
```

##随机长度字符串
```
const randomString = (num) => {
  return Math.floor(Math.random()*Math.pow(10, num)).toString(16);
}

randomString(5);
```

##字符串下划线转驼峰
```
const toCamelCase = (str) => {
  return str.replace(/\_(\w)/g, (match, item) => {
    console.log(match, item);
    return item.toUpperCase();
  });
};
//正则为/\_(\w)/g 要有括号才能匹配
toCamelCase('i_love_china');
```

##字符串驼峰转下划线
```
const toLineCase = (str) => {
  return str.replace(/([A-Z])/g, (match, item) => {
    console.log(match,item);
    return '_' + item.toLowerCase();
  });
};
toLineCase('iLoveChina');
```
##一维数组转二维数组
```
const toDulArray = (arr, num) => {
  const res = [];
  let arrLength = arr.length;
  let resLength = Math.ceil(arrLength/num);
  let index = 0;
  for (let i=0;i<resLength;i++) {
    res[i] = [];
    for (let j=0;j<num;j++){
      if (arr[index]){
        res[i][j] = arr[index];
        index++;
      }
    }
   // res[i] = res[i].filter(item => item);
  }
  return res;
}
toDulArray([1,2,3,4,5,6,7,8,90,1,2,4,6,7,8,9],3)
```

##数组去重obj
```
let toDulArray = (arr) => {
  const obj = {};
  arr.forEach((item, index) => {
    obj[item] = index;
  });
  return Object.keys(obj).map(item => +item);
}
```

##获取重复值
```
let getDul = (arr) => {
  let obj = {};
  let res = [];
  arr.forEach(item => {
    if (obj[item + typeof item]) {
      res.push(item);
    } else {
      obj[item + typeof item] = item;
    }
  })
  return res;
}
```

##对象键的最大值
```
var valueMax = (obj) => {
  let res = Object.values(0);
  Object.entries(obj).forEach(([key, value], index) => {
    if (value>res) {
      res = value;
    }
  })
  return res;
}
```

##字符串模板功能
```
//format('hello ${name}', {name: 'word'}) => hello, world
var format = (temp, rep) => {
  return temp.replace(/\$\{([^\{]+)\}/g, (match, item) => {
    console.log(match, item);
    return item = rep[item];
  })
}
```

## 字符串去除左右两边空格
```
'   fdsfds  '.replace(/(^\s+|\s+$)/g, '');
```

//++i前置加，先加完再执行当前语句 , i++后置加，先执行当前语句，在加1，后面的i会改变，当前不会

##lazyman 实现
```
class lazyMan{
  constructor(name) {
      this.tasks = [];
      const first = () => {
          console.log(`my name is ${name}`);
          this.next();
      }
      this.tasks.push(first);
      setTimeout(()=>this.next(), 0);
  }
  next() {
      const task = this.tasks.shift();
      task && task();
  }
  eat(food) {
      const eat = () => {
          console.log(`eat ${food}`);
          this.next();
      };
      this.tasks.push(eat);
      return this;
  }
  sleep(time) {
      const newTime = time * 1000;
      const sleep = () => {
          console.log(`sleep ${time}s!`);
          setTimeout(() => {
              this.next();
          }, newTime);
      };
      this.tasks.push(sleep);
      return this;
  }
  sleepFirst(time) {
      const newTime = time * 1000;
      const sleepzFirst = () => {
          console.log(`sleep ${time}s first!`);
          setTimeout(() => {
              this.next();
          }, newTime);
      };
      this.tasks.unshift(sleepzFirst);
      return this;
  }
}
const aLazy = new lazyMan('王大锤');
aLazy.eat('苹果').eat('香蕉').sleep(5).eat('葡萄').eat('橘子').sleepFirst(2)
```

##koa 中间件的执行队列的处理
```
   compose(middleware)默认会执行中间件数组的第一个，即dispatch（0），然后通过执行索引加1的中间件，直到数组中所有的中间件执行完毕
   ，第一个中间件通过await next（）返回第二个中间件的执行，第二个中间件通过await next（）返回第三个中间件的执行，中间件处理完毕后
   接下来就是通过向中间件中传递的context来对请求进行处理。
   与layzman不同的是，layzman通过shift切割数组来对队列进行处理， 而koa是通过对数组索引（+1）进行处理。
```
##数组的排列组合
```
//[3,2,6,9] => ["3-2", "3-6", "3-9", "3-2-6", "3-2-9", "3-2-6-9", "2-6", "2-9", "2-6-9", "6-9"];
var fn = (arr) => {
  let res = [];
  while(arr.length) {
    let index = 0;
    while(index++ < arr.length) {
      let tempArr = arr.slice();
      let headArr = tempArr.splice(0, index);
      let headString = headArr.join('-');
      tempArr.forEach(item => {
        res.push(headString + '-' + item);
      })
    }
    arr.shift();
  }
  return res;
}
fn([3,2,4,5]);
```
##数组解构
```
// destructuringArray([1, [2, 3], 4], '[a, [b], c]') => {a: 1, b: 2, c: 3} 数组解构
function distruct(arr, str){
  let str2Arr = JSON.parse(str.replace(/(\w+)/g, '"$1"'));
  const res = {};
  return (function _distruct(keyArr, valueArr) {
    return keyArr.reduce((acc, curValue, curIndex) => {
      if (Array.isArray(curValue)){
        _distruct(curValue, valueArr[curIndex]);
      } else {
        acc[curValue] = valueArr[curIndex];
      }
      return acc;
    }, res);
  })(str2Arr, arr);
}


distruct([1, [2, 3], 4], '[a, [b], c]');
```


##快速排序
```
function sort(arr) {
  let len = arr.length;
  function partition(arr, start, end) {
    let x = arr[end];
    let i = start - 1;
    for (var j = start; j <=end - 1; j++) {
      if (arr[j] <= x) {
        i++;
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
    }
    [arr[i+1], arr[end]] = [arr[end], arr[i+1]];
    return i+1;
  }

  void (function quicksort(arr, start, end) {
    if (start < end) {
      let dividIndex = partition(arr, start, end);
      quicksort(arr, start, dividIndex);
      quicksort(arr, dividIndex+1, end);
    }
  })(arr, 0, len);
  return arr;
}
```

##函数柯里化
```
function curry(fe){
  let args = [];
  let len = args.length;
  return function bar(){
    args = [...args, ...arguments];
    if (args.length >= fe.length) {
      return fe.apply(this, args);
    }
    return bar;
  }
}

function curry(fn){
  let args = [];
  return function callback(){
    if (!arguments.length){
      return fn.apply(this, args);
    }
    args.push(...arguments);
    return callback;
  }
}

var count = curry(function(...rest){
  return rest.reduce((acc, curV) => acc+curV);
});

count(10)(20)(30)();
```
```
function add(x) {
  var sum = x;
  var tmp = function (y) {
      sum = sum + y;
      return tmp;
  };
  tmp.toString = function () {
      return sum;
  };
  return tmp;
}
console.log(add(1)(2)(3));  //6
console.log(add(1)(2)(3)(4));
```

## 函数执行若干次才执行
```
function fn(...rest){
  console.log(rest);
}

function after(fn, time=1) {
  let args = [];
  return function (){
    args.push(...arguments);
    if (!--time){
      fn.apply(this, args);
    }
  }
}

var newFn = after(fn, 3);
newFn(10);
newFn(20);
newFn(30);
```
##节流函数
```
function throttle(fn, delay=50){
  let start = 0;
  return function (){
    let now = +new Date;
    if(now - start >= delay){
      fn.apply(this, arguments);
      start = now;
    }
  }
}

let my_throttle = throttle(() => console.log(1), 1000);

document.onscroll = function(){
  my_throttle();
}
```
##生成器函数
```
function* fn(a){
  a = yield a;
  let b = yield 2;
  let c = yield 3;
  return a + b + c;
}

function co(fn, ...args){
  let gen = fn(...args);
  return new Promise((res, rej) => {
    void (function next(lastval){
      let {value, done} = gen.next(lastval);
      if (done){
        res(value);
      } else {
        if (value instanceof Promise){
          value.reject(value);
        } else {
          next(value);
        }
      }
    })()
  })
}

co(fn, 100).then(value => {
  console.log(value);
}).catch((e) => console.log(e.message))
```

##blueBird实现
```
module.exports = {
  promisify(fn){
    return function(){
      let args = [...arguments];
      new Promise((res, rej) => {
        fn.apply(null, args.concat((err, data) =>{
          if (err){
            rej(err)
          } else{
            res(data)
          }
        }))
      })
    }
  }
}

const fs = require('fs');
const {promisify} = require('./promisefy.js');
const readFile = promisify(fs.readFile);
readFile('./test.js').then((data) => {
  console.log(data);
})
```
##过滤数组重复的值
```
arr.filter((item, index, self) => {
  return self.indexOf(item) !== self.lastIndexOf(item);
});
```
##统计数组中值出现的次数
```
function sortArr(arr){
  return arr.reduce((acc, curV) => {
    let match = acc.find((item) => curV===item.value)
    if(match){
      match.times++;
    } else {
      acc.push({value: curV, times: 1});
    }
    return acc;
  }, [])
}
sortArr(arr1)
```
```
function uniq(str){
  let arr = str.split('');
  return arr.reduce((acc, curV) => {
    if (curV !== acc[acc.length - 1]){
      acc.push(curV);
    }
    return acc;
  }, [arr[0]]).join('');
}
```
```
function max(arr){
  let newArr = arr.reduce((acc, cur, curI) => {
    if (curI<arr.length-1){
      acc.push(cur + arr[curI+1]);
    }
    return acc;
  }, []);
  return Math.max(...newArr);
}
```
