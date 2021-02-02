1.Symbol是一种原始数据类型，表示独一无二的值，Symbol值用Symbol()函数生成。它是JavaScript语言的第七种数据类型，前六种是布尔值Boolean,数值Number,
字符串String,对象Object,null和undefined。
var s = Symbol();
typeof(s);  //'Symbol'
注意：Symbol函数前面不能使用new运算符，这是因为生成的Symbol是原始类型的值，不是一个对象。
Symbol值不是对象，它是一种类似于字符串的数据类型，所以不能添加属性。

2.Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
var s1 = Symbol('foo');
var s2 = Symbol('bar');
s1 // Symbol(foo)
s2 // Symbol(bar)
s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
如果不加参数，那么输出的s1,s2都是Symbol(),不利于区分，加的参数相当于是对Symbol加上的描述。

3.Symbol函数返回的值都是不相等的（不适用Symbol.for的情况下），不管是否加参数，即使加的参数是完全一样的(因为Symbol函数的参数只是表示对当前 Symbol 
值的描述)，如：
var s1 = Symbol();
var s2 = Symbol();
s1 === s2; // fasle

var s3 = Symbol('foo');
var s4 = Symbol('foo');
s3 === s4; // false

4.Symbol值不能与其他类型的值进行运算，会报错：
var sym = Symbol('My symbol');
"your symbol is " + sym;  // TypeError: can't convert symbol to string
`your symbol is ${sym}`;  // TypeError: can't convert symbol to string
但是Symbol值可以显示的转换为字符串：
var sym = Symbol('My symbol');
String(sym) // 'Symbol(My symbol)'
sym.toString() // 'Symbol(My symbol)'
Symbol值也可以转换为布尔值，但是不能转换为数值：
var sym = Symbol();
Boolean(sym) // true
!sym  // false

Number(sym); //TypeError

5.Symbol的用途：
（1）作为对象的属性名：保证对象的属性名的唯一性。此时访问对象的属性时，不能‘.’运算符（点运算符后面总是字符串，会忽略Symbol），必须放[]里面。
（2）定义常量时使用Symbol，保证常量值互不相等。
（3）消除魔术字符串（魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值）

6.Symbol作为对象的属性，对象使用for...in，for...of遍历时，不会出现在循环中，也不会被Object.keys(),Object.getOwnPropertyNames(),JSON.stringify()
返回，它也不是私有属性，它有一个Object.getOwnPropertySymbols()方法，通过这个方法可以获取一个对象的所有Symbol属性名。
Object.getOwnPropertySymbols方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

7.Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值。
由于Symbol()写法没有登记机制，所以每次调用都会返回一个不同的值。
Symbol.for为Symbol值登记的名字，是全局环境的，可以在不同的 iframe 或 service worker 中取到同一个值
```
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');
s1 === s2 // true
```
Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key
```
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"
Symbol.keyFor(s2) // undefined
```
8.内置的Symbol值：（11个）
（1）Symbol.hasInstance 对象是属性，调动instanceof时调用
（2）Symbol.isConcatSpreadable 对象的属性表示一个布尔值，表示对象调用Array.prototype.concat()时，是否可以展开。
（3）Symbol.species
（4）Symbol.match
（5）Symbol.replace
（6）Symbol.search
（7）Symbol.split
（8）Symbol.iterator
（9）Symbol.toPrimitive
（10）Symbol.toStringTag
（11）Symbol.unscopables

