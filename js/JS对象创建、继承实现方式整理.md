# JS对象创建、继承实现方式整理

标签（空格分隔）： js

---

##JS对象创建方式

>**1. 对象字面量**
```
var person = {
    name : "jack",
    age : 18,
    friends: ["lida","lier","wangwu"],
    say : function(){
        console.log("Hello,everyone");
    }
}
```
>  **2. new + Object**

```
var person = new Object();
    person.name = "jack";
    person.age = 18;
    person.say = function(){
        console.log("Hello,everyone");
    }
```
以上两种方法在使用同一接口创建多个对象时，会产生大量重复代码，为了解决此问题，工厂模式被开发。

> **3. 工厂模式**

```
function createPerson(name,age,family){
    var o = new Object();
        o.name = name;
        o.age = age;
        o.family = family;
        o.say = function(){
            alert(this.name);
        }
    return o;
}
var person1 =  createPerson("lisi",21,["lida","lier","wangwu"]);
//instanceof 无法判断出它是谁的实例
console.log(person1 instanceof createPerson); //false;
```
> **4. 构造函数模式**
```
function Person(name,age,family){
    this.name = name;
    this.age = age;
    this.family = family;
    this.say = function(){
        alert(this.name);
    }
}
var person1 = new Person("lisi",21,["lida","lier","wangwu"]);
console.log(person1 instanceof Person); //true 可以被instanceof识别
//创建对象实例时，加new与不加new的区别
var person2 = Person("lisi",21,["lida","lier","wangwu"]);
//若不加new，只是对函数进行了调用，此时this指向window，属性和方法会挂到window上，person2为undefined; 通过this 会将属性方法指向新实例对象.
```
对比工厂模式有以下不同之处：

 - 1、没有显式地创建对象
   
 - 2、直接将属性和方法赋给了 this 对象

   
 -  3、没有 return 语句

 
>以此方法调用构造函数步骤 :

>1、创建一个新对象

>2、将构造函数的作用域赋给新对象（将this指向这个新对象）

>3、执行构造函数代码（为这个新对象添加属性）

>4、返回新对象 ( 指针赋给变量person）

构造函数也有其缺陷，每个实例都包含不同的Function实例（构造函数内的方法在做同一件事，但是实例化后却产生了不同的对象，使用不同的方法），因此产生了原型模式。

> **5. 原型模式**
```
function Person() {
}
Person.prototype.name = "lisi";
Person.prototype.age = 21;
Person.prototype.family = ["lida","lier","wangwu"];
Person.prototype.say = function(){
    alert(this.name);
};
console.log(Person.prototype);   //Object{name: 'lisi', age: 21, family: Array[3]}

var person1 = new Person();        //创建一个实例person1
console.log(person1.name);        //lisi

var person2 = new Person();        //创建实例person2
person2.name = "wangwu";
console.log(person2.age);              //21
```
原型模式的好处是所有对象实例共享它的属性和方法（即所谓的共有属性），此外还可以如代码第16,17行那样设置实例自己的属性（方法）（即所谓的私有属性），可以覆盖原型对象上的同名属性（方法）。

>缺点是，当一个实例改变其属性时，其他实例属性相应改变。
```
console.log(person1.family); // ["lida","lier","wangwu"];
person2.family.push("xioaxiao");
console.log(person1.family); //["lida", "lier", "wangwu", "xiaoxiao"];

```

> **6. 混合模式**
```
function Person(name,age,family){
    this.name = name;
    this.age = age;
    this.family = family;
}
Person.prototype = {
    constructor: Person,  
    say: function(){
        alert(this.name);
    }
}
var person1 = new Person("lisi",21,["lida","lier","wangwu"]);
console.log(person1);
var person2 = new Person("wangwu",21,["lida","lier","lisi"]);
console.log(person2);
```
可以看出，混合模式共享着对相同方法的引用，又保证了每个实例有自己的私有属性。最大限度的节省了内存


----------
##JS继承实现方式

> **1. 原型链继承（原型对象指向另一个类型的实例）**
```
function Person(name,age){
    this.name = name;
    this.age = age;
}
function Boy(){
    this.age = "男"；
}
Boy.prototype = new Person();
```
`重点`：让新实例的原型等于父类的实例。

`特点`：1、实例可继承的属性有：实例的构造函数的属性，父类构造函数属性，父类原型的属性。（新实例不会继承父类实例的属性！）

`缺点`：1、新实例无法向父类构造函数传参。
　　　2、继承单一。
　　　3、所有新实例都会共享父类实例的属性。（原型上的属性是共享的，一个实例修改了原型属性，另一个实例的原型属性也会被修改！）
　　　
> **2. 借用构造函数继承（使用call，apply方法）**
```
function Person(name,age){
    this.name = name;
    this.age = age;
}
function Boy(){
    Person.call(this,"xiaoxiao",19);
    this.sex = "男";
}
```
`重点`：用.call()和.apply()将父类构造函数引入子类函数（在子类函数中做了父类函数的自执行（复制））

`特点` : 1、只继承了父类构造函数的属性，没有继承父类原型的属性。

　　　2、可以继承多个构造函数属性（call多个）。

　　　3、在子实例中可向父实例传参。

`缺点`：1、只能继承父类构造函数的属性。

　　　2、无法实现构造函数的复用。（每次用每次都要重新调用）

　　　3、每个新实例都有父类构造函数的副本，臃肿。

> **3. 组合继承（原型继承配合构造函数实现继承）**
```
Person.prototype.say = function(){
    console.log("我是"+this.name);
}
function Person(name,age){
    this.name = name;
    this.age = age;
}
function Boy(){
    Person.call(this,"xiaoxiao",19);
    this.sex = "男";
}
Boy.prototype = new Person();
```
`重点`：结合了两种模式的优点，传参和复用

`特点`：1、可以继承父类原型上的属性，可以传参，可复用。

　　　2、每个新实例引入的构造函数属性是私有的。

`缺点`：调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。

> **4. 原型继承（子类的prototype == 父类的prototype）**
```
Person.prototype.name = "张三";
Person.prototype.age = "18"; 
function Person(){
}
function Boy(){
}
Boy.prototype = Person.prototype;

//es5提供create方法，用a来创造b，b.__proto__ === a
var a = { rep: 'apple' }
var b = Object.create(a)
console.log(b)  // {}
console.log(b.rep) // {rep: "apple"}
console.log(b.__proto__ === a) // true
```
`重点`：用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。object.create()就是这个原理。

`特点`：类似于复制一个对象，用函数来包装。

`缺点`：1、所有实例都会继承原型上的属性。

　　　2、无法实现复用。（新实例属性都是后面添加的）
> **5. 改进式原型继承（圣杯模式）**
```
function inherit(target,orign){
    function F(){};
    F.prototype = orign.prototype;
    target.prototype = new F();
    target.prototype.constructor = target;
    target.prototype.uber = orign.prototype;
}
Person.prototype.name = "张三";
Person.prototype.age = "18"; 
Person.prototype.say = function(){
    console.log("我是"+this.name);
}
function Person(){
}
function Boy(){
}
inherit(Boy,Person);
var boy = new Boy();

```