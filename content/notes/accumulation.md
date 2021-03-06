Title: 日积月累
Date: 2015-04-13 00:19
Modified: 2015-04-13 00:19
Slug: accumulation
Authors: Joey Huang
Summary: 日积月累
Status: draft

[TOC]

## 20160406

### Pelican 代码阅读

* 命令行参数解析: python std lib -> argparse
* Log 系统: python std lib -> logging
* 导入 pelicanconf.py 中的设置信息
  使用 `imp.load_source()` 或 `SourceFileLoader().load_module()` 把 `pelicanconf.py` 作为模块导入，再使用 `inspect.getmembers()` 来获取模块里的变量，所有的全大写的变量作为设置信息读取进来。导入模块时，也可以使用 `__import__()` 函数，但参数的形式是不一样的。
* 给定一个字符串 `pelican.Pelican`，怎么样用这个字符串所代表的类创建对象？
  使用 `__import__` 和 `getattr()` 实现

```python
    cls = settings['PELICAN_CLASS']     # default class: 'pelican.Pelican'
    if isinstance(cls, six.string_types):
        module, cls_name = cls.rsplit('.', 1)
        module = __import__(module)
        cls = getattr(module, cls_name)

    return cls(settings), settings
```

* 如何把某个目录加入当前的模块搜索目录？
  把目录添加进 `sys.path` 列表即可
