# 内存泄漏操作
1. 意外的全局变量
[链接1](https://github.com/zhansingsong/js-leakage-patterns/blob/master/%E5%B8%B8%E8%A7%81%E7%9A%84JavaScript%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2/%E5%B8%B8%E8%A7%81%E7%9A%84JavaScript%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2.md)
[链接2](https://github.com/zhansingsong/js-leakage-patterns/blob/master/%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E4%B9%8BListeners/%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E4%B9%8BListeners.md)

* 意外的全局变量
* console.log变量未参加垃圾回收
* 闭包中变量
* DOM泄露(将DOM写在局部变量中,操作完后忘记释放)
* timers(定时器没清除)

原因:

比如在js的function中忘记写var或者是用window的this来声明,调用该函数就会导致在全局中生成该变量
```js
function test(){
  xx = '111';
  this.zz = '222'
}
test(); // window.xx 和 window.zz就会被生成
```
解决:

使用严格模式可以防止function中没有写var的情况
```js
function foo(arg) {
  "use strict" // 在foo函数作用域内开启严格模式
  bar = "111";// 报错：因为bar还没有被声明
}
```

# 闭包
## 介绍. 作用
作用
* 封装私有变量
* 模拟块级作用域(常见的for循环面试题)

## 模块化和闭包的关系
闭包是返回一个function,而模块化是返回一个由function为value的obj

即闭包是对函数内部变量的引用,而模块化是对函数内部函数的引用.

# 介绍es6的东西以及为什么使用es6
es6语法糖好用,避免es5的坑

* 定义变量 let const 块级作用域 变量提升
* 解构赋值
* 箭头函数
* 新增的对象方法 Array.isArray等
* 模板字符串
* class

## promise以外的异步
async generator

generator提供惰性求值,只有执行next,才会执行yield的值

## 改变this指向:call bind apply的区别
call 和 apply都是马上执行, call传入参数, apply传入数组
bind返回一个待函数

## js基本数据类型
number boolean string undefined symbol object function

## 两栏布局和双飞翼布局
两栏布局: 
* 左侧用left浮动 width设置宽度,右侧用margin-left将其宽度降低
* 左侧用left浮动 width设置宽度,右侧用overflow:hidden将其隐藏
* 在外部div上增加position:relative,左侧默认设置宽度,右侧使用 position:absolute来进行定位

圣杯布局 [代码](https://github.com/Linbubin/share/blob/master/css/shengbei.html?1550650276978)
中间自适应,两侧定宽, 中间优先展示渲染,允许任意列的高度设置

双飞翼布局 [代码](https://github.com/Linbubin/share/blob/master/css/shuangfeiyi.html)
在圣杯布局的基础上将将container中的左右翼拿到外部,删除position定位属性.将container的padding改成main-inner的margin-left和margin-right属性.

## 节流和防抖
> 防抖:多次触发事件后，事件处理函数只执行一次，并且是在触发操作结束时执行。<br>
> 节流:触发函数事件后，短时间间隔内无法连续调用，只有上一次函数执行后，过了规定的时间间隔，才能进行下一次的函数调用。

差别: 节流在防抖的基础上增加了一个最小时间差,当时间差到达时,则会执行该函数,免得重要的js代码被挂起.

## 跨域 为什么和解决
原因: 同源(协议+域名+端口)策略导致.

解决:
1. jsonp
2. 跨域资源共享(CORS)
* 后端设置Header Access-Control-Allow-Origin
3. WebSocket协议跨域
* 前后端均使用socket.io来进行数据传输 双方都有个 send 和 on方法

## position的属性
1. fixed
2. relative
3. absolute
4. static 默认值 没有定位 元素出现在正常的流中(忽略 top, bottom, left, right)
5. inherit 继承父元素

## react中深拷贝和浅拷贝
* 浅拷贝会由于引用赋值的原因，b修改会导致a修改
* 深拷贝可以用 Object.assign({}, xxx)或解构赋值{...obj} 或 lodash的cloneDeep 或 JSON.parse(JSON.string(obj))

## react生命周期
componentWillMount
render
componentDidMount

componentWillReceiveProps
shouldComponentUpdate
componentWillUpdate
will
componentDidUpdate

> getDerivedStateFromProps 替代 componentWillReceiveProps

## 性能优化,react中的性能优化
1. react中的性能优化
* shouldComponentUpdate增加判断
* 没有操作的组件尽可能使用无状态组件
* map中的key不要用index

2. html性能优化
* 多使用内存、缓存,减少请求次数
* 加载页面和静态资源 - 静态资源的压缩合并、静态资源缓存、使用CDN让资源加载更快、使用ssr后端渲染,数据直接输出到HTML中
* 页面渲染 - CSS放前面,JS放后面、懒加载(图片懒加载-先src一个很小的图片,data-src写原图,在js里面取到data-src里面的原图地址,赋值给src、下拉加载更多)、减少DOM查询、减少DOM操作、事件节流(input框 输入马上搜索 keydown会连续按,就clearTimeout 停下来再去查)、尽早执行操作(DOMContentLoaded)

## css3动画
* animation + keyframes 设置进度百分比
## web安全问题

## h5 存储方案
1. sessionStorage 会话级存储   会话结束(浏览器关闭)时,会话级数据即消失.  -- getItem setItem
2. localStorage 本地级存储 只要不执行localStorage.clear()就永远存在.
3. cookie cookie