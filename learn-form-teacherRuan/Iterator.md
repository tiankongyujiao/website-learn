#### 1.Iterator接口：
Iterator接口为各种不同的数据结构（数组，对象，Map，Set等）提供统一的访问机制。
任务数据结构只要部署了Iterator接口，这种数据结构就是可遍历的，即可以完成遍历操作，依次处理该数据结构的所有成员。
**Iterator接口的作用：**
（1）为各种数据结构提供统一的，简便的访问接口，即for...of循环（es6新语法），当使用for...of循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。
（2）使得数据结构的成员能够按照某种次序排序。

遍历器对象本质上是一个指针对象（指针对象拥有next方法），开始指向数据结构的起始位置。
第一次调用指针对象next方法，可以将指针指向数据结构的第一个成员，下次调用next就指向数据结构的第二个成员，依次类推，直到它指向数据结构的结束位置。
每次调用指针对象的next方法都会返回两个值，即value和done，分别表示当前数据结构的值，和遍历是否结束。
```
var it = makeIterator(['a','b']);  // it是遍历器对象
it.next() // { value: "a", done: false }
it.next() // { value: "b", done: false }
it.next() // { value: undefined, done: true }
```
//遍历器生成函数
```
function makeIterator(arr){
  var index = 0;
  retrun{
    //next方法用来移动指针对象的位置，开始指向数组的开始位置，第一次移动指向数组的第一个元素‘a’, 第二次调用指向‘b’。
    next: function(){
      retrun index < arr.length?
        {value:arr[index++],done:false}:
        {value:undefined,done:true};
    }
  }
}
```
调用指针对象的next方法就可以遍历事先给定的数据结构。
done：false 和 value:undefined 是可以省略的。

#### 2.Symbol.iterator
当使用for...of循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口，默认的Iterator接口部署在数据结构的Symbol.iterator属性上，这是一个预定义好的，
类型为Symbol的值，所以属性名要用[]括起来。

**Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。**

原生具备Iterator接口的数据结构：数组Array，Map, Set, String，函数的arguments对象，DOM NodeList对象(使用js原生方法获得的对象，使用jQuery获取的
不具有Symbol.iterator属性，或者说使用jQuery的$符号获取的集合不是NodeList)，以及TypedArray(据说是固定长度缓冲区类型，允许读取缓冲区中的二进制数据)。
这些数据结构原生部署了Symbol.iterator属性，即部署了遍历器接口，所以可以直接使用for...of遍历。

3.注意对象Object原生不具有Symbol.iterator属性，因为Object不是一种线性结构，是无序的，遍历器是一种线性处理，部署遍历器接口就是部署一种线性转换。
  一个对象如果要具备可被for...of循环调用的Iterator接口，就必须在Symbol.iterator属性上部署遍历器生成函数（原型链上的对象具有该方法也可以）。

（a）一个最简单直接的给对象部署遍历器接口的方法就是把数组的遍历器方法赋值给它（即对象的Symbol.iterator属性直接引用数组的Iterator接口Symbol.iterator）。
这种方法只对键值是递增的数值（数值键名），有lenght属性(具有Iterator接口的对象一定具有length属性)的对象有用，如：
```
let iterable = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3,
  [Symbol.iterator]: Array.prototype[Symbol.iterator]  //遍历器生成函数  或者 [][Symbol.iterator];
};
for (let item of iterable) {
  console.log(item); // 'a', 'b', 'c'
}
```
（b）还可以将Object的键名生成一个数组以后遍历，Object.keys(obj),如：
```
var obj = {name:'zj',age:'27'};
for(var key of Object.keys(obj)){
    console.log(key);  //name,age
}
```
（c）还有一个遍历Object的方法：使用Generator函数包装起来，如：
```
function* entries(obj){
    for(let key of Object.keys(obj)){
      yield [key,obj[key]];
    }
}
for(let [ley,value] of entries(obj)){
  console.log( key +'->'+ value)
}
//name -> zj
//age -> 27
```

#### 4.调用Iterator接口的场合：
有一些场合会默认调用Iterator接口（即Symbol.iterator方法），除了for...of循环，还有几个场合：
（1）解构赋值
（2）扩展运算符（...）也会调用默认的Iterator接口。只要一个数据结构部署了Iterator接口，就可以对它使用扩展运算符将其转换为数组。
    let arr = [...iterable];
（3）yield *
yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。
```
let generator = function* () {
  yield 1;
  yield* [2,3,4];
  yield 5;
};

var iterator = generator();

iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```
（4）其他场合
由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口，例如Map(),Set(),Array.from(),
