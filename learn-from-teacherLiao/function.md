1.函数有返回值，通过return返回，加了return语句后，函数就立马执行完毕，并返回结果，如果没有return语句，函数最后执行完毕以后返回undefined。

2.函数也是一个对象，函数名是指向函数的变量。

3.函数的参数个数可以多传，也可以少传，多传的视为没有意义的参数，函数会忽略掉，如果少传了，这个参数的位置就是undefined。

4.arguments，函数传入的所有参数，它类似数组，并不是数组。即使函数不定义参数，只要调用的时候传入了参数，仍然可以通过arguments得到。
最常用的是通过arguments.length判断传入的参数的个数。

5.  ...rest参数，只能放在函数参数的最后：
function foo(a, b, ...rest) {}
传入的参数先绑定a,b,剩下的绑定rest，如果正常的参数a,b都没有填满，比如只传了一个参数，此时，rest是一个空数组[].

6.return语句的坑：
使用retrun时，注意不要把return语句的内容换行写在下一行，因为JavaScript会自动在语句的末尾加‘;’，此时下一行的内容就被自动忽略了，如
function aa(){
  retrun  //JavaScript会自动在这里加上;，那么后面的语句还有啥卵用
    {name:'zj'};
}
所以正确的写法：
function aa(){
  return {
    name:'zj'
  }
}

7.不在函数内部定义的变量都具有全局作用域，即定义在了window对象上面，我们平时使用的alert()其实也是window的一个变量。
全局变量可以使用delete删除，而用var声明的变量不能用delete删除。
任何变量（函数也被视为变量），如果没有在当前作用域中找到，就会一直往上查找，最后如果在全局作用域中也查找不到，就会报ReferenceError错误。
let 和 const都具有块级作用域。

8.绑定到对象上的函数成为方法，方法和普通函数没啥区别，但是内容this对象绑定到当前对象上，普通函数的this指向window（严格模式下指向undefined）。
要保证this指向正确，必须用obj.xxx()的形式调用！不能像如下使用：
var fn = xiaoming.age; // 先拿到xiaoming的age函数
fn(); // NaN

可以使用apply方法指定this的指向，它有两个参数：apply(obj,[]);obj是this绑定的对象，数组[]是该方法传递的参数。
也可以使用call方法，与apply方法唯一的区别就是apply接受的是一个数组，call是把参数按顺序传入，用‘,’隔开。
对普通函数调用，我们通常把this绑定为null。
Math.max.appay(mull,[1,2,3]);  // 3
Math.max.call(null,1,2,3);     // 3

如果对象方法sayHello里面定义了一个函数aa(),函数aa里面的this指向window（非严格模式下），并非指向当前对象，一个解决的办法是：
可以在对象的方法sayHello开头定义var that = this; 在函数aa里面使用that表示this。

9.JavaScript的所有对象都是动态的，即使是内置的函数，也可以重新指向新的函数。

