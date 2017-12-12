---
title: js继承
categories:
  - javascript设计模式
tags:
  - js继承
date: 2017-11-05 21:49:20
---

> 每个类都有三个部分组成

* 构造函数内的，供实例对象复制用的
* 构造函数外的，直接通过点语法添加的，这是供类使用的，实例对象访问不到
* 类的原型中的，实例化对象可以通过其原型链间接的访问到，也是为供所有实例化对象所共有

<!--- more -->

### 子类的原型继承---类式继承

> 类式继承需要将第一个类的实例赋值给第二个类的原型

```javascript
// 父类
function SuperClass() {
    this.SuperValue = true;
}
// 为父类原型添加方法
SuperClass.prototype.getSuperValue = function() {
    // alert(1)
    return this.SuperValue;
}
// console.dir(SuperClass);
var supe = new SuperClass();
// console.dir(supe);
// supe.getSuperValue();
// 声明子类
function SubClass() {
    this.subValue = false;
}
// 子类继承父类
SubClass.prototype = new SuperClass();
// 为子类添加共有方法
SubClass.prototype.getSubValue = function() {
    // alert(2);
    return this.subValue;
}
// console.dir(SubClass);
// 子类继承父类SuperValue属性，
// 访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着__proto__这条链向上找，这就是原型链。
var sub = new SubClass();
// sub.getSuperValue();
// sub.getSubValue();
// console.log(sub.SuperValue);
// console.log(sub.subValue);
// console.dir(sub);

```

> 为什么这样做？？？

类的原型对象的作用就是为类的原型添加共有方法，但是累不能直接访问这些属性和方法，必须通过prototype来访问。

我们在实例化一个父类的时候，新创建的对象复制了父类的构造函数内的属性和方法并且将隐氏原型_proto_指向父类的原型对象，这样就拥有了父类的原型对象上的方法和属性。

如果将新创建的对象赋值给子类的原型，那么子类的原型就可以访问到父类的原型上的属性和方法，同样也可以访问从父类构造函数中复制的属性和方法，同样子类也可以访问父类原型上的属性和方法与父类构造函数中复制的属性和方法。

> 检测继承关系

instanceof通过判断对象的_proto_与构造函数的prototype，来确定这个对象是否是某个类的实例，而不关心对象与类的自身结构

```javascript
console.log(sub instanceof SubClass);         //true
console.log(sub instanceof SuperClass);       //true
console.log(SubClass instanceof SuperClass);  //false

// why false??
// instanceof 是判断前面的对象是否是后面类的实例，他并不表示两者的继承，
console.log(SubClass.prototype instanceof SuperClass); //true

```

> 类式继承的缺点

* 由于子类通过其原型Prototype对父类实例化，继承了父类，所以说父类中的共有属性要是引用类型，就会在子类中被所有属性共有，因此一个子类的实例更改子类原型从父类构造函数中继承来的共有属性就会直接影响到其他子类

```javascript
function SuperClass() {
    this.books = ['javascript', 'html', 'css']
    this.a = 12
}
function SubClass() {}
SubClass.prototype = new SuperClass()
var instance1 = new SubClass()
var instance2 = new SubClass()
console.log(instance2.books)
instance1.books.push('设计模式')
instance1.a = 34
console.log(instance2.books)
console.log(instance2.a)
```
* 由于子类实现的继承是靠原型prototype对付类的实例化实现的，因此在创建父类的时候，是无法向父类传递参数的，因而在实例化父类的时候也无法对父类构造函数内的属性进行初始化。

### 创建即继承---构造函数继承

```javascript
// 构造函数继承
// 声明父类
function SuperClass(id) {
    this.books = ['javascript', 'html', 'css'];
    this.id = id;
}
// 父类声明原型
SuperClass.prototype.showBooks = function() {
    console.log(this.books);
}
// 声明子类
function SubClass(id) {
    // 继承父类
    SuperClass.call(this, id);
}
// 创建第一个子类的实例
var instance1 = new SubClass(11);
// 创建第二个子类的实例
var instance2 = new SubClass(22);
// 验证
instance1.books.push('设计模式');
console.log(instance1.books);
console.log(instance2.books);
console.log(instance1.id);
console.log(instance2.id);

```
> 继承原理

