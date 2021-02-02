学习笔记（http://www.cnblogs.com/freelyflying/p/6978126.html，
https://github.com/zhengweikeng/blog/blob/master/posts/2016/%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0%E4%B8%ADthis%E7%9A%84%E7%94%A8%E6%B3%95.md）
## 1.箭头函数
es6引入了箭头函数（arrow function），箭头函数和普通函数的this不一样，它从本质上来讲没有自己的this，它的this是派生而来的，根据词法作用域派生而来。
它默认指向它在定义时所处的对象，而不是执行时所处的对象。（继承父级的this）
箭头函数没有自己的this，它的this是由它的普通父级函数（非箭头函数）决定，普通父级函数（非箭头函数）的this就是箭头函数的this。
箭头函数式不能通过applay，call来指定this的，因为它没有自己的this，如果使用了applay，和call函数，那么applay和call的第一个参数默认会被忽略。
但是箭头函数的普通父级函数（非箭头函数）是可以使用applay和call函数来指定this的，这时候箭头函数的this随普通父级函数（非箭头函数）而定。
（1）实例1：
```
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2017); // 27
```
(2)实例2：
```
var obj = {
  field: 'hello',
  getField: () => {
    console.log(this.field)  
  }
}
obj.getField() // undefined     因为getField中的this就是window，而window是没有field这个属性的，所以就是undefined了
```
所以我们一般不建议对象中定义函数的时候使用Arrow Function，毕竟this就会造成错误了。所以应该这么写
```
var obj = {
  field: 'hello',
  getField(){
    console.log(this.field)  
  }
}
obj.getField(); // hello
```
(3)实例3：
```
function taskA() {
  var arrow_fn = () => {
    console.log(this);  //obj
    console.log(this.name);  //Jack
  }
  arrow_fn()
}
var obj = {name: 'Jack'}
taskA.apply(obj);
```
解释一下：此时执行taskA.apply(obj),arrow_fn的this是定义时决定的，即指向普通父函数taskA（非箭头函数）的this，此时taskA使用apply改变了this的指向，它
指向了新的对象obj，所以返回的this是obj，this.name是Jack。

### 2.普通函数的this：
普通函数的this是运行时决定的。

### 3.ES6中箭头函数与普通函数this的区别：
普通函数中的this：
（1）this总是代表它的直接调用者，如obj.func,此时func中的this就指向obj。
（2）在默认情况下（非严格模式下），如果没有找到this，就指向window。
（3）在默认情况下（严格模式下），如果没有找到this，this是undefined。
（4）使用apply，call，bind(es5新增)绑定的，this指向绑定的对象（bind和apply，call的区别就是bind返回的仍是一个函数，调用完以后还要使用()执行，
    而apply和call是对函数的直接调用。）
 ### 箭头函数中的this：
 （1）默认指向在定义它时,它所处的对象,而不是执行时的对象, 定义它的时候,可能环境是window（即继承父级的this）;
