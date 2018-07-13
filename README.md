# ES6-
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
上面代码定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。也就是说，
ES5 的构造函数Point，对应 ES6 的Point类的构造方法。
//牢记
constructor方法默认返回实例对象（即this），完全可以指定返回另外一个对象。

--------ES6在继承的过程中为什么要用super这个函数？
super关键字，它指代父类的实例（即父类的this对象）。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。

----------构造函数在new的过程中发生了什么？
function A(){this.name='aaaa'}
var o = new Object();
o.__proto__ = A.prototype;//这里还记得之那个function里面的默认的属性么?
A.call(o)//由于这里this是指向o.
把这个o返回给a;//完成var a = new A()的过程.
简单来说就是先new一个空对象，然后将控对象绑定到构造函数中的this，再调用构造函数，同时将__proto__指针指向构造函数的prototype属性，也就是构造函数的原型对象。


JavaScript----什么是纯函数

定义
简单来说，一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。这么说肯定比较抽象，我们把它掰开来看：

函数的返回结果只依赖于它的参数。
函数执行过程里面没有副作用。
const a = 1
const foo = (b) => a + b
foo(2) // => 3

foo 函数不是一个纯函数，因为它返回的结果依赖于外部变量 a，我们在不知道 a 的值的情况下，并不能保证 foo(2) 的返回值是 3。虽然 foo 函数的代码实现并没有变化，传入的参数也没有变化，但它的返回值却是不可预料的，现在 foo(2) 是 3，可能过了一会就是 4 了，因为 a 可能发生了变化变成了 2。

const a = 1
const foo = (x, b) => x + b
foo(1, 2) // => 3

现在 foo 的返回结果只依赖于它的参数 x 和 b，foo(1, 2) 永远是 3。今天是 3，明天也是 3，在服务器跑是 3，在客户端跑也 3，不管你外部发生了什么变化，foo(1, 2) 永远是 3。只要 foo 代码不改变，你传入的参数是确定的，那么 foo(1, 2) 的值永远是可预料的。

这就是纯函数的第一个条件：一个函数的返回结果只依赖于它的参数。

函数执行过程没有副作用 
一个函数执行过程对产生了外部可观察的变化那么就说这个函数是有副作用的。

我们修改一下 foo：

const a = 1
const foo = (obj, b) => {
  return obj.x + b
}
const counter = { x: 1 }
foo(counter, 2) // => 3
counter.x // => 1

我们把原来的 x 换成了 obj，我现在可以往里面传一个对象进行计算，计算的过程里面并不会对传入的对象进行修改，计算前后的 counter 不会发生任何变化，计算前是 1，计算后也是 1，它现在是纯的。但是我再稍微修改一下它：

const a = 1
const foo = (obj, b) => {
  obj.x = 2
  return obj.x + b
}
const counter = { x: 1 }
foo(counter, 2) // => 4
counter.x // => 2

现在情况发生了变化，我在 foo 内部加了一句 obj.x = 2，计算前 counter.x 是 1，但是计算以后 counter.x 是 2。foo 函数的执行对外部的 counter 产生了影响，它产生了副作用，因为它修改了外部传进来的对象，现在它是不纯的。

但是你在函数内部构建的变量，然后进行数据的修改不是副作用：

const foo = (b) => {
  const obj = { x: 1 }
  obj.x = 2
  return obj.x + b
}

虽然 foo 函数内部修改了 obj，但是 obj 是内部变量，外部程序根本观察不到，修改 obj 并不会产生外部可观察的变化，这个函数是没有副作用的，因此它是一个纯函数。

除了修改外部的变量，一个函数在执行过程中还有很多方式产生外部可观察的变化，比如说调用 DOM API 修改页面，或者你发送了 Ajax 请求，还有调用 window.reload 刷新浏览器，甚至是 console.log 往控制台打印数据也是副作用。

纯函数很严格，也就是说你几乎除了计算数据以外什么都不能干，计算的时候还不能依赖除了函数参数以外的数据。

总结
一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。

为什么要煞费苦心地构建纯函数？因为纯函数非常“靠谱”，执行一个纯函数你不用担心它会干什么坏事，它不会产生不可预料的行为，也不会对外部产生影响。不管何时何地，你给它什么它就会乖乖地吐出什么。如果你的应用程序大多数函数都是由纯函数组成，那么你的程序测试、调试起来会非常方便。

---------'||'和'&&'运算符的妙用（$$运算符优先级高于||）
&&遇上假就停止，并返回这个假，相反遇到真就继续执行直到拿最后一个
||遇上真就停止，并返回这个真，相反遇到假就继续执行直到拿最后一个


-------require().default
babel6+ 用require引 export default 导出的组件  还要加个require().default
