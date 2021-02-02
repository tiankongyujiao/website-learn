### 为啥要打包，js的模块化
需要打包的原因就在于js的模块化。
es6之前，js原生是没有模块化规范。
##### CommonJS
node的模块化规范，在node里面通过文件的形式来组织js代码，让js代码之间作用域隔离，同时通过require和module.exports来对模块进行导入和导出。
##### AMD：异步模块定义Asynchronous Module definition
一个适合浏览器端运行的js模块化规范。  
AMD所有模块默认都是异步加载的，而CommonJS是同步加载的，因为如果在web端也是用同步加载，那么页面在解析脚本文件的过程中可能使页面暂停使用。   
需要前置引入一个require.js库，然后通过define来定义一个模块，通过require来加载一个模块，通过回调的形式来确定模块已经加载成功了。  

和CommonJS一样不管加载多少次，都只会加载进来一次，后面加载的都是缓存。也就是是单例的。

require.js的原理是使用jsonp来对异步模块进行加载。
##### CMD是国内比较流行的模块化规范，AMD是国际流行的模块化规范，AMD已经包容了CMD。
##### UMD是一种同构的情况，并不算是一种模块化规范，只是兼容了AMD，CMD和CommonJS。
##### 比较经典的就是CommonJS和AMD
##### CommonJS和AMD的共同特点
+ **语言上层**的**运行环境**中实现的模块化规范，模块化规范有环境自己定义。
+ 相互之间不能共用模块，例如不能在node.js运行AMD模块，不能直接在浏览器中运行CommonJS模块。

#### ESModule
在es6之后，js有了**语言层面**的模块化导入导出关键词与语法以及与之匹配的ESModule规范。使用ESModule，我们可以使用import和export两个关键词来对模块进行导入导出。  
大部分环境都不支持直接使用ESModule，所以我们要对ESModule模块进行编译。

#### js的运行环境有三大类：1.node.js 2.浏览器 3.小程序
每个运行环境都有一个解析器，否则这些环境也不会认识js语法。解析器的作用就是用ECMAScript规范去解释js语法，也就是处理和执行语言本身的内容。  
字解析器的上层，每个运行环境都会在解释器的基础上封装一些环境相关的API，例如node.js中的global对象、process对象，浏览器中的window对象、document对象。

#### babel：把js core中高版本规范的语法能按照相同语义在静态阶段转化为低版本语法，这样即便是早期的浏览器，他们的内置js解释器也能看懂。
然而不幸的是，babel会把import和export便以为CommonJS规范的require和module.exports，这样就不能在浏览器环境中使用了。为了在web端直接使用CommonJS规范的模块，除了使用babel编译之外，我们还需要一个步骤叫做**打包**（bundle）。  
打包工具的作用就是将模块化内部实现的细节抹平，无论是AMD还是CommonJS模块化规范的模块，经过打包之后都能变成能直接运行在node.js或者web的内容。


