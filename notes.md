
### 主流浏览器和内核
```
ie                Trident/EdgeHTML(最新IE内核)
chrome            Webkit/blink(2014年以后)
firefox           Gecko
opren             Presto
sarfie            Webkit
```
### JavaScript typeof 返回类型
` Function Boolean String Number Object undefined Symbol`
#### 类型转换
```
Number();
ParseInt();
ParseFloat();
String();
Boolean();
toString();

undefined == null   

Boolean([])
[] == false
[] == true

eg:布尔类型与其它任何类型进行比较，布尔类型将会转换为number类型。
```

### 变量的可读性，可写性，可删除性，可枚举性
```
var num1 = 0;  
num2 = 0;    
```
```
var x={age:18}
//在使用defineProperty()方法设置属性时，通常需要在对象内部维护一个新内部变量（以下划线_开头，表示不希望被外部访问），作为存取器函数的中介。
Object.defineProperty(x,'age',{
    value:16, //value set get  默认值undefined
    set:function(val){
        this._age = val
    },
    get: function () { // 读取该属性时候调用，并将该函数的返回值赋值给value
        return this._age += 1
    },
    writable:true,
    enumerable:true, // 是否可枚举
    configurable:true //该指定对象的描述 是否可变
})
```
**arguments.callee ==>指向函数本身    fun.caller ==> 指向函数的执行环境**

### 预编译

- js 运行 (语法分析 => 预编译 => 解释执行)
- 语法分析：通篇扫描，不执行，发现语法错误
```
<!--预编译发生在函数执行的前一刻-->
1.创建AO对象
2.找形参和变量声明，将形参名和变量作为AO的属性名，值为undefined
3.将实参和形参统一
4.找函数声明，并值赋予函数体
```



### 数组的方法
1.数组的基本方法分类
```
1. push() unshift() pop() shift() sort() reverse() splice() //这些方法可以改变原数组
2. concat() join() split() toString() slice() //产生新数组，不改变原数组
```

2.二维数组一维化
```
let arr = [1,2,[3,4],5]
let tmp=[]
// 方法一
arr.join(',').split(',')
//方法二
function newArr(arr){
    arr.map(i=>{
        if(Array.isArray(i){
            newArr(i)
        })else{
            tmp.push(i)
        }
    })
}
//let newArr = [].concat(...arr.map(i=>(Array.isArray(i)?newArr(i):i)))
```
3.数组去重
```
var arr = [1, 2, 2, 3, 3, 4, 4, 4, 5]
<!--方法一 利用ES6 Set数据结构属性-->
function arrayUnique(arr) {
    return [...(new Set(arr))] //es6语法
}
<!--方法二 利用对象属性名不重复原理-->
function arrayUnique(arr) {
    var obj = {},
        newArr = [],
        len = arr.length;
    for (var i = 0; i < len; i++) {
        if (!obj[arr[i]]) {
            obj[arr[i]] = 'a';
            newArr.push(arr[i])
        }
    }
    return newArr
}
```
4.数组循环
```
arr.forEach(), arr.Map(), arr.filter(), arr.some(), arr.every() == > 接受俩个参数 第一个是回调函数， 第二个是this的指向
回调函数接受三个参数 value index arr
//例子：arr.forEach(function(value,index,arr),{})
注意：
1. forEach代替for循环
2. map有返回值， 返回一个新数组， 一般都需要写return
3. filter返回一个新数组， 返回的是为true的数据
4. some查找数组中某个元素符合就返回true
5. every查找数组中每个元素是否都符合要求

arr.reduce(), arr.reduceRight() == > 回调函数接受四个参数 pre(上一个元素) cur（ 当前元素） index（ 当前元素索引） arr（ 循环数组）
```
5.给一个数组随机乱序
```
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
arr = arr.sort(function() {
    return Math.random() - 0.5
})
```
6.类数组
```
//类数组(1.属性为索引属性 2.必须有length属性 3.最好加上push方法)
var obj = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    'length': 3,
    'push': Array.prototype.push,
    'splice': Array.prototype.splice
}
```
### this的问题
1.关于this指向
```
    1.预编译阶段this指向window
    2.全局作用域里this指向window
    3.call/apply 可以改变this的指向
    4.obj.a();这时候谁调用this就指向谁 ==> 在这里指向obj
```
2.//call、apply和bind方法的区别
```
    call和apply都是用来调用方法，改变this指向的。
    call 第一个参数为对象，后面为实参；
    apply只有俩个参数，第一个为对象，第二个为参数数组（arguments）

    bind方法改变this指向，返回新函数 ，第一个参数改变this指向（null==> window  obj==>obj），其他参数为具体实参
```

