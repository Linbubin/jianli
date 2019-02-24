# 常见状态码301 302
301 永久重定向
302 临时重定向
* 永久重定向会被浏览器缓存,除非清缓存,否则后端改代码也会重定向.
* 临时重定向当url被重新输入时，就会再根据服务器的返回进行判断.
* 有点类似于localstorage和sessionstorage的区别

# axios Ajax fetch区别
* Ajax是指Jquery中,要使用这一个功能要引入整个Jquery不值得
* axios是一种对Ajax的封装，fetch是一种浏览器原生实现的请求方式，跟Ajax对等
* axios客户端支持防止CSRF/XSRF（伪脚本攻击）,fetch默认不会带cookie，需要添加配置项(`credentials: 'include'`)
* fetch对400、500等状态码不会reject(自己改造判断200-300之间状态码,或者response.ok),而且不能设置timeout(自己改造加定时器reject)
* Ajax如果指的是传统的XMLHttpRequest(XHR),和他们区别就是缺少现代化的类promise.
* fetch 不支持同步请求,fetch 不支持取消一个请求(xhr.abort()),fetch 无法查看请求的进度(xhr.onprogress)

# 跨域里带有头信息,header信息从哪里读取
服务端发送跨域请求时,header信息增加Access-Control-Expose-Headers值

# 兄弟组件之间没有共同父组件,他们的信息该怎么传递

# 描述下diff算法是怎么回事

# react中的key是用来干嘛的
key是一个字符串，用来唯一标识同父同层级的兄弟元素。当React作diff时，只要子元素有key属性，便会去原v-dom树中相应位置（当前横向比较的层级）寻找是否有同key元素，比较它们是否完全相同，若是则复用该元素，免去不必要的操作。

# redux工作原理,数据流怎么样
用户发出Action，Reducer更新state，View重绘
1. 页面触发事件由Action Creators返回action转发给Store 
2. Store再将当前state和收到的action转给Reducer 
3. Reducer处理后返回一个新state反馈给Store 
4. Store管理的state改变了
5. 就会引发React组件的重新渲染

# 页面有20条数据同时需要修改sql怎么操作
先创建一个临时表,再进行条件修改,postgresql可以看[这里第二条](https://github.com/Linbubin/share/tree/master/postgresql)