* 由于call这个方法可以更改函数的作用域，因此在子类中，对superClass调用这个方法就是将子类中的变量在父类中执行一遍。

* 由于父类中是=给this绑定属性的，因此子类自然也就继承了父类的共有属性

* 由于这种类型的继承没有涉及原型prototype，所以父类的原型方法自然不会被子类继承

> 每个函数都包含两个非继承而来的方法：call()方法和apply()方法。

* 相同点：这两个方法的作用是一样的。

都是在特定的作用域中调用函数，等于设置函数体内this对象的值，以扩充函数赖以运行的作用域。

一般来说，this总是指向调用某个方法的对象，但是使用call()和apply()方法时，就会改变this的指向。

* 不同点：接收参数的方式不同。

apply()方法 接收两个参数，一个是函数运行的作用域（this），另一个是参数数组。

### 将有点为我所用---组合继承 

#### 总结前面两种特点

> 类式继承式通过子类的原型prototype对父类实例化来实现的。

> 构造函数式继承式通过在子类的构造函数作用环境中执行一次父类的构造函数来实现。

```javascript
// 组合式继承
// 声明父类

function SuperClass(name) {
    this.name = name;
    this,boob = ['html', 'css', 'javascript'];
}
// 父类原型共有方法
SuperClass.prototype.getName = function() {
    console.log(this.name);
}
// 声明子类
function SubClass(name, time) {
    // 构造函数式继承父类name 属性
    SuperClass.call(this, name);
    // 子类中新增共有属性
    this.time = time;
}
// 类式继承  子类原型继承父类
SubClass.prototype = new SuperClass();
// 子类原型方法
SubClass.prototype.getTime = function() {
    console.log(this.time);
}
// 测试
var instance1 = new SubClass('js book', 2017);
instance1.books.push('设计模式');
console.log(instance1.books);
instance1.getName();
instance1.getTime();
var instance2 = new SubClass('css book', 2018);
console.log(instance2.books);
instance2.getName();
instance2.getTime();

```
> 结合了类式继承和构造函数继承两种方式的优点

> 不足之处

在使用构造函数继承时执行了一遍父类的构造函数，而在实现子类原型的类式继承式又调用了一遍父类的构造函数。因此父类构造函数调用了两遍。

### 洁净的继承者---原型式继承

```javascript
function inheritObject(o) {
    // 声明一个过渡函数对象
    function F() {}
    F.prototype = o;
    return new F();
}
```
> 和类式继承有点像？？

对，是对类式继承的一个封装，过渡对象相当于类式继承中的子类，只不过在原型式继承中作为一个过渡对象出现的，目的是为了创建要返回的新的实例化对象。

> ??是不是类式继承中的问题在这里也会出现？？？

是的，！！！，不过这种方式由于F过渡类的构造函数中无内容，所以开销比较小，使用起来也比较方便，

```javascript
var book = {
    name: 'js book',
    alikeBook: ['css book', 'html book']
}
var newBook = inheritObject(book)
newBook.name = 'ajax book'
newBook.alikeBook.push('xml books')

var otherBook = inheritObject(book)
otherBook.name = 'flash book'
otherBook.alikeBook.push('as books')
console.log(newBook.name)
console.log(newBook.alikeBook)

console.log(otherBook.name)
console.log(otherBook.alikeBook)

console.log(book.name)
console.log(book.alikeBook)
```

和类式继承一样，父类对象book中的值类型的属性被复制，引用类型的属性被共用。

### 如虎添翼---寄生式继承