### javaScript事件处理模型
- 1.事件冒泡
```
结构上嵌套的元素（非视觉上），事件从子元素冒泡向父元素。
```
- 2.事件捕获(Goole Chrome)
```
addEventListener(type,handle,true)  //true为事件捕获 false 为事件冒泡
结构上嵌套的元素，事件自父元素捕获至子元素。
```
- 3.事件模型触发顺序 先捕获 后冒泡
- 4. focus blur change submit reset select等事件不冒泡
- 5.阻止事件冒泡
```
W3C e.stopPropagation();
IE/Google  e.cancelBulle = true;
```
- 6.阻止默认事件
```
    1. return false 只有用on+type绑定的事件才可以(句柄事件)
    2. event.preventDefault(); W3C标准
    3. event.returnValue = false; IE
```

### 事件对象
- 1.兼容处理 IE浏览器中在window.event上，google浏览器上会自动封装成对象传入。 var e = e || window.event
- 2.事件源对象var target = e.target || e.srcElement

### js加载事件线
- js异步加载（1.IE:defer='defer',2.W3C:aysnc='aysnc',3.回调函数可以实现按需加载，最常用的方法）
```
function loadScript (src,callback) {
    var script = document.createElement('script');
    script.type = 'text/javascript';
    if(script.readyState){
        script.onreadystatechange = function () {
            if(script.readyState == 'complete' || script.readyState == 'loaded'){
                callback();
            }
        }
    }else{
        script.onload = function () {
            callback()
        }
    }
    script.src = src
    document.head.appendChild(script);
}
```
- 1、创建document对象，并开始解析web页面，解析HTML元素和内容，这个阶段document.readyState = 'loading';
- 2、遇到link引入的外部css，创建新的线程，并继续解析文档。
- 3、遇到没有defer和aysnc的script标签，阻塞加载，等待js加载完成，再继续解析文档
- 4、遇到设有defer或aysnc的script标签，创建新线程异步加载js，并继续解析文档。
- 5、遇到img等，先正常解析dom结构，然后异步加载src，并继续解析文档。
- 6、当文档解析完成，document.readyState = 'interactive'
- 7、文档解析完成后，所有设置有defer的脚本按顺序执行。
- 8、document对象触发DOMConetentLoad事件，开始识别浏览器事件。
- 9、当所有aysnc当所有页面和脚本下载并执行完成后，document.readyState = 'complete',window对象触发load事件。

### 正则表达式
- 1、转义符'\',把转义符后面的第一个内容强制转换成文本类型；
```
    \n (换行)
    \r （行结束）
    \t （table）
    多行字符串在行末加\
```
- 2.正则表达式属性（可以单独写也可以组合写）
```
var reg = \abc\i 
var reg = \abc\g  
var reg = \abc\igm
eg: i表示忽视大小写，g表示全局匹配，m表示多行匹配

var str = 'abcdf\na';
var reg = /^a/gm;
str.match(reg);
```
- 表达式 ==> []
```
1.^在表达式里表示非
2.(abc|bcd|ac) |表示或
3.
//选出字符串中连着都是三位的字符
var reg = /[0-9][0-9][0-9]/g;
var str = '1s23ad132sa1d32s1ad321sa3d21sa3';
```
- 元字符
```
1. \w === [0-9A-z_]
2. \W === [^\w]
3. \d === [0-9]
4. \D === [^\d]
5. \s === '表示空白的符号' [\t\n\f\r\v ]
6. \S === [^\s]
7. \b === '单词边界'
8. \B === '非单词边界'
9. . === [^/r/n]
```
- 量词
```
1. + 1 - infinity 
2. * 0 - infinity
3. ? 0 - 1
4. {x} x个
5. {x,y} x-y个
6. {x,} x-infinity
7. $ 以什么结尾
```

### 网络部分

