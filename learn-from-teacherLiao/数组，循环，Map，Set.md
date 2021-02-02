#### 1.Array的方法里面直接修改当前Array的是splice，reserve，sort，join等
splice一个数组万能方法，可以增加，可以删除，返回的是删除的结果，如：
var arr = [1,2,3,4,5,6];
arr.splice(2,2,4,5,6); // [3,4] 删除然后增加
console.log(arr);  // [1, 2, 4, 5, 6, 5, 6]
arr.splice(2,2);   // [4,5] 只删除 不增加
console.log(arr);  // [1, 2, 6, 5, 6]
arr.slice(2,0,11,12,13); // []
console.log(arr); // [1, 2, 11, 12, 13, 6, 5, 6]

#### Array数组方法里面不直接修改当前数组的方法有：pop,push,shift,unshift,concat,slice等，他们不会修改当前Array，而是返回一个新数组。
数组的slice()方法和字符串的substring()一个用法，第一个参数是起始位置，第二个参数是结束为止，包含其实为止，不包含结束位置。而字符串的substr()方法也是
截取字符串，它和substring()不同的是，它的第二个参数是截取的字符串的长度，而不是结束位置。

2.JavaScript用一个{...}表示一个对象，键值对xxx: xxx形式申明，用‘，’隔开，最后一个键值对末尾不要加‘，’，如果加了，地版本的IE将会报错。

JavaScript规定方位一个不存在的属性返回undefined，并不报错，如：
```
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
```
删除一个不存在的属性也不报错，如：
delete xiaoming.grade; 

检查一个对象是否拥有一个属性，可以使用in操作符，如：
```
console.log('age' in xiaoming);   // true
console.log('grade' in xiaoming); // false
```
判断一个属性是否在一个对象中存在，用in操作符判断的时候，如果这个属性在这个对象的原型上，也返回true，如：
```
console.log('toString' in xiaoming);  // true
```
要判断一个属性是否自身拥有而不是集成来的可以使用hasOwnProperty,如：
```
console.log(xiaoming.hasOwnProperty('name'));     // true
console.log(xiaoming.hasOwnProperty('toString')); // false
```

3.条件判断语句中，JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true。

### 4.js中的几种循环遍历器：
(1)for循环：通过初始条件、结束条件和递增条件来循环执行语句块，for循环的三个条件都是可以省略的，这时可以通过break跳出循环，否则三个条件都省略就是死循环。

(2)for...in循环：for循环的变体
for(var key in object){}
key是对象的键值，由于Array也是对象，也可以使用for...in循环，此时key就是数组的下标。
注意：for...in对Array循环得到的key是String,而不是Number。

for...in会遍历对象proto以上能直接看到的属性
所以如果不确定iterable对象是否仅仅包含需要遍历的属性
最好还是用forEach,for...of,或者while这些

(3)while循环。
(4)do...while循环：循环至少执行一次。

(5)for...of循环：es6新语法，具有iterable类型的集合可以通过新的for ... of循环来遍历，可以使用break，continue和retrun跳出循环，如数组，对象，Map,Set等
for(var ele of object){}
ele是对象的值，而不是键，是数组的值，而不是索引。
for ... of遍历读取的是键值，要想获取键名，可以通过数组实例的entries()好keys()方法，values()用来获取键值，例：
for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
ES6的数组Array、Set、Map都部署了entries(),keys(),values()方法，调用后都返回遍历器对象：
(a) entries()返回遍历器对象，用来遍历[键名，键值]组成的数组，对于数组，键值就是索引值，对于Set，键名与键值相同。
Map 结构的 Iterator 接口，默认就是调用entries方法。
(b) keys()返回一个遍历器对象，用来遍历所有的键名。
(c) values()返回一个遍历器对象，用来遍历所有的键值。

##### for...in和for...of的区别：
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    alert(x); // '0', '1', '2', 'name'
}
```
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    alert(x); // 'A', 'B', 'C'
}
```
for ... in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。不仅遍历数字键名，还会遍历手动添加的其它键名，甚至包括原型上的键。一个Array数组实际上
也是一个对象，它的每个元素的索引被视为一个属性。
for ... in循环将把name包括在内，但Array的length属性却不包括在内。for...in是为遍历对象而设计的，不适合数组。
for ... of循环则完全修复了这些问题，它循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。

(6)更好的方式是直接使用iterable内置的forEach方法，它只接收一个函数，每次迭代都自动调用这个函数:
```
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {//左侧的参数不能省略，比如想获得index，则element必须加上，不能直接省却第一个，第一个表示的就是当前
元素的值，不管你用什么表示
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    alert(element);
});
```
Set和Array类似，但是Set没有索引，因此回调函数的前两个参数都是元素本身，第三个参数为Set自身。
```
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {//左侧的参数不能省略，比如想获得index，则element必须加上，不能直接省却第一个，第一个表示的就是当前
元素的值，不管你用什么表示
    alert(element);
});
```
Map的回调函数参数依次为value、key和map本身：
```
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    alert(value);
});
```

forEach的弊端在于无法中途跳出forEach循环，break命令或return命令都不能奏效。


