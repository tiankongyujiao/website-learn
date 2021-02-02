# es6迭代器和生成器
## 迭代器
> **可迭代对象**：一个对象必须实现 @@iterator 方法, 意思是这个对象（或者它原型链 prototype chain 上的某个对象）必须有一个名字是 Symbol.iterator 的属性。  
>**迭代器对象**: 有next方法（必须），return方法（可选，如果for...of循环提前退出（通常是因为出错，或者有break语句或continue语句），就会调用return方法），throw方法（可选）

#### 原生具备Iterator接口：数组、Set和Map结构、某些类似数组的对象（比如字符串、arguments对象、DOM NodeList 对象）
并不是所有类似数组的对象都具有 Iterator 接口，一个简便的解决方法，就是使用Array.from方法将其转为数组。
```
let arrayLike = { length: 2, 0: 'a', 1: 'b' };

// 报错
for (let x of arrayLike) {
  console.log(x);
}

// 正确
for (let x of Array.from(arrayLike)) {
  console.log(x);
}
```
#### 调用iterator接口的场合
1. 结构赋值：对数组和Set结构进行解构赋值时，会默认调用Symbol.iterator方法。
```
let [a, ...rest] = [1,2,3,4] 
// a = 1, rest = [2,3,4]
```
2. 扩展运算符：扩展运算符（...）也会调用默认的iterator接口
```
let arr = [1, 2]
let arr2 = [3, 4, ...arr, 5]
// arr2 = [3,4,1,2,5]
```
可以将任何部署了Iterator接口的数据结构，转为数组:
```
let arr = [...iterable]
```
3. yield*:yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。
```
let generator = function* () {
  yield 1;
  yield 2;
  yield* [3,4];
  yield 5;
}
var iterator = generator();
iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```
4. 其他场合：由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口。下面是一些例子。
+ for...of
+ Array.from()
+ Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）
+ Promise.all()
+ Promise.race()
> Symbol.iterator方法的最简单实现是Generator函数。
## 生成器
> 生成器从语法上可以理解为一个状态机，封装了多个内部状态。  
> 形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield语句，定义不同的内部状态（yield在英语里的意思就是“产出”）。
```
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
// 上述helloWorldGenerator生成器函数有三个状态：hello，world和return语句（结束执行）
```
调用生成器函数以后，函数并不会立即执行，返回的是一个指向内部状态的指针对象，也就是遍历器对象（返回的遍历器对象也就是生成器函数的内部指针对象）。  
Generator函数是分段执行的，yield语句是暂停执行的标记，而next方法可以恢复执行。  

迭代器和生成器的关系：迭代器（遍历器）可以由生成器实现（个人理解），执行一个生成器函数会返回一个遍历器对象，也就是说生成器函数不仅是一个状态机，也是一个遍历器对象生成函数，返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态

> 生成器对象是由生成器函数（[generator function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*)）返回的,并且它符合可迭代协议和迭代器协议。

生成器函数为JavaScript提供了手动的“惰性求值”（Lazy Evaluation）的语法功能：
```
function* gen() {
  yield  123 + 456;
}
// 上面代码中，yield后面的表达式123 + 456，不会立即求值，只会在next方法将指针移到这一句时，才会求值
```

**yield语句注意点**：
1. yield语句只能用在 Generator 函数里面，用在其他地方都会报错。
2. yield语句如果用在一个表达式之中，必须放在圆括号里面。
```
  console.log('Hello' + yield); // SyntaxError
  console.log('Hello' + yield 123); // SyntaxError

  console.log('Hello' + (yield)); // OK
  console.log('Hello' + (yield 123)); // OK
}
```
3. yield语句用作函数参数或放在赋值表达式的右边，可以不加括号。
```
function* demo() {
  foo(yield 'a', yield 'b'); // OK
  let input = yield; // OK
}
```

### 生成器函数与 Iterator 接口的关系
> 任意一个对象的Symbol.iterator方法，等于该对象的遍历器生成函数，调用该函数会返回该对象的一个遍历器对象。

由于Generator函数就是遍历器生成函数，因此可以把Generator赋值给对象的Symbol.iterator属性，从而使得该对象具有Iterator接口。
```
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] 
// 上面代码中，Generator函数赋值给Symbol.iterator属性，从而使得myIterable对象具有了Iterator接口，可以被...运算符遍历了。
```
> Generator函数执行后，返回一个遍历器对象。该对象本身也具有Symbol.iterator属性，执行后返回自身。
```
function* gen(){
  // some code
}

var g = gen();

g[Symbol.iterator]() === g
// true
// 上面代码中，gen是一个Generator函数，调用它会生成一个遍历器对象g。它的Symbol.iterator属性，也是一个遍历器对象生成函数，执行后返回它自己。
```

### next方法
> yield句本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield语句的返回值。 

