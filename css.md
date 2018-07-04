## 清除浮动
`.parent:after {
  clear: both;
  display: block;
  content: '',
}

.parent {
  *zoom: 1;
}`

## 关于居中
* 定宽
`{
  margin: 0 auto;
}`

* 不定宽 水平垂直居中
`{
  position: absloute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}`

`{
  position: absloute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: 0 auto;
}`

* flex布局 >ie10
`.parent {
  display: flex;
  justify-content: center;
  align-item: center;
}`

* 水平居中
`.parent {
  text-align: center;
}
.child {
  display: inline-block;
}`

`{
  text-align:center;
  line-height: 20px;
  height: 20px;
}
`
* 绝对定位，定宽高的
`{
    width:100px;
    height:100px;
    position:absolute;
    top:50%;
    left:50%;
    margin-top:-50px;
    margin-left:-50px;
}`

## css hack
`
<!--[if !IE]><!--> 除IE外都可识别 <!--<![endif]-->
<!--[if IE]> 所有的IE可识别 <![endif]-->
<!--[if IE 8]> 仅IE6可识别 <![endif]-->
<!--[if lt IE 8]> IE8以下版本可识别 <![endif]-->
<!--[if gte IE 8]> IE8以及IE8以上版本可识别 <![endif]-->
`

## 布局
定宽加自适应
`.left {
  width: 200px;
}
.right {
  width: calc(100% - 200px)
}`

flex布局

## top和translate来移动的区别
top是cpu通过计算来进行渲染的，translate是GPU计算渲染的；

CPU可以同时做其他的一些计算；所以translate的性能优于top

## animation
animation-name 动画名称

animation-duration 动画时间

animation-timing-function 速度曲线，开始结束的方式

animation-delay 开始播放前的延迟时间

animation-iteration-count 播放次数

animation-direction 是否反向播放

`
#id {
  animation: mymove 10s infinite;
}

@keyframes mymove {
  from {
    left: 0;
  }
  to {
    left: 200px;
  }
}
`

## translate transform transition区别
translate: 移动，transform的一个方法

用法 transform: translate(10px, 20px);

transform: 变形

transform: skew()/scale()/translate()

transition: 允许css属性值在一定时间区间内平滑过渡

transition: property duration timing-function delay;