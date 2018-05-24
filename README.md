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