next方法的参数表示上一个yield语句的返回值，所以第一次使用next方法时，不能带有参数。V8引擎直接忽略第一次使用next方法时的参数，只有从第二次使用next方法开始，参数才是有效的。
```
function* dataConsumer() {
  console.log('Started');
  console.log(`1. ${yield}`);
  console.log(`2. ${yield}`);
  return 'result';
}

let genObj = dataConsumer();
genObj.next();
// Started
genObj.next('a')
// 1. a
genObj.next('b')
// 2. b
```
### for...of循环
1. for...of循环在在遍历生成器函数返回的遍历器对象时不包括return语句返回的值。
2. for...of循环可以与break、continue和return配合使用。
3. 使用generator函数和for...of循环实现斐波那契数列：
```
function* fibonacci() {
  let [prev, curr] = [0, 1];
  for (;;) {
    [prev, curr] = [curr, prev + curr];
    yield curr;
  }
}

for (let n of fibonacci()) {
  if (n > 1000) break;
  console.log(n);
}
```
4. 利用for...of循环，可以写出遍历任意对象（object）的方法：
```
function* objectEntries(obj) {
  let propKeys = Reflect.ownKeys(obj);

  for (let propKey of propKeys) {
    yield [propKey, obj[propKey]];
  }
}

let jane = { first: 'Jane', last: 'Doe' };

for (let [key, value] of objectEntries(jane)) {
  console.log(`${key}: ${value}`);
}
// first: Jane
// last: Doe
// 对象jane原生不具备Iterator接口，无法用for...of遍历。这时，我们通过Generator函数objectEntries为它加上遍历器接口，就可以用for...of遍历了。
```
### Generator函数返回的遍历器对象有两个方法
+ Generator.prototype.throw()
1. 不要混淆遍历器对象的throw方法和全局的throw命令。
2. 如果Generator函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获。
3. 生成器对象的throw方法被捕获以后，会附带执行下一条yield语句。

+ Generator.prototype.return()
1. 执行return方法以后，Generator函数的遍历就终止了，返回值的done属性为true，以后再调用next方法，done属性总是返回true。
2. 如果Generator函数内部有try...finally代码块，那么return方法会推迟到finally代码块执行完再执行。

### yield* 语句
1. 从语法角度看，如果yield命令后面跟的是一个遍历器对象，需要在yield命令后面加上星号，表明它返回的是一个遍历器对象。
```
let delegatedIterator = (function* () {
  yield 'Hello!';
  yield 'Bye!';
}());

let delegatingIterator = (function* () {
  yield 'Greetings!';
  yield* delegatedIterator;
  yield 'Ok, bye.';
}());

for(let value of delegatingIterator) {
  console.log(value);
}
// "Greetings!
// "Hello!"
// "Bye!"
// "Ok, bye."
// delegatingIterator是代理者，delegatedIterator是被代理者。由于yield* delegatedIterator语句得到的值，是一个遍历器，所以要用星号表示。运行结果就是使用一个遍历器，遍历了多个Generator函数，有递归的效果。
```
2. 如果yield*后面跟着一个数组，由于数组原生支持遍历器，因此就会遍历数组成员。
```
function* gen(){
  yield* ["a", "b", "c"];
}

gen().next() // { value:"a", done:false }
// 上面代码中，yield命令后面如果不加星号，返回的是整个数组，加了星号就表示返回的是数组的遍历器对象。
```
3. 任何数据结构只要有Iterator接口，就可以被 yield* 遍历。
```
let read = (function* () {
  yield 'hello';
  yield* 'hello';
})();

read.next().value // "hello"
read.next().value // "h"
```
4.如果被代理的Generator函数有return语句，那么就可以向代理它的Generator函数返回数据。
```
function* genFuncWithReturn() {
  yield 'a';
  yield 'b';
  return 'The result';
}
function* logReturned(genObj) {
  let result = yield* genObj;
  console.log(result);
}

[...logReturned(genFuncWithReturn())]
// The result
// [ 'a', 'b' ]
```
5. yield* 命令可以很方便地取出嵌套数组的所有成员。
```
function* iterTree(tree) {
  if (Array.isArray(tree)) {
    for(let i=0; i < tree.length; i++) {
      yield* iterTree(tree[i]);
    }
  } else {
    yield tree;
  }
}

const tree = [ 'a', ['b', 'c'], ['d', 'e'] ];

for(let x of iterTree(tree)) {
  console.log(x);
}
// a
// b
// c
// d
// e
```

### 生成器函数和普通函数的区别：
1. 生成器函数返回的是遍历器对象，是生成器函数的实例，集成了生成器的prototype。
2. 生成器函数的实例不是this对象。和用new生成的实例不一样，用new生成的普通的构造函数的实例有this对象，在构造函数的this上面添加一个属性，实例对象能够访问到，但是生成器函数的实例对象访问不到。生成器函数不能和new一起使用。
```
function* g() {
  this.a = 11;
}

let obj = g();
obj.a // undefined
```
**以上内容均来自廖雪峰的[es6教程](http://jsrun.pro/tutorial/cZKKp)，包括示例，本文仅方便自己加深印象和复习使用**