```javascript
// 寄生式继承
// 声明基对象
var book = {
    name: 'js book',
    alikeBook: ['css book', 'html book']
}
function inheritObject(o) {
    // 声明一个过渡函数对象
    function F() {}
    F.prototype = o;
    return new F();
}
// var c = new inheritObject(book);
// console.log(c)
function createBook(obj) {
    // 通过原型继承方式创建新对象
    var o = new inheritObject(obj)
    // 拓展新对象
    o.getName = function() {
        console.log(name)
    }
    return o;
}

var a1  = createBook(book)
var a2  = createBook(book)
a1.alikeBook.push('设计模式')
console.log(a1.alikeBook)
console.log(a2.alikeBook)
```

其实寄生式继承就是对原型继承的第二次封装，并且在第二次封装的过程中对继承的对象进行了扩展，这样新创建的对象不仅仅有父类中的属性和方法而且还添加了新的属性和方法。

### 终极继承者---寄生组合式继承

> 之前学习了组合继承，就是将类式继承通构造函数继承组合，这种方式存在的问题就是子类不是父类的实例，而子类的原型式父类的实例，因此才有了寄生组合式继承。是寄生式继承与构造函数继承的组合，但是这里寄生式继承有点特殊，这里他处理的不是对象，而是类的原型。

```javascript
function inheritObject(o) {
    // 声明一个过渡函数对象
    function F() {}
    F.prototype = o;
    return new F();
}

function inheritPrototype(SubClass, SuperClass) {
    // 复制一份父类的原型副本保存在变量中
    var p = inheritObject(SuperClass.prototype);
    // 修正因为重写子原型导致子类的constructor属性被修改
    p.constructor = SubClass;
    // 设置子类的原型
    SubClass.prototype = p;
}

```

> 在寄生组合式继承中，通过构造函数继承的属性和方法是没有问题的吗，这里主要探究通过寄生式继承重新继承父类的原型，我们需要继承的仅仅是父类的原型，不在需要调用父类的构造函数，换句话说，在构造函数继承中，我们已经调用了父类的构造函数，因此我们需要的就是父类的原型对象的一个副本，而这个副本我们我们通过原型继承便可得到，但是直接赋值个子类会有问题，因为对父类原型对象复制得到的复制对象p中的constroctor指向的不是SubClass子类对象，因此在寄生式继承中要对复制对象p做一次增强，修复其constroctor属性指向不正确问题，最后将得到的复制对象p赋值给子类的原型，这样子类的原型就继承了父类的原型并且没有执行父类的构造函数。

```javascript
// 寄生式继承  继承原型
// 传递参数  SuperClass  父了
// 传递参数  SubClass    子类

function inheritObject(o) {
    // 声明一个过渡函数对象
    function F() {}
    F.prototype = o;
    return new F();
}
function inheritPrototype(SubClass, SuperClass) {
    // 复制一份父类的原型副本保存在变量中
    var p = inheritObject(SuperClass.prototype);
    // 修正因为重写子原型导致子类的constructor属性被修改
    p.constructor = SubClass;
    // 设置子类的原型
    SubClass.prototype = p;
} 
function SuperClass(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green']
}
SuperClass.prototype.getName = function() {
    console.log(this.name)
}
function SubClass(name, time) {
    // 构造函数继承
    SuperClass.call(this, name)
    this.time = time
}
// 寄生式继承父类原型
inheritPrototype(SubClass, SuperClass)
SubClass.prototype.getTime = function() {
    console.log(this.time)
}
// 创建两个测试方法
var instance1 = new SubClass('js book', 2017);
var instance2 = new SubClass('css book', 2018);

instance1.colors.push('black')
console.log(instance1.colors)
console.log(instance2.colors)
instance2.getName();
instance2.getTime();

```

> 这中继承最大的改变就是对子类原型的处理，被赋予父类原型的一个应用，这是一个对象，因此有一点要注意，就是子类在想添加原型方法必须通过prototype对象，通过点语法的形式一个个添加方法了，否则直接赋予对象就会覆盖掉从父类原型继承的对象了。

