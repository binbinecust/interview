* offsetWidth/offsetHeight,clientWidth/clientHeight与scrollWidth/scrollHeight的区别
offsetWidth/offsetHeight返回值包含content + padding + border，效果与e.getBoundingClientRect()相同
clientWidth/clientHeight返回值只包含content + padding，如果有滚动条，也不包含滚动条
scrollWidth/scrollHeight返回值包含content + padding + 溢出内容的尺寸
clientWidth=5+200+5
scrollWidth=5+实际宽度+5
offsetWidth=5+5+200+5+5

* import 和 require的区别；import {} 和没有{}的区别
遵循规范：require是amd规范 import是es6规范
调用时间：require是运行时调用，可以放在代码的任意地方，import是编译时调用，放在文件开头
本质：require是赋值过程，import是解构过程，浏览器需要对其进行bable转换
https://segmentfault.com/a/1190000010426778

exports = module.exports = {};指向同一块内存
其实require导出的内容是module.exports的指向的内存块内容，并不是exports的。
简而言之，区分他们之间的区别就是 exports 只是 module.exports的引用，辅助后者添加内容用的。

export与export default均可用于导出常量、函数、文件、模块等
在一个文件或模块中，export、import可以有多个，export default仅有一个
通过export方式导出，在导入时要加{ }，export default则不需要
export能直接导出变量表达式，export default不行。