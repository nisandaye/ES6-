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