##### 网络协议 tcp和udp
```
    tcp:基于链接的协议，传送数据之前必须要确认链接（三次握手）,能为应用程序提供可靠地通信链接，传输数据量大，传输速度慢。
    udp:不需要建立链接，直接发送数据，适用于一次性发送少量数据,对可靠性要求不高，传输数据量小，速度快。
```
```
 5层模型：应用层 => 传输层 => 网际层 => 网络接口  （应用层 => osi的应用层 表示层 会话层）
 7层模型：应用层 => 表示层 => 会话层 => 传输层 => 网络层 => 数据链路层=> 物理层
 对应的协议：
        应用层(Application)：HTTP,TFTP，FTP，WAIS,DNS,Telnet
        表示层(Presentation)：Telnet,Rlogin,SNMP,Gopher
        会话层(Session)：SMTP,DNS
        传输层(Transport)：TCP,UDP
        网络层(Network)：IP,ICMP,ARP,RARP,AKP,UUCP
        数据链路层(Data Link)：FDDI,Ethernet,Arpanet,PDN,SLIP,PPP
        物理层(Physical)：ISO规范
```
```
二者区别：
1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接
2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保   证可靠交付
3、TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流;UDP是面向报文的 UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）
4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
5、TCP首部开销20字节;UDP的首部开销小，只有8个字节
6、TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道
```
#####  http协议
   