* 进程内信号/事件: 使用 [blinker](https://github.com/jek/blinker) 实现
* 插件系统
  * 使用进程内信号 `blinker` 来实现与插件的通信
  * 插件需要实现 `register()` 函数，这个函数由 Pelican 在初始化时调用
  * 插件需要注册 Pelican 系统的信号，根据不同的信号做相应的处理，具体参阅 `docs/plugins.rst` 以及 `pelican/signals.py`。
* 如何处理可选的依赖包？
  比如 Pelican 支持多种文档格式 markdown, rst, etc. 怎么样确保只使用 markdown 的人没安装 rst 转码库 `docutils` 也能正常运行呢？这里的方法是处理 `ImportError`，并且当发生 `ImportError` 时禁用这个转换器。
* TODO: Reader/Writer/Generator 源码阅读

## 20150413

#### 配置 Alfred 来搜索 StackOverflow 和 Github

* StackOverflow
  http://stackoverflow.com/search?q={query}
* Github
  https://github.com/search?q={query}

#### unsplash api

http://tumblr.unsplash.com/api/read?num=10

#### 壁纸应用，图片来自 unsplash

https://github.com/kamidox/wallsplash-android

## 20150416

#### wallsplash-android MainActivity.java

* Enum for Java
  definitin of MainActivity.Category
* materialdrawer
  https://github.com/mikepenz/MaterialDrawer
* OpenLibra-Material
  https://github.com/saulmm/OpenLibra-Material
* aboutlibraries
  https://github.com/mikepenz/AboutLibraries
* iconics
  https://github.com/mikepenz/Android-Iconics

#### wallsplash-android ImagesFragment.java

* RecyclerView -> fragment_images.xml
  A flexible view for providing a limited window into a large data set.
  REF: GridLayoutManager, RecyclerView.Adapter
* ActivityOptionsCompat
  Helper for accessing features in android.app.ActivityOptions introduced in API level 16 in a backwards compatible fashion.
* errorview
  https://github.com/xiprox/ErrorView
  A custom view that displays an error image, a title, and a subtitle given an HTTP status code.

#### wallsplash-android UnsplashApi.java

* RxAndroid
  https://github.com/ReactiveX/RxAndroid
  Android specific bindings for RxJava
* RxJava
  https://github.com/ReactiveX/RxJava
  RxJava – Reactive Extensions for the JVM – a library for composing asynchronous and event-based programs using observable sequences for the Java VM
* retrofit
  https://github.com/square/retrofit
  Type-safe REST client for Android and Java by Square, Inc.
* okhttp
  https://github.com/square/okhttp
  An HTTP+SPDY client for Android and Java applications.
* Gson
  https://github.com/google/gson
  Gson is a Java library that can be used to convert Java Objects into their JSON representation. It can also be used to convert a JSON string to an equivalent Java object. Gson can work with arbitrary Java objects including pre-existing objects that you do not have source-code of.
* API
  http://wallsplash.lanora.io/pictures
  http://wallsplash.lanora.io/random

## 20150423

#### 做成一件事情

* 选择一个方向：Android 进阶社区
* 持续输入价值：入门，进阶，疑难问题案例分析
* 持续运营：把文章/视频发到热闹的社区引流
* 想像空间：证明团队价值；与更大的世界发生交集；

**注意点**：专注；持续；快乐。忌朝三暮四，左顾右盼。

## 20150419

#### Android Performance Training

* udacity.com 上来自 google 官方的原始资料
  https://www.udacity.com/course/viewer#!/c-ud825/l-3729268966/m-3785788694
* ChinaGDG from youku.com
  整理出几个专题：主要是内存专题和电池专题。同归复习渲染和运算专题。
  http://www.youku.com/playlist_show/id_23494296.html
* 通过示例现场演示几个工具的使用
  Memory Monitor, Heap Viewer, Allocation Tracker
* 可参考的文章
  hukai.me
  http://hukai.me/android-performance-memory

## 20150607

####《持续的幸福》阅读笔记

* 幸福 1.0 vs 幸福 2.0
  幸福 1.0 主要关注生活满意度，由积极情绪，投入和意义构成。而幸福 2.0 则由 PERMA 指标构成，具体就是积极情绪，投入，意义，成就，积极的人际关系。Seligman 教授认为，要让人生蓬勃发展，可以提高这五项指标来获得。
* 三件好事练习
  从进化的角度来看，人是悲观偏好的，那些过分乐观的人活不过冰河世纪。如何克服悲观偏好，可以从三个好事练习着手。即每天记录三个好事，可以是大事（如获得升职），也可以是生活的小事（如发现一个好吃的饭馆）。然后写上这个事对你的影响或产生的原因。这样来引导自己多关注积极的事情。
* 突出优势练习
  通过 www.authentichappiness.org 来测量自己的性格优势，通过发挥自己的性格优势来增加幸福感。
* 积极主动式回应 vs 消极主动式回应
  女儿告诉我她在学校运动会上获得了一个奖牌。
  积极主动：太棒了，你拿到奖牌时是什么感受？你的同学是什么反应？这个运动挺难的，平时没见你练啊，你怎么拿到奖牌的？
  积极被动：哦，真厉害。
  消极被动：我今天上班很累。
  消极主动：不好好读书，拿个运动会奖牌干什么用，把时间花在学习上。
* 洛萨达比例
  职场洛萨达比例在 3:1 以上时，公司的业务是蓬勃发展的。要获得良好的家庭生活，洛萨达比例需要达到 5:1。
  洛萨达比例 (以其发现者 Marcel Losada 命名) 是指对所有的语言按照积极和消极进行编码，积极和消极的比例即洛萨达比例。
* 成就公式
  成就 = 技能 x 努力。成就是一个矢量，而不是绝对距离。成就是朝一个特定的方向持续努力的结果。
  **速度**: 自动化的东西越多，速度越快，我们对该任务的知识就越多。
  **缓慢**: 成就中举足轻重，有意识的过程（如规划，精细化，检查错误和创造）。速度越快，知识越多，留给这些执行功能的时间就越多。
  **学习速度**: 新的信息能以多快的速度变为自动知识，以留给缓慢的执行过程更多的时间。
* 学习的速度
  技能的学习是让我们获得一种自动化处理的能力。比如打字，刚开始的时候需要先想我们打什么字，这个字由哪几个字根组成，这些字根在哪个按键上，然后手指再按下这些按键。这是技能，当这项技能经过足够多的练习，变成一个自动化的过程后，打字就快很多了。因为头脑在想打哪个字的时候，手指已经直接按下能打出这个字所在的键盘了。这就是自动化处理能力。自动化处理能力可以大大提高效率。这也是一万小时天才理论的基础。学习的快，是指你能以多快的速度培养一项技能的自动化处理能力。
* 创伤后成长的五要素
  1. 认识到创伤后信念崩塌是正常的反应；
  2. 减少焦虑和强迫性的想法；
  3. 讲出创作经历；
  4. 描述创伤后积极的改变；
  5. 总结因为创伤而产生的更加坚强，更加无惧挑战的人生原则和立场。
  **那些杀不死我的，必将使我强大。**---尼采
* ABCDE 模式
  情感的后果不是由不好的事情导致的，而是由你对这个事情的解读导致的。
  Adversity -> Belief -> Consequence。Disputation 代表反驳，Energization 代表你成功进行反驳后受到的启发。
* 习得性无助
  经历过束手无策的逆境，再遇到逆境时，就会变得消极被动，容易放弃。
  **实验第一部份**
  掌握组：老鼠经历 64 次可避免的电击。
  无助组：老鼠经历 64 次无法避免电击。
  对照组：没有受到电击。
  **实验第二部分**：注射有 50% 概率到死的癌细胞
  掌握组：25% 死亡率
  无助组：75% 死亡率
  对照组：50% 死亡率

## 20160614

Node.js Design Patterns: Chapeter 1 Node.js Design Fundamentals

异步和回调；模块系统；观察者模式等等

### The Node.js philosophy

* Small Core: 微内核
* Small Module: 小模块。解决了 dependency hell 问题，每个模块可以有自己独立的依赖模块列表。即一个软件可以依赖同一个模块的不同版本。
  * Small is beautiful.
  * Make each program do one thing well.
  * Easier to understand and use
  * Simpler to test and maintain
  * Perfect to share with the browser
* Small surface area: 小接口。暴露出最小的，最重要的接口。次要的放在模块的属性，方法里。
  * 暴露尽量少的接口 -> 易于使用
  * 模块设计的目的是为了被使用，而不是被扩展
* Simplicity and pragmatism: 简单实用原则


## 20160615

最高效的深度拷贝库: https://github.com/ivolovikov/fastest-clone

* javascript 元编程：利用 `new Function('params', 'function body')` 或 `eval('code')` 来进行实现函数
* javascript 元编程：利用 `Function.prototype.call()` 或 `Function.prototype.apply()` 来对函数进行调用
* 判断是否在 node.js 环境： `if (typeof module == 'object' && typeof module.exports == 'object')`
* 递归获取 Object 实例的所有属性以及属性的属性：`_getKeyMap: function (source, deep, baseKey, arrIndex)`

Node.js Design Patterns: Chapeter 1 Node.js Design Fundamentals

### The reactor pattern

* I/O is slow: 与内存访问速度在 GB/s 数量级；磁盘的访问速度在 MB/s 数量级。
* Blocking I/O:　阻塞式访问 I/O ，需要用多线程机制来处理并发。需要增加线程锁和线程数据同步，增加了复杂性。
* Non-blocking I/O: 使用非阻塞式 I/O 的一个方案是忙等待 (Busy Wait)，即不停地调用 I/O 函数，直到读出数据或出错为止。这种方案较浪费 CPU 。
* Event demultiplexing: 多路事件分解器来提高使用非阻塞 I/O 的效率。把所有待监控的资源都放在一个数组里，交给事件分解器来监视，当资源上有事件发生（比如有数据可供读取）时，监视动作返回，再去读取数据。当没有事件发生时，监控函数会进入睡眠状态，把 CPU 让给别的程序。这一编程模式类似用 `select` 函数实现异步 Socket 编程。
* The reactor pattern: 反应堆模式。比 Event demultiplexing 更进一步。直接给资源的操作提供一个 Handler 作为回调函数。当资源可用时，直接调用回调函数。实现完整的异步 I/O 编程模式。
* libuv: The non-blocking I/O engine of Node.js。不同的操作系统有各种的 Event demultiplexing 机制，比如 Linux 下的 `epoll`，Mac OS X 下的 `kqueue`，Windows 下的 I/O Completion Port API (IOCP)。不同平台的事件分发机制不同，且同一个平台下不同的资源的异步模式也不同。比如 Unix 下 socket 支持异步 I/O，而普通的文件系统 API 则不支持异步 I/O，这个时候就需要用单独的线程来模拟异步文件读写操作。libuv 库就是为了解决这些问题而产生的。它向 javascript 提供了不同操作系统，不同资源的异步 I/O 的抽像封装和实现。
* The recipe for Node.js: Node.js 包含以下几部分
  * 操作系统底层接口的封装，并暴露给 javascript 调用，如 libuv 等
  * V8, 这是 Google 给 Chrome 开发的高性能的 javascript 引擎
  * node.js 核心库 (node-core)，实现 Node.js 的高层 API


## 20160616

Node.js Design Patterns: Chapeter 1 Node.js Design Fundamentals

### The callback pattern

* The continuation-passing style: CPS 把函数作为参数传递，完成回调
  * Synchronous continuation-passing style：同步回调，即函数返回后，回调也己调用了。
  * Asynchronous continuation-passing style：异步回调，即函数返回后，回调还没被调用，回调会被推送到 event queue 里，在下一轮的 event loop 里调用回调。比如使用 `setTimeout()`， `process.nextTick()` 来实现。
  * Non continuation-passing style callbacks：非 CPS 方式的回调。比如 `Array.map()` 的回调函数，通过回调函数的返回值直接交互。
* Synchronous or asynchronous?：同步还是异步？
  * An unpredictable function：同步和异步不可预见性，即[有时是同步回调，有时是异步回调](#an_unpredictable_function)。这种问题会引入非常难查的 bug 。
  * Using synchronous APIs：改装成同步 API，比如通过 `fs.readFileSync()` 函数来实现。不推荐使用这种方式，会破坏 Node.js 的异步 I/O 模式。一个判断标准是：Use blocking API only when they don't affect the ability of the application to serve concurrent requests.
  * Deferred execution：延后执行，把 API 改装成异步回调。通过 `process.nextTick()` 实现。需要注意在 event loop 里的优先级。
* Node.js callback conventions: Node.js 的回调惯例
  * Callbacks come last: 把回调放在最后一个参数，这样在使用的时候，可以直接用匿名函数或箭头函数实现。如 `fs.readFile(filename, [options], callback)` 。
  * Error comes first: 把错误放在第一个参数。在 Node.js 里，CPS 风格的回调的第一个参数通常是错误信息，数据结果在第二个及之后的参数提供。如果没有出错，第一个参数的值为 `null` 或 `undefined` 。如 `fs.readFile('foo.txt', 'utf8', function(err, data) {}` 。
  * Propagating errors: 错误传递机制。同步调用时，错误传递机制通过 `throw` 来抛出异常，这样可以使错误在调用栈里往上跳，直到它被处理为止。而在异常调用里，异常处理的机制是，在 CPS 回调链里，通过回调函数一层层往上传递。可参阅[示例代码](#propagating_errors)。
  * Uncaught exceptions: 未处理的异常可以通过 `process.on('uncaughtException', function(err){}` 来捕获到。系统默认是直接退出程序。一般的处理是在这里记录异步 Log ，然后退出程序。针对网络服务而言，退出程序重启总是保证持续服务可用的一个折衷策略。

<a name="an_unpredictable_function"></a>**同步或异步不可预测的函数**

```javascript
var fs = require('fs');
var cache = {};
function inconsistentRead(filename, callback) {
  if(cache[filename]) {
    //invoked synchronously
    callback(cache[filename]);
  } else {
    //asynchronous function
    fs.readFile(filename, 'utf8', function(err, data) {
      cache[filename] = data;
      callback(data);
    });
  }
}
```

<a name="propagating_errors"></a>**异步回调的异常处理**

```javascript
var fs = require('fs');
function readJSON(filename, callback) {
  fs.readFile(filename, 'utf8', function(err, data) {
    var parsed;
    if(err)
      //propagate the error and exit the current function
      return callback(err);
    try {
      //parse the file contents
      parsed = JSON.parse(data);
    } catch(err) {
      //catch parsing errors
      return callback(err);
    }
    //no errors, propagate just the data
    callback(null, parsed);
  });
};
```

## 20160620

Node.js Design Patterns: Chapeter 1 Node.js Design Fundamentals

### The module system and its patterns

模块系统是 Node.js 的代码结构化的基础，实现信息隐藏和接口实现的功能。

* The revealing module pattern: 模块系统的本质是利用函数来创建具有信息隐藏功能的代码块。函数内的嵌套函数及变量对模块外部不可见，只有函数返回的属性和方法才对外可见。
* Node.js modules explained: 官方文档 [Node.js 模块系统](https://nodejs.org/api/modules.html) 是最权威且最清晰的资料。
* module.exports vs exports: The variable exports is just a reference to the initial value of module. exports。
* require is synchronous
* The resolving algorithm
  * File modules: 以 `/` 或 `./` 或 `../` 开头的参数，解释为文件模块
  * Core modules: 如果没有以路径开头，则解释为 Node.js 核心内置模块，如 `var fs = require(fs);` 。
  * Package modules: 如果没有核心模块与之匹配，则查找当前目录下的 `node_modules` 目录下查找匹配的模块，如果找不到，则查找从父目录的 `node_modules` 目录下查找，直至根目录下的 `node_modules` 。
  * 文件模块/包模块匹配策略
    * `moduleName.js`
    * `moduleName/index.js`
    * The `main` property of `moduleName/package.json`
* Solution for **Dependency Hell**: 按照上述模块搜索算法，每个模块都可以通过自己的 `node_modules` 子目录指定其依赖的子模块。这样即使同一个应用程序里不同模块引用了相同的子模块，他们各自独立，可以是不同的版本。
* The module cache: 模块缓存可以解决几个问题
  * 循环引用问题
  * 确保引用的一致性
  * 加快效率
* Cycles: 模块循环引用问题，A require B, B require A
* Module de nition patterns
  * Named exports: `exports.info = function(message) { ... }`
  * Exporting a function: `module.exports = function(message) { ... }`
  * Exporting a constructor: [示例代码](#exporting_a_constructor)
  * Exporting an instance: [示例代码](#exporting_an_instance)。巧妙地利用模块的缓存功能，使每个引用此模块的模块都引用了同一个实例。这样就实现了单例 (Singleton) 模式。
* Modifying other modules or the global scope: 不是好的实践，但在自动测试领域有其应用场景，我们称之为猴子补丁 (Monkey Patching) 。[示例代码](#monkey_patching) 。

<a name="exporting_a_constructor"></a>**模块返回构造函数**

```javascript
//file logger.js
function Logger(name) {
 this.name = name;
};
Logger.prototype.log = function(message) {
 console.log('[' + this.name + '] ' + message);
};
Logger.prototype.info = function(message) {
 this.log('info: ' + message);
};
Logger.prototype.verbose = function(message) {
 this.log('verbose: ' + message);
};
module.exports = Logger;
```

<a name="exporting_an_instance"></a>**模块返回一个实例/单例**

```javascript
// file logger.js
function Logger(name) {
  this.count = 0;
  this.name = name;
};

Logger.prototype.log = function(message) {
  this.count++;
  console.log('[' + this.name + '] ' + message);
};
module.exports = new Logger('DEFAULT');
```

<a name="monkey_patching"></a>**猴子补丁**

```javascript
// file patcher.js
// ./logger is another module
require('./logger').customMessage = function() {
  console.log('This is a new functionality');
};

// Using our new patcher module would be as easy as writing the following code:
// file main.js
require('./patcher');
var logger = require('./logger');
logger.customMessage();
```

## 20160621

### rethinkdb

提供在线数据库，提供数据的增删改查。一大亮眼特性是 changefeed。它能够把数据库中某个查询结果集的改变 publish 出来，供其他人 subscribe。这个特性对 realtime collaboration 的 app 来说非常有用。可以实现数据实时同步。

什么样的场景适合使用 rethinkdb ?

* Collaborative web and mobile apps
* Streaming analytics apps
* Multiplayer games
* Realtime marketplaces
* Connected devices

### leancloud

提供在线非结构化数据库，提供数据的增删改查。辅助类，提供实时聊天及推送，流量分析等功能。

### wilddog

国内的 BaaS (Backend as a Service) 平台。提供两大功能：

* 实时同步: 提供毫秒级实时数据同步。使用 TLS + websocket 保障通信安全。不支持 websocket 的环境使用 long-polling 模拟长连接。保障通信实时性。即一个数据修改后，另外一个订阅者可以马上得到同步。
* 在线 Json 数据库：提供数据的增删改查功能。

### 带网关的 IoT 系统通信需求

* 安全性：通信安全 (TLS) 及访问授权 (Auth)
* 实时性：控制命令和状态能及时送达，达到毫秒级实时性
* 双向通信：不同于 request/response 响应模型。App 与 Gateway，App 与 Server 之间必须支持双向实时通信。
* 一致性：App 与 Gateway 之间；App 与 Server 之间需要实现一致的通信协议和通信模型，减少系统复杂度和开发工作量。

### websocket & long-polling

* [RFC 6455 - The WebSocket Protocol](http://tools.ietf.org/html/rfc6455)
* [WebSocket API Specification](http://www.w3.org/TR/websockets/)
* [socket.io](http://socket.io): A powerful cross-platform WebSocket API for Node.js

### XaaS

> 传统云服务公司的定义：SaaS、PaaS、IaaS。越往下自由度越高，越往上使用起来越简单。
> SaaS解决的是开箱即用的问题，不用写代码，直接用。PaaS解决的是运维的问题，写完代码往云端一扔，搞定。而IaaS解决的是硬件资源弹性扩容的问题，像个水龙头，用多少拧多少。
> 目前PaaS代表的产品比如HeroKu，Google App Engine、国内SAE等，几乎全线已挂或半死不活。PaaS挂掉的原因是没有解决根本问题，半吊子。又不简单，又不自由。
> 广义BaaS是指用户需要通过远程API获得服务的云服务产品。比如类似统计服务MixPanel、友盟等。狭义的BaaS是指通过远程API提供计算和存储资源的产品，比如Parse、Firebase、Twilio、Pusher，Apple Cloud Kit这样的产品。

REF: http://www.leiphone.com/news/201605/UQ4LxnsXfxqv2r39.html

## 20160622

Node.js Design Patterns: Chapeter 1 Node.js Design Fundamentals

### The observer pattern

The observer pattern is already built into the Node.js core and is available through the `EventEmitter` class.

* EventEmitter 类的用法，可以手动创建一个 `EventEmitter` 实例来使用。
* 错误处理：不能直接抛出异常，因为事件回调一般在单独的消息循环里处理，抛出的异常会丢失。一个通用的做法是定义一个独立的 `error` 事件，然后 `emmit` 这个事件。
* Make any object observable：通过继承 `EventMitter` 来实现。ES5 可以通过 `util.inherits()` 实现，ES6 可以直接用 `inherit` 关键字实现。
* Synchronous and asynchronous events: 同步事件和异常事件
* EventEmitter vs Callbacks: 应该用哪个呢？
  * semantic: callbacks should be used when a result must be returned in an asynchronous way; events should instead be used when there is a need to communicate that something has just happened.
  * 如果一个事件可能发生多次，或者可能根本不会发生，使用 EventEmitter 是较好的选择
  * 使用 EventEmitter 可以让多个监听者同时监听到事件。而 callback 是一对一的结果返回。
* Combine callbacks and EventEmitter: 结合两者的优势。

```javascript
var glob = require('glob');
glob('data/*.txt', function(error, files) {
  console.log('All files found: ' + JSON.stringify(files));
}).on('match', function(match) {
  console.log('Match found: ' + match);
});
```

关于 `EventEmitter` 可参阅[官方资料](https://nodejs.org/dist/latest-v6.x/docs/api/events.html) 。

## 20160624

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### The difficulties of asynchronous programming

**The callback hell**: 使用 `request` 和 `mkdirp` 实现的一个简单的爬虫程序，可以明显地看到异步流程控制代码很容易陷入 callback hell 的陷阱。如[示例程序](#callback_hell)。callback hell 的代码有如下问题：

* 可读性差：很难界定回调函数的起始位置和结束位置
* 变量名重叠：比如回调函数里的错误码 `err` 在每个回调函数里都有，容易引起误解
* 闭包函数会引起少量的内存和性能问题，比如内存泄露

<a name="callback_hell"></a>**爬虫程序：callback hell**

```javascript
function spider(url, callback) {
    var filename = utilities.urlToFilename(url);
    fs.exists(filename, function(exists) {                              //[1]
        if(!exists) {
            console.log("Downloading " + url);
            request(url, function(err, response, body) {                //[2]
                if(err) {
                    callback(err);
                } else {
                    mkdirp(path.dirname(filename), function(err) {      //[3]
                        if(err) {
                            callback(err);
                        } else {
                            fs.writeFile(filename, body, function(err) { //[4]
                                if(err) {
                                    callback(err);
                                } else {
                                    callback(null, filename, true);
                                } });
                        } });
                } });
        } else {
            callback(null, filename, false);
        } });
}
```

### Using plain JavaScript

使用 JavaScript 的一些通用规则可以避免 callback hell 问题。

**Callback discipline**: 编写回调函数的一些原则

* You must exit as soon as possible. 尽早返回。即先处理错误。
* You need to create named functions for callbacks. 给回调创建命名函数。
* You need to modularize the code. Split the code into smaller, reusable functions whenever it's possible.

下面是按照编写回调函数的原则执行后的改进版本：

```javascript
function saveFile(filename, contents, callback) {
    mkdirp(path.dirname(filename), function(err) {
        if(err) {
            return callback(err);
        }
        fs.writeFile(filename, contents, callback);
    });
}

function download(url, filename, callback) {
    console.log('Downloading ' + url);
    request(url, function(err, response, body) {
        if(err) {
            return callback(err);
        }
        saveFile(filename, body, function(err) {
            console.log('Downloaded and saved: ' + url);
            if(err) {
                return callback(err);
            }
            callback(null, body);
        });
    });
}

function spider(url, callback) {
    var filename = utilities.urlToFilename(url);
    fs.exists(filename, function(exists) {
        if(exists) {
            return callback(null, filename, false);
        }
        download(url, filename, function(err) {
            if(err) {
                return callback(err);
            }
            callback(null, filename, true);
        })
    });
}
```

**Sequential execution**: 顺序执行

爬虫程序就是一个典型的顺序执行的程序。文件是否存在 -> 从网络下载 -> 新建文件夹 -> 写文件。对己知的顺序执行的异步任务，可以使用下面的代码模板：

```javascript
function task1(callback) {
    asyncOperation(function() {
        Start Task 1 Task 2 Task 3 End
        task2(callback);
    });
}

function task2(callback) {
    asyncOperation(function(result) {
        task3(callback);
    });
}

function task3(callback) {
    asyncOperation(function() {
        callback();
    });
}

task1(function() {
    //task1, task2, task3 completed
});
```

**Sequential iteration**: 异步遍历序列数据

* `task()` 函数最好是异步函数，如果是同步函数，可能造成深度递归，从而使栈溢出

```javascript
function iterate(index) {
    if(index === tasks.length)  {
        return finish();
    }
    var task = tasks[index];
    task(function() {
        iterate(index + 1);
    });
}

function finish() {
    //iteration completed
}

iterate(0);   // start iterate sequence asynchronize
```

**思考**

* 使用异步遍历序列数据的方法，实现爬虫的另外一个版本：递归下载网页和网页里的所有链接。注意，只下载相同域名下的链接。
* 更一般化地抽你异步遍历模型，可以实现如下函数签名的异步遍历函数 `iterateSeries(collection, iteratorCallback, finalCallback)` 。

## 20160627

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### Parallel execution

如果多个任务没有先后顺序上的依赖，那么可以使用并行执行的模型来实现。当任务全部完成后，通过回调函数通知调用者。

```javascript
var tasks = [...];
var completed = 0;

tasks.forEach(function(task) {
    task(function() {
        if(++completed === tasks.length) {
            finish(); }
    });
});

function finish() {
    //all the tasks completed
}
```

## 20160630

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### Limiting the concurrency

带上限的并发执行可以有效地控制资源消耗，避免资源过载。

```javascript
var tasks = [...];
var concurrency = 2, running = 0, completed = 0, index = 0;

function next() {              //[1]
    while(running < concurrency && index < tasks.length) {
        task = tasks[index++];
        task(function() {            //[2]
            if(completed === tasks.length) {
                return finish();
            }
            completed++, running--;
            next();
        });
        running++;
    }
}

next();

function finish() {
    //all tasks finished
}
```

### Globally limiting the concurrency

上面的模式无法实现全局限制，如果一个任务产生两个并发任务，多个任务就会产生多个并发任务。我们需要实现全局并发数量限制，可以用一个队列来实现。

<a name="globally_limiting_the_concurrency"></a>**全局限制并发数量**

```javascript
function TaskQueue (concurrency) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
}

TaskQueue.prototype.pushTask = function (task, callback) {
    this.queue.push([task, callback]);
    this.nextTask();
};

TaskQueue.prototype.nextTask = function () {

    function makeCallback(self, task, callback) {
        return function (err) {
            callback(err, task);
            self.running --;
            self.nextTask();
        }
    }

    while (this.running < this.concurrency && this.queue.length > 0) {
        var [task, callback] = this.queue.shift();
        task(makeCallback(this, task, callback));
        this.running ++;
    }
};

module.exports = TaskQueue;
```

具体 `TaskQueue` 代码可[参阅 GitHub 上的源码](https://github.com/kamidox/exercism/tree/master/javascript/task-queue)。

## 20160704

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### The async library

[async](https://npmjs.org/package/async) 已经成为 JavaScriopt 异步流程控制的事实标准。它最初设计目标是运行在 Node.js 环境，目前也可以在浏览器环境执行。

**Sequential execution**

async 库有 20 多个函数用来执行串行任务。[`series()`](http://caolan.github.io/async/docs.html#.series) 函数确保异步函数顺序执行；[`waterfall()` ](http://caolan.github.io/async/docs.html#.waterfall) 在确保异步函数顺序执行的同时，还会以上一个函数的输出作为下一个函数的输入。

**Parallel execution**

async 库有大量的并行异步任务控制器。比如 `map()` 从一个列表逐个元素映射，生成一个新列表，在列表元素映射的过程中，是并行发生的，所以没有顺序保证。`parallel()` 只是单纯地确保任务并行执行。

**Limited parallel execution**

async 也可以限制并发个数的并行执行。典型地，`parallel()` 有个对应的 `parallelLimit()` 函数，作为并发个数限制的版本。`queue()` 作为任务队列，和书中介绍过的 `TaskQueue` 完成类似的功能。

### Promises

#### What is a promise?

Promises 是一种抽像，它允许异步函数返回一个 Promises 对象，用这个对象来表达异步函数的最终结果。用 Promises 术语来讲，当异步函数未完成时，这个 Promises 对象处于 **未决 (pending)** 状态。当异步函数成功完成时，这个 Promises 对象处于 **达成 (fulfilled)** 状态。当异步任务发生错误时，这个 Promises 对象处于 **驳回 (rejected)** 状态。只要异步函数完成，不管是成功还是失败，我们称这个 Promises 对象为 **确定 (settled)** 状态，这个状态和未决状态相对应。

In very simple terms, promises are an abstraction that allow an asynchronous function to return an object called a promise, which represents the eventual result of the operation. In the promises jargon, we say that a promise is pending when the asynchronous operation is not yet complete, it's fulfilled when the operation successfully completes, and rejected when the operation terminates with an error. Once a promise is either fulfilled or rejected, it's considered settled.

Promises 对象有个重要的方法 `then()` 可以用来处理异步代码。

```javascript
promise.then([onFulfilled], [onRejected])
```

`onFulfilled` 是一个函数，用来处理达成状态的 Promises。`onRejected` 用来处理驳回状态的 Promises。

利用这一特性，可以把 CPS 回调转换为 Promises 异步处理方式：

```javascript
asyncOperation(arg, function(err, result) {
    if(err) {
        //handle error
    }
    //do stuff with result
});

asyncOperation(arg)
    .then(function(result) {
        //do stuff with result
    }, function(err) {
        //handle error
    });
```

用 Promises 写的代码更简洁，结构化现强。但这不是 Promises 的主要功能。**Promises 的神奇功能在于 `then` 函数本身返回一个 Promises 对象**。 `then` 函数的函数 `onFullfilled` 和 `onRejected` 什么时候被调用？在异步函数处于 settled 状态时被调用。如果 `onFulfilled` 或 `onRejected` 函数返回一个对象 `x`，那么 `then` 函数返回的 Promises 对象具有以下特性：

* 如果 `x` 是一个值，那么 `then` 返回的 Promises 对象处于**达成**状态，且其值为 `x`
* 如果 `x` 是一个 Promises 对象，那么 `then` 返回的 Promises 对象将会**达成** `x` 所达成的状态
* 如果 `x` 是一个 Promises 对象，那么 `then` 返回的 Promises 对象将会**驳回** `x` 所最终驳回的状态

这一特性的重要性在于我们可以写出链式的 Promises 表达式。如果我们不指定 `onFulfilled` 或 `onRejected` ，那么 Promises 对象会自动把确定的状态 (达成或驳回) 传递给下一个 Promises 对象。这样我们可以非常方便地把错误处理传递下去，直到最后一环的 `onRejected` 函数中处理即可。

使用这个编程模型，顺序执行的异步函数可以写得非常简洁：

```javascript
asyncOperation(arg)
    .then(function(result1) {
        //returns another promise
        return asyncOperation(arg2);
    })
    .then(function(result2) {
        //returns a value
        return 'done';
    })
    .then(undefined, function(err) {
        //any error in the chain is caught here
    });
```

关于链式 Promises 的工作流程，可以参阅下图：

![Promises Chain](../../images/accumulate_promises_chain.png)

另外一个特性是，Promises 对象的 `then` 函数总是确保 `onFulfilled` 和 `onRejected` 被异步调用。即使一个 Promises 对象是由一个同步函数返回的，也是如此。这样就确保了调用的一致性，避免出现[同步异步不可预测性的问题](#an_unpredictable_function)。这种问题最典型的情况是，不知道函数的调用顺序。

#### Promises/A+ implementations

[Promises/A+](https://promisesaplus.com) 是 ES6 Promises 采纳的实现标准。除此之外，还有一系列第三方库实现了 Promises/A+ 标准：

* Bluebird (https://npmjs.org/package/bluebird)
* Q (https://npmjs.org/package/q)
* RSVP (https://npmjs.org/package/rsvp)
* Vow (https://npmjs.org/package/vow)
* When.js (https://npmjs.org/package/when)
* ES6 Promises

这些库的区别在于在 Promises/A+ 标准之外的功能上。Promises/A+ 实际上只规范了 `then` 函数的行为以及 Promises 对象从 pending 状态到 settled 状态的过程。对其他功能并没有规定，比如怎么样创建一个 Promises 对象，即构造函数的函数签名是没有规定的。

关于 ES6 Promises 可以参阅 [ES6-cheatsheet](https://github.com/DrkSephy/es6-cheatsheet#promises)。

## 20160705

### 选股辅助系统

做一个产品，采集所有股票的历史数据，可以对选股进行辅助分析。用户输入一个股票后，给出买入或卖出的建议。以及股票适合长线还是短线的策略建议。

* 股价全部历史走势图
* 股价在其历史股价的水平：当前价格与历史最高价的百分比
* 股价以月为周期的波动情况：波动幅度越大，投机价值越大
* 股价以年为周期的波动情况
* 每个季度净利润走势图
* 每个季度净利润排名走势图
* 每个季度营收数据走势图
* 每个季度营收数据排名走势图
* 每个季度每股收益走势图
* 每个季度每股收益排名走势图
* 每个季度每股营收走势图
* 每个季度每股营收排名走势图
* 历史分红配股情况：分列出每一年的分红配股情况
* 历史分红配股情况在所有股票中的排名：需要有个算法，算出每支股票分红配股分数
* 股价与 深证指数/上证指数 的吻合情况及其排名 （排名：需要算出每支股票与上证指数/深证指数的相关系数）

## 20160706

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### Promises

#### Promisifying a Node.js style function

Node.js 库里只有少数几个原生支持 Promises ，我们可以通过转换，把 Node.js 类型的 CPS 回调函数改装成 Promises 样式的函数。

```js
var Promise = require('bluebird');

module.exports.promisify = function (callbackBasedApi) {
    return function promisified() {
        // copy array: copy arguments of function promisified()
        var args = [].slice.call(arguments);
        return new Promise(function (resolve, reject) {
            args.push(function (err, result) {
                if (err) {
                    return reject(err);
                }
                if (arguments.length <= 2) {
                    resolve(result);
                } else {
                    resolve([].slice.call(arguments, 1));
                }
            });
            callbackBasedApi.apply(null, args);
        });
    }
};
```

大多数 Promises 实现都提供了把传统的 CPS 回调的 API 转换为 Promises 样式的工具函数，比如 Q 的 `Q.denodeify()`, `Q.nbind()`，bluebird 的 `Promises.promisify()` 等。

#### Sequential execution

Promises 对己知函数的顺序执行的模式代码如下：

```js
function promisesFunc() {
    return func1(params1)
        .then(=> (result1) {
            return func2(result1[0]);
        })
        .then(=> (result2) {
            return func3(result2);
        })
        .then(=> (result3)) {
            return func4(result3);
        };
}
```

对未知列表进行迭代顺序执行时，其模式代码如下：

```js
var tasks = [...];
var promise = Promise.resolve();    // create a empty Promise Object which resolved as 'undefined'
tasks.forEach(function (task) {     // chain each task with Promises Object
    promise = promise.then(function () {
        return task();
    });
});
```

#### Parallel execution

使用 Promises 也可轻松执行并行任务：

```js
function spiderLinks(currentUrl, body, nesting) {
    if (nesting === 0) {
        return Promise.resolve();
    }
    var links = utilities.getPageLinks(currentUrl, body);
    var promises = links.map(function (link) {
        return spider(link, nesting - 1);
    });
    return Promise.all(promises);
}
```

#### Limited parallel execution

ES6 Promises 并没有提供原生的机制来实现并行任务的控制。一个办法是使用 Javascript 原生方法来实现并行执行数量限制，比如[使用 `TaskQueue` 来实现](#globally_limiting_the_concurrency)。

#### 示例代码

关于使用 ES6 Promise 实现的网络爬虫己发布到 github 上。

* [spider](https://github.com/kamidox/exercism/tree/master/javascript/spider)
  使用串行 Promise 模型，递归下载一个网站下的所有资源，包括 html, css, javascript 等。
* [spider v2](https://github.com/kamidox/exercism/tree/master/javascript/spider-v2)
  使用全局并发限制的 Promise 模型，递归下载一个网站下的所有资源，包括 html, css, javascript 等。

## 20160713

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### Generator

#### Basic

生成器是 ES6 引入的一个新特性，和其他语言的生成器类似：

```js
function *infiniteNumbers() {
    var n = 1;
    while (true) {
        yield n++;
    }
}

var numbers = infiniteNumbers(); // returns an iterable object

numbers.next(); // { value: 1, done: false }
numbers.next(); // { value: 2, done: false }
numbers.next(); // { value: 3, done: false }
```

#### Generator as iterator

生成器可以作为替代器使用：

```js
function* iteratorGenerator(arr) {
    for(var i = 0; i < arr.length; i++) {
        yield arr[i];
    };
}
var iterator = iteratorGenerator(['apple', 'orange', 'watermelon']);
var currentItem = iterator.next();
while(!currentItem.done) {
    console.log(currentItem.value);
    currentItem = iterator.next();
}
```

#### Passing values back to a generator

通过生成器的 `next()` 函数可以向生成器传递参数：

```js
function* twoWayGenerator() {
    var what = yield null;
    console.log('Hello ' + what);
}
var twoWay = twoWayGenerator();
twoWay.next();
twoWay.next('world');
```

甚至可以向生成器传递一个异常：

```js
var twoWay = twoWayGenerator();
twoWay.next();
twoWay.throw(new Error());
```

这个代码会使生成器函数在 `yield` 返回的地方抛出异常。

#### Asynchronous control flow with generators

生成器可以用来作为异步流程控制工具。用来解决 callback hell 问题。

```js
function asyncFlow(generatorFunction) {

    function callback(err) {
        if(err) {
            return generator.throw(err);
        }
        var results = [].slice.call(arguments, 1);
        generator.next(results.length > 1 ? results : results[0]);
    };

    var generator = generatorFunction(callback);
    generator.next();
}
```

`asyncFlow()` 函数接受一个生成器函数作为参数。作为参数的生成器函数有个特点，即接受一个 `callback` 作为参数。有了 `asyncFlow()` 我们可以实现一个简单的文件复制的函数：

```js
var fs = require('fs');
var path = require('path');

asyncFlow(function* (callback) {
    var fileName = path.basename(__filename);
    var myself = yield fs.readFile(fileName, 'utf8', callback);
    yield fs.writeFile('clone_of_' + fileName, myself, callback);
    console.log('Clone created');
});
```

这样，通过 `asyncFlow()` 函数把异步的流程变成类似**同步**函数的流程。注意到函数中的 `callback` 参数实在有点多余，可以进一步变形如下：

```js
function readFileThunk(filename, options) {
    return function(callback) {
        fs.readFile(filename, options, callback);
    }
}

function writeFileThunk(filename, options) {
    return function(callback) {
        fs.writeFile(filename, options, callback);
    }
}

function asyncFlowWithThunks(generatorFunction) {
    function callback(err) {
        if(err) {
            return generator.throw(err);
        }
        var results = [].slice.call(arguments, 1);
        var thunk = generator.next(results.length > 1 ? results : results[0]).value;
        thunk && thunk(callback);   // 注意：这里的 thunk 是生成器的返回值，它是一个函数
    };

    var generator = generatorFunction();
    var thunk = generator.next().value;
    thunk && thunk(callback);   // 注意：这里的 thunk 是生成器的返回值，它是一个函数
}
```

经过变形以后，我们使用 Generator 来实现异步流程控制的时候，就不需要看到 `callback` 函数了，显得更简洁：

```js
asyncFlowWithThunks(function* () {
    var myself = yield readFileThunk(__filename, 'utf8');
    yield writeFileThunk("clone of clone.js", myself);
    console.log("Clone created");
});
```

关于如何使用 Generator 进行异步流程控制，可参阅 Github 上的 [async-flow](https://github.com/kamidox/exercism/tree/master/javascript/async-flow) 示例代码。

#### Generator-based control flow using co

 一些第三方库利用上面介绍的 `asyncFlow()` 的原理实现了异步控制，其中最早的是 [`suspend`](https://npmjs.org/package/suspend)，另外一个是 [`co`](https://npmjs.org/package/co)。`co` 有自己的生态系统：

* Web frameworks, the most popular being [koa](https://npmjs.org/package/koa)
* Libraries implementing specific control flow patterns
* Libraries wrapping popular APIs to support co

另外一个值得介绍的是 [`thunkify`](https://npmjs.org/package/thunkify)，用来对传统的 CPS 回调函数进行变形处理。

#### Sequential execution

利用上面介绍的内容，使用 Generator 重新实现网络爬虫的串行版本。

```js
var thunkify = require('thunkify');
var co = require('co');
var request = thunkify(require('request'));
var fs = require('fs');
var mkdirp = thunkify(require('mkdirp'));
var readFile = thunkify(fs.readFile);
var writeFile = thunkify(fs.writeFile);
var nextTick = thunkify(process.nextTick);
```

经过 thunkify 后，使用生成器函数版本的资源下载函数 `download` 变成跟同步函数一样简洁，线性写下来即可：

```js
function* download(url, filename) {
    console.log('Downloading ' + url);
    var results = yield request(url);
    var body = results[1];
    yield mkdirp(path.dirname(filename));
    yield writeFile(filename, body);
    console.log('Downloaded and saved:' + url);
    return body;
}
```

使用相同的方法可以写出 `spider()` 和 `spiderLinks()` 函数，最后使用 `co` 来调用生成器函数：

```js
co(function* () {
    try {
        yield spider(process.argv[2], 1);
        console.log('Download complete');
    } catch(err) {
        console.log(err);
    };
})();
```

#### Parallel execution

使用 Generator 并不有直接实现并行执行。但第三方库 `co` 提供了针对数组进行并发执行的功能。如下示例代码：

```js
co(function* () {
    var res = yield [
        Promise.resolve(1),
        Promise.resolve(2),
        Promise.resolve(3),
    ];
    console.log(res); // => [1, 2, 3]
}).catch(onerror);
```

`co` 的参数是个生成器函数，生成器函数 `yield` 了一个数组。数组里包含了一系列的任务，这些任务将会**并发**执行，直到所有任务都完成后，`yield` 才会返回。所以上面的示例代码将输出 `[1, 2, 3]` 。

## 20160715

Node.js Design Patterns: Chapeter 2 Asynchronous Control Flow Patterns

### Generator

#### Producer-consumer pattern

如果要使用 Generator 实现并发数据控制，我们有一些思路：

* 使用我们之前实现的基于回调的 `TaskQueue` 来实现并发数量控制。要使用这一机制，我们需要把 Generator 函数和其他函数通过 thunkify 进行变形。
* 使用我们之前实现的基于 Promise 的 `TaskQueue` 版本来进行并发数量控制。要使用这一机制，我们需要把 Generator 函数转换为 Promise 对象即可。
* 使用 `async` 进行并发数量控制。我们需要把 Generator 函数转换为普通的基于回调的函数。
* 使用 `co` 生态库 [co-limiter](https://npmjs.org/package/co-limiter) 进行并发数量控制。
* 实现生产者/消费者模型，用它来进行并发数量控制。这实际上也是 `co-limiter` 库的原理。

### 比较总结

几种异步流程控制的优缺点对比如下：

![Comparation](https://raw.githubusercontent.com/kamidox/blogs/master/images/js_async_flow.png)

## 20160716

### 以练习为核心的编程学习打卡社区

#### 核心业务流程

* 学生登录网站，查找需要练习的编程课程
* 进入课程后，逐个练习完成作业，每个练习配有思路讲解以及用到的关键技术介绍
* 上传作业发起打卡流程
* 系统运行测试例检查作业完成情况，如果失败学生可以选择重试或者参考标准答案，如果成功则记录打卡行为
* 学习完某个课程后，获得相关课程的徽章或证书，徽章和证书可分享到社交媒体

#### 内容运营开发

* 以 [exercism.io](http://exercism.io) 为蓝本，摘抄合理的练习
* 以领域内优质图书为蓝本，提取关键技术，开发想着练习

#### 课程示例

* JavaScript 快速入门
    * 课程目标：熟悉 JavaScript 语法；熟悉 JavaScript 标准库；使用 JavaScript 解决简单问题
    * 目标用户：有其他语言背影，想要学习 JavaScript 编程的程序员
    * 课程内容：以 exercism.io 为蓝本进行习题设计
* Node.js 设计模式
    * 课程目标：JavaScript 进阶；熟悉 Node.js 及其生态库的核心设计模式
    * 目标用户：熟悉 JavaScript 基础知识，想进一步深入 JavaScript 设计模式的程序员
    * 课程内容：以 Node.js Design Patterns 为蓝本，设计不同版本的爬虫程序来进行练习
* 轻松掌握 ES6：逐个知识点，配合适量的练习，掌握 ES6 新特性

#### 运营模式

前期开发一些课程，在微信公众号，掘金，简书等平台推广。后期设计充值打卡的赢利模式。如十节课，需要充值 100 元，完成一节课返还 10 元。超过期限没有完成课程的同学，没收课程充值费用。后期可设计，放弃某个练习，返还一半练习费用；参考标准答案扣除全部的练习费用。

## 20160720

Node.js Design Patterns: Chapeter 3 Coding With Stream

### Discovering the importance of streams

大部分的异步 IO API 使用 Buffering 模式，即数据缓存起来，然后一次性返回。比如 `fs.readFile()` 即工作在缓存模式下。而 Streaming 模式允许数据一到就直接被处理。流模式有以下两个优点：

* 空间效率高：当处理大文件（比如超过 1 GB）时，缓存模式可能会耗尽内存而出错。而流模式则没问题。具体可参阅[空间效率](#spatial_efficiency)。
* 时间效率高：缓存模式需要把数据全部缓存完了，才通过回调函数返回给调用者。而流模式只要有数据到达，就直接返回给调用者。具体可参阅[时间效率](#time_efficiency)。
* 可组合性：这个是流模式最显著的优点。可以通过 `pipe()` 函数把几个流处理程序组合起来。比如，如果需要在传输过程中加密，只需要在客户端加上代码 `.pipe(crypto.createCipher('aes192', 'a_shared_secret'))`，然后在服务端加上代码 `.pipe(crypto.createDecipher('aes192', 'a_shared_secret'))` 。这样就加上了加密功能。

<a name="spatial_efficiency"></a>**空间效率: 使用 GZip 算法压缩文件**

```js
var fs = require('fs');
var zlib = require('zlib');

// 压缩大于 1 GB 的文件时，会报错
function gzipBuffered(filename) {
    var start = Date.now();
    fs.readFile(filename, (err, data) => {
        zlib.gzip(data, (err, data) => {
            fs.writeFile(filename + '.b.gz', data, () => {
                console.log(`${filename} successful compressed. Elapsed ${Date.now() - start} ms.`);
            })
        })
    })
}

// 对超大文件工作良好
function gzipStreamed(filename) {
    var start = Date.now();
    fs.createReadStream(filename)
        .pipe(zlib.createGzip())
        .pipe(fs.createWriteStream(filename + '.s.gz'))
        .on('finish', () => {
            console.log(`${filename} successful compressed. Elapsed ${Date.now() - start} ms.`)
        })
}
```

<a name="time_efficiency"></a>**时间效率: 上传经过 gzip 压缩的文件**

```js
// 服务器端代码 gzip-stream-server.js
var http = require('http');
var fs = require('fs');
var zlib = require('zlib');

var server = http.createServer((req, res) => {
    var filename = req.headers.filename;
    console.log(`receiving ${filename} ...`);
    var start = Date.now();
    req.pipe(zlib.createGunzip())
        .pipe(fs.createWriteStream(filename + '.received'))
        .on('finish', () => {
            res.writeHead(201, {'Content-Type': 'text/plain'});
            res.end('File uploading successful.');
            console.log(`received ${filename} - elapsed ${Date.now() - start} ms`)
        });
});

server.listen(3000, () => {
    console.log('Listening on 3000 ...');
});

// 客户端代码 gzip-stream-client.js
var fs = require('fs');
var zlib = require('zlib');
var http = require('http');
var path = require('path');

var file = process.argv[2];
file = file || './gzip-stream-client.js';
var server = 'localhost';
var port = 3000;

var options = {
    hostname: server,
    port: port,
    method: 'PUT',
    headers: {
        filename: path.basename(file),
        'Content-Type': 'application/octet-stream',
        'Content-Encoding': 'gzip'
    }
};

var req = http.request(options, (res) => {
    console.log(`Server response: ${res.statusCode}`);
});

var start = Date.now();
fs.createReadStream(file)
    .pipe(zlib.createGzip())
    .pipe(req)
    .on('finish', () => {
        console.log(`${file} successful uploaded.  - elapsed ${Date.now() - start} ms`)
    });
```

## 20160821

[blockly-games](https://github.com/google/blockly-games) - Games for tomorrow's programmers.

### Blockly-Games - boot.js

Blockly-Games 应用程序的 BootLoader，负载载入指定的应用程序。

1. 根据 `location.pathname` 解析出应用的名称和语言
2. 把语言记录在 `window` 全局变量里
3. 根据解析出来的名称和语言，创建一个 `script` 标签，把应用对应的脚本 `compressed.js` 或 `uncompressed.js` 插入到页面里

### Blockly-Games - deps

构建 Blockly-Games 用到的工具和库。

* [closure-library](https://github.com/google/closure-library): Google's common JavaScript library
  Closure Library is a powerful, low-level JavaScript library designed for building complex and scalable web applications. It is used by many Google web applications, such as Google Search, Gmail, Google Docs, Google+, Google Maps, and others.
* [closure-templates](https://github.com/google/closure-templates): A client- and server-side templating system that helps you dynamically build reusable HTML and UI elements
  [helloword_js](https://developers.google.com/closure/templates/docs/helloworld_js) is a example to getting started.
* [closure-compiler](https://github.com/google/closure-compiler): A JavaScript checker and optimizer.
  The Closure Compiler is a tool for making JavaScript download and run faster. It is a true compiler for JavaScript. Instead of compiling from a source language to machine code, it compiles from JavaScript to better JavaScript. It parses your JavaScript, analyzes it, removes dead code and rewrites and minimizes what's left. It also checks syntax, variable references, and types, and warns about common JavaScript pitfalls.
* [closurebuilder.py](https://github.com/google/closure-library/blob/master/closure/bin/build/closurebuilder.py): Utility for Closure Library dependency calculation.
  ClosureBuilder scans source files to build dependency info.  From the dependencies, the script can produce a manifest in dependency order, a concatenated script, or compiled output from the Closure Compiler.

### Blockly-Games - build

Blockly-Games 应用程序编译流程。

1. 使用 `closure-templates` 格式编写后缀为 `.soy` 的源代码。并把它编译成 JavaScript 文件 `soy.js`。
2. 最后通过 `closurebuilder.py` 工具，把所有依赖的文件打包，生成 `compressed.js` 和 `uncompressed.js`。
3. Apps 的 `index.html` 通过 `boot.js` 载入应用对应的 `compressed.js` 或 `uncompressed.js`。

这样就构建出了一个完整的应用。其中 `uncompressed.js` 代码是关键：

```js
function loadScript() {
    var src = srcs.shift();
    if (src) {
      var script = document.createElement('script');
      script.src = src;
      script.type = 'text/javascript';
      // 下面的代码是关键：第一个脚本载入完成后会载入第二个脚本，直到所有 JavaScript 全部载入为止
      script.onload = loadScript;
      document.head.appendChild(script);
    }
  }
  loadScript();
```

## 20160822

<a name="google-closure-templates"></a>

### Google Closure Templates

Closure Templates are a client- and server-side templating system that helps you dynamically build reusable HTML and UI elements.

* 传统的模板 (如 jinja2 etc.) 需要为每个页面制作一个巨大的模板文件。而 Closure Templates 的思想则是定义 UI 组件，这些 UI 组件可以在不同的页面里重用。*注：这一点有点牵强，很多模板框架支持 include 机制引用页面片段。*
* 原生的多语言支持。
* 可以把 Closure Templates 编译成 JavaScript 或 Java 语言，这样可以在客户端和服务器重用代码。

*在客户端生成页面的技术就叫 Client Rendering, 在服务器生成页面的技术就叫 Server Rendering.*

What are the benefits of using Closure Templates?

* Convenience. Closure Templates provide an easy way to build pieces of HTML for your application's UI and help you separate application logic from display.
* Language-neutral. Closure Templates work with JavaScript or Java. You can write one template and share it between your client- and server-side code.
* Client-side speed. Closure Templates are compiled to efficient JavaScript functions for maximum client-side performance.
* Easy to read. You can clearly see the structure of the output HTML from the structure of the template code. Messages for translation are inline for extra readability.
* Designed for programmers. Templates are simply functions that can call each other. The syntax includes constructs familiar to programmers. You can put multiple templates in one source file.
* A tool, not a framework. Works well in any web application environment in conjunction with any libraries, frameworks, or other tools.
* Battle-tested. Closure Templates are used extensively in some of the largest web applications in the world, including Gmail and Google Docs.
* Secure. Closure Templates are contextually autoescaped to reduce the risk of XSS.

### Closure Templates HelloWorld

[Hello World Using JavaScript](https://developers.google.com/closure/templates/docs/helloworld_js) 是个快速了解 Closure Templates 的入门示例。几个关键步骤：

1. 下载 [Closure Templates JavaScript Compiler](https://dl.google.com/closure-templates/closure-templates-for-javascript-latest.zip)
  使用这个工具把 Closure Templates 源文件 `.soy` 编译成 JavaScript 源码
2. 按照 Closure Templates 规范编写 `.soy` 文件。目的是定义一系列可重用的 UI 组件。这些 UI 组件可以包含基本的程序化定制的内容，如参数，条件判断，循环等等。还可以使用 Closure Templates 的 `call` 命令调用其他 `.soy` 文件里定义的 UI 组件。
3. 使用 Closure Templates JavaScript Compiler 把 `.soy` 文件编译生成 JavaScript 文件。每个 template 会被编译成一个同名的 JavaScript 函数。
4. 创建 html 文件，通过 `<script>` 标签引用生成的 JavaScript 文件。并且在 html 文件里通过 JavaScript 调用之前定义在 `.soy` 文件里的 UI 组件。再通过 `document.write` 函数把 UI 组件的内容写到 html dom 树里。

本质上，Closure Templates 还是使用 JavaScript 输出 html 界面元素，再通过 `document.write` 输出到 html dynamic dom 里。那为什么要发明一个 Closure Templates，绕一大圈来做这个事情呢？

画外音响起，是一个中年男子磁性又略带沧桑的声音：我们到达一个个目的地，却忘记了为什么出发。是时候停下脚步 ...

其实，[Closure Templates 的设计目标及特点](#google-closure-templates) 就是目的。

## 20160823

### Blockly-Games - Multi-languages

Blockly-Games 多语言使用 Google Closure Templates 来实现的，其[官方文档](https://developers.google.com/closure/templates/docs/translation)有详细的步骤说明。

多语言的编译步骤如下：

1. 在 `.soy` 文件里，把所有需要翻译的文本用 `msg` 命令标记
2. 下载 [closure-templates-msg-extractor-latest.zip](https://dl.google.com/closure-templates/closure-templates-msg-extractor-latest.zip)
3. 运行 `SoyMsgExtractor.jar` 用来从 `.soy` 文件里提取出所有待翻译的文本，并生成 XLIFF 格式
4. 运行 `i18n/xliff_to_json.py` 把 XLIFF 文件转成 json 文件，放在 `json` 目录下
5. 运行 `i18n/json_to_js.py` 把 `.soy` 文件编译成对应语言的 `soy.js` 源文件
6. 运行 `python build-app.py` 生成每种语言 `compressed.js` 和 `uncompressed.js` 文件

### Blockly-Games - closure-templates

`.soy` 文件规范。

### Blockly-Games - MVC

Blockly-Games 的 MVC 架构。

### 20160826

#### 欺诈交易异常检测

核心思路：从数据集里，找出大部分帐户的行为模式，以此建立模型。如果一个帐户的行为模式与此模型偏差较远，则可定义为异常。
欺诈交易：并非所有的异常帐户都有欺诈交易，但我们可以通过人工参与的方式从异常帐户里人为发现一些线索，从而优化特征选择，从而不断地优化模型。

经过不断优化后的模型，检测出来的异常帐户中，大部分都是欺诈交易。则这个模型就是个成功的模型。

**行为模式**

* 被欺诈的帐户是否有这样的特点：欺诈发生时的一段时间内（比如一周）的资金转出量会不会比平常大很多？
  如果有这样的特征，则可使用[PGA 算法](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.24.5743&rep=rep1&type=pdf)来检查出这些帐户。
* 实施欺诈的帐户资金动向有哪些特点？比如多次均衡地有资金转入，但一次性有大比资金转出？
  如果有这样的特征，也可以使用 PGA 算法检测出来。
* 检测出可能被欺诈的帐户后，再看看可能实施了欺诈的帐户列表。看看他们之间有没有资金往来，如果有，这两个帐户存在欺诈的可能性就更大了。

#### 实施步骤

1. 清洗数据
   把不活跃帐户信息过滤掉，以免模型过多地关注这些不活跃的帐户的行为模式。怎么样定义不活跃帐户？
2. 提取特征
   根据行为模式的分析，提取特征
3. 建立模型
   建立模型，并拿测试数据来训练算法
4. 检测算法
   拿算法来找出异常的帐户，人工确认其是否存在欺诈，如果不存在欺诈，则优化特征。以此不断迭代，直到找出合理的模型。

#### 高斯分布异常检测模型

使用高斯分布进行检测，这一算法简单方便，可以较快地得出异常帐户列表。但这些异常帐户是否存在欺诈交易需要人工鉴别，并且根据鉴别的结果，进一步优化特征。这一算法的其他挑战有：

1. 所有的特征需要逐一检查，让其**独立地满足正态分布**，否则不能适用这个算法。
2. 训练数据是一个超级大矩阵，因为我们需要把用户的一段时间内所有行为数据都提取为特征，并且和所有的用户组成一个超级大矩阵。这样在非分布式计算的环境下，要训练这样的模型可能性基本为零。不过可以拿小部分数据试运行，检测模型的合理程度。如果可行，再想办法解决运算量的问题。

#### 欺诈交易异常检测

1. 根据 PGA 算法检测可能被欺诈的帐户
2. 根据 PGA 算法检测可能实施欺诈的帐户
3. 检查被欺诈帐户和实施欺诈帐户之间的资金关联关系，如果存在强关联关系，则这组欺诈可能将变得很高