```
1、简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
2、灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
3.无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
4.无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。
5、支持B/S及C/S模式。
```

 * url 协议部分 => 域名部分 => 端口部分(省略采用默认端口) => 虚拟目录 => 文件名 => 锚部分(#) => 参数部分(?) 
##### http请求
```
http请求 request  请求行(request line) => 请求头(header) => 空行 => 请求数据
1.请求行以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本。
2.请求头部，紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息
3.空行，请求头部后面的空行是必须的
4.请求数据也叫主体，可以添加任意的其他数据

http请求 response  状态行 => 消息报头 => 空行 => 响应正文
```
##### 状态码以及常见状态
```
http 状态码
    1xx:指示信息--表示请求已接收，继续处理
    2xx:成功--表示请求已被成功接收、理解、接受
    3xx:重定向--要完成请求必须进行更进一步的操作
    4xx:客户端错误--请求有语法错误或请求无法实现
    5xx:服务器端错误--服务器未能实现合法的请求
常见状态
    200 OK                        //客户端请求成功
    400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
    401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
    403 Forbidden                 //服务器收到请求，但是拒绝提供服务
    404 Not Found                 //请求资源不存在，eg：输入了错误的URL
    500 Internal Server Error     //服务器发生不可预期的错误
    503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

```

 ### 数据库
 
 #### 1.数据库分类
 
    |         | 关系型    |  非关系型  |
        :-:   | :-:       | :-:
    | 实时       | MySql、Oracle、DB2、SqlServer      |   MongoDB、Redis、Memcache   |
    | 非实时        | Hive     |   HBase、ElasticSearch  |

    2013年阿里巴巴提出去I(IBM小型机)O(Oracle)E(EMC存储设备)
    
#### 常用数据里命令

- show databases   ==>  显示手游的数据库。
- user 数据库名称  ==> 链接数据库
- show tables ==> 显示数据库里的所有表
- select keyword from tablename;
- create datebase '数据库名';
```
    创建表的规定：必须有唯一标识(id)，且不能有任何意义。
    常用type字段分类：
        1.整数int
        2.长整数bigint(21)
        3.浮点数float
        4.双精度浮点数double
        5.字符串varchart(num) num为字符串长度
        6.文本text
    表常用引擎：
        1.MYISAM：读很多，写的很少，写是表级锁。
        2.InnoDB：读较多，写较多，行级锁。
```
- insert into 表名 ( `字段`,`字段`) values (1,'值') ==> 增加数据
- update tablename set age =  19 where name = '熊猫' ==> 修改数据要添加限制条件，否则会修改全表字段
- select name,age from test ==> 查询命令
```
    <!--tests为表名-->
    select * from tests where age = 18 or age = 19; //or为或  and为且
    select count(1) from tests;
    select sum(age) from tests;
    select avg(age) from tests;
    select age,count(1) from tests group by(age);
    select * from tests limit 2,2;  //前面是偏移量，后面是每一页的个数
    select * from tests order by id desc limit 2,2; //加desc为倒序 不加为顺序
```
- delete from tests where name = '熊猫';

### 一些方法的封装

1.对象原型继承(圣杯模式的封装）
```
function inherit(function() {
    var F = function() {};
    return function(Son, Father) {
        F.prototype = Father.prototype;
        Son.prototype = new F();
        Son.prototype.constructor = Son;
        Son.prototype.uber = Father; //方便以后找son的构造函数
    }
})()
```
2.对typeOf方法的扩展
```
function myType(d) {
    let tmp = {
        '[objcet Object]': 'object',
        '[objcet Array]': 'Array',
        '[objcet Number]': 'object number',
        '[objcet Boolean]': 'object Boolean',
        '[objcet String]': 'object String',
    }
    if (typeof d === null) {
        return 'null'
    } else if (typeod d == 'objict') {
        var str = Object.prototype.toString.call(d);
        return tmp[str];
    } 
    return typeof d;
}
```
3.异步加载js的封装
```
function loadScript(url,callback){
    var script = document.createElement('script')
    script.type='text/javascript'
    if(script.readyState){
        script.onreadystatechange=function(){
            //判断是否是IE
            if(script.readyState=='complete'||script.readyState=='loaded'){
                callback()
            }
        }
    }else{
        script.onload=function(){
            callback()
        }
    }
    script.scr=url
    document.head.appendChild(script)
}
```
4.ajax上传文件封装
```
var updata={
    config:{
        url:'',//上传地址
        format:'',//文件格式
        async:true,
        files:[],
        filenmae:'files',
        success:function(){
            //成功回调
        },
        error:function(){
            // 失败回调
        }
    },
    update:function(){
        var _this = this
        let xhr =new new XMLHttpRequest();
        let formData = new FormData();
        this.files.forEach( (element, index) =>{
            formData.append(this.filenmae,element)
        });
        xhr.onreadystatechange = function(){
            if(xhr.readyState == 4){
                if(xhr.status == 200){
                    _this.config.success(xhr.responseText)
                }else{
                    _this.config.error(xhr.responseText)
                }
            }
        }
        xhr.open('POST', this.config.url, this.config.async)
        xhr.send()
    },
    init:function(config){
        if(Object.prototype.toString().call(config) !== '[objcet Object]'){
            throw '配置类型为一个对象'
            return false
        }
        this.config = Object.assign(this.config,config)
        this.update()
    }
}
```
5.对象克隆 深度拷贝
```
function deepCloneObj(orgin, trget) {
    var trget = trget || {},
        toStr = Object.prototype.toString,
        arrStr = '[object Array]';
    for (var i in orgin) {
        if (orgin.hasOwnProperty(i)) { //==>判断是否是原型链上的属性 hasOwnProperty
            if (typeof orgin[i] == 'object' && orgin[i] !== 'null') { //==>判断是否是原始值
                trget[i] = toStr.call(orgin[i]) == arrStr ? [] : {}; //判断是否是数组
                deepCloneObj(orgin[i], trget[i])
            } else {
                trget[i] = orgin[i]
            }
        }
    }
    return trget;
}
```
6.模拟实现bind
```
Function.prototype.myBind = function(target) {
    target = target || window;
    //self 指向调用的函数
    var self = this
    var args = [].slice.call(arguments, 1); //==>类数组转换为数组(除了类数组第一位)
    var temp = function() {};
    var F = function() {
        var _args = [].slice.call(arguments, 0);
        return self.apply(this instanceof target ? this : target, args.concat(_args));
    }
    temp.prototype = this.prototype;
    F.prototype = new temp();
    return F;
}
```
7.纯函数和记忆函数
```
纯函数 ==> 不依赖、修改其作用域之外变量的函数   ==> 减少bug
函数记忆  性能优化 效率更高 内存占用大


// 例子 ==>常规写法  if n=5 count==>15
var count = 0,
    cache = [];

function factorial(n) {
    count++
    if (n == 0 || n == 1) {
        return 1
    }
    return n * factorial(n - 1)
}
//函数记忆  if n=5 count==>9
function factorial(n) {
    count++
    if (cache[n]) {
        return cache[n]
    } else {
        if (n == 0 || n == 1) {
            cache[0] = 1;
            cache[1] = 1;
            return 1
        } else {
            cache[n] = n * factorial(n - 1)
            return cache[n]
        }
    }
}
//记忆函数封装
function memory(fn) {
    var cache = {};
    return function() {
        var key = arguments.length + Array.portotype.join.call(arguments) //==>作为参数的唯一标识
        if (cache[key]) {
            return cache[key]
        } else {
            cache[key] = fn.apply(this, arguments)
            return cache[key]
        }
    }
}
```
8.函数柯里化
```
定义：柯里化通常也称部分求值，其含义是给函数分步传递参数，每次传递参数后,部分应用参数，并返回一个更具体的函数接受剩下的参数，中间可嵌套多层这样的接受部分参数函数，逐步缩小函数的适用范围，逐步求解,直至返回最后结果。
作用：柯里化(Currying)具有：延迟计算、参数复用、动态生成函数的作用。
//封装
function curring(fn) {
    var _args = [];
    return function cb() {
        if (arguments.length === 0) {
            return fn.apply(this, _args);
        }
        Array.prototype.push.apply(_args, [].slice.call(arguments));
        return cb;
    }
}
```
9.防抖 ==> 实时搜索 拖拽
```
function debounce(handler, delay) {
    var timer = null;
    return function() {
        var _self = this,
            _arg = arguments;
        clearTimeout(timer);
        timer = setTimeout(function() {
            handler.apply(_self, _arg)
        }, delay)
    }
}
```
10.节流 ==> 窗口调节 页面滚动 抢购疯狂点击
```
function throttle(handler, wait) {
    var lastTime = 0;
    return function() {
        var nowTime = new Date().getTime();
        if (nowTime - lastTime > wait) {
            handler.apply(this, arguments);
            lastTime = nowTime;
        }
    }
}
```
11.获取滚动条滚动距离
```
function getScrollOffset () {
    if(window.pageXOffset){
        return {
            x:window.pageXOffset,
            y:window.pageYOffset
        }
    }else{
        return {
            x:document.body.scrollLeft + document.documentElement.scrollLeft,
            y:document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```
12.获取可视区窗口的尺寸
```
function getViewportOffset () {
    if(window.innerWidth){
        return {
            w : window.innerWidth,
            h : window.innerHeight
        }
    }else{
        if(document.compatMode == 'BackCompat'){
            return {
                w : document.body.clientWidth,
                h : document.body.clientHeight
            }
        }else{
            return {
                w : document.documentElement.clientWidth,
                h : document.documentElement.clientHeight
            }
        }
    }
}
```
13.获取元素相对于文档的坐标位置
```
Element.prototype.getElementPosition = function () {
    var x = 0;
    var y = 0;
    var dom = this;
    for(dom; dom && dom.offsetParent;dom = dom.offsetParent){
        x += dom.offsetLeft;
        y += dom.offsetTop;
    }
    return {
        x: x,
        y: y
    }   
}
```
14.获取元素的样式
```
Element.prototype.getStyle = function(prop){
    var element = this
    if(window.getComputedStyle){
        return window.getComputedStyle(element,null)[prop]
    }else{
        return element.currentStyle[prop]
    }
}
```
15.封装addEvent解决兼容性问题
```
function addEvent (e,type,handle) {
    if(e.addEventListener){
        e.addEventListener(type,handle,false)
    }else if(e.attachEvent){
        e.attachEvent('on' + type,function(){
            handle.call(e)
        })
    }else{
        e['on' + type] = handle;
    }
}    
```
16.封装函数 阻止事件冒泡
```
function stopBubble (e) {
    if(e.stopPropagation){
        e.stopPropagation()
    }else{
        e.cancelBubble = true
    }
}
```
17.封装函数 阻止默认事件
```
function cancleHandler (e) {
    if(e.preventDefault){
        e.preventDefault()
    }else{
        e.returnValue = false;
    }
}
```
18.封装异步加载（按需加载）
```
function loadScript (src,callback) {
    var script = document.createElement('script');
    script.type = 'text/javascript';
    if(script.readyState){
        script.onreadystatechange = function () {
            if(script.readyState == 'complete' || script.readyState == 'loaded'){
                callback();
            }
        }
    }else{
        script.onload = function () {
            callback()
        }
    }
    script.src = src
    document.head.appendChild(script);
}
```
