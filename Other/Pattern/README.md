# 设计模式（Design pattern）

设计模式代表了最佳的实践。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。

设计模式可以分为三大类：创建型模式（Creational Patterns）、结构型模式（Structural Patterns）、行为型模式（Behavioral Patterns）。

## 创建型模式

> 这些设计模式提供了一种在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。

- 工厂模式（Factory Pattern）
- 抽象工厂模式（Abstract Factory Pattern）
- 单例模式（Singleton Pattern）
- 建造者模式（Builder Pattern）
- 原型模式（Prototype Pattern）

## 结构型模式

> 这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。

- 适配器模式（Adapter Pattern）
- 桥接模式（Bridge Pattern）
- 过滤器模式（Filter、Criteria Pattern）
- 组合模式（Composite Pattern）
- 装饰器模式（Decorator Pattern）
- 外观模式（Facade Pattern）
- 享元模式（Flyweight Pattern）
- 代理模式（Proxy Pattern）

## 行为型模式

> 这些设计模式特别关注对象之间的通信。

- 责任链模式（Chain of Responsibility Pattern）
- 命令模式（Command Pattern）
- 解释器模式（Interpreter Pattern）
- 迭代器模式（Iterator Pattern）
- 中介者模式（Mediator Pattern）
- 备忘录模式（Memento Pattern）
- 观察者模式（Observer Pattern）
- 状态模式（State Pattern）
- 空对象模式（Null Object Pattern）
- 策略模式（Strategy Pattern）
- 模板模式（Template Pattern）
- 访问者模式（Visitor Pattern）

## 常用模式分析

[详解 Javascript十大常用设计模式](https://juejin.im/entry/58c280b1da2f600d8725b887)

  * [工厂模式](#工厂模式)
  * [抽象工厂模式](#抽象工厂模式)
  * [单例模式](#单例模式)
  * [策略模式](#策略模式)
  * [观察者模式](#观察者模式)

### 工厂模式

> 工厂模式类似于现实生活中的工厂可以产生大量相似的商品，去做同样的事情，实现同样的效果。

代码实例：
```js
function CreatePerson(name,age,sex) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.sex = sex;
    obj.sayName = function(){
        return this.name;
    }
    return obj;
}
var p1 = new CreatePerson("longen",'28','男');
var p2 = new CreatePerson("tugenhua",'27','女');
```
如上代码：函数CreatePerson能接受三个参数name,age,sex等参数，可以无数次调用这个函数，每次返回都会包含三个属性和一个方法的对象。

### 抽象工厂模式

> 将其成员对象的实列化推迟到子类中，子类可以重写父类接口方法以便创建的时候指定自己的对象类型。

父类只对创建过程中的一般性问题进行处理，这些处理会被子类继承，子类之间是相互独立的，具体的业务逻辑会放在子类中进行编写。

### 单例模式

> 单体模式是一个用来划分命名空间并将一批属性和方法组织在一起的对象，如果它可以被实例化，那么它只能被实例化一次。

单体模式的优点是：

- 可以用来划分命名空间，减少全局变量的数量。
- 使用单体模式可以使代码组织的更为一致，使代码容易阅读和维护。
- 可以被实例化，且实例化一次。

示例代码：
```js
// 单体模式
var Singleton = function(name){
    this.name = name;
    this.instance = null;
};
Singleton.prototype.getName = function(){
    return this.name;
}
// 获取实例对象
function getInstance(name) {
    if(!this.instance) {
        this.instance = new Singleton(name);
    }
    return this.instance;
}
// 测试单体模式的实例
var a = getInstance("aa");
var b = getInstance("bb");

console.log(a === b); // true
console.log(a.getName());// aa
console.log(b.getName());// aa
```

示例代码二：实现弹窗（避免多次显示和隐藏弹窗的dom频繁操作）
```js
// 实现单体模式弹窗
var createWindow = (function(){
    var div;
    return function(){
        if(!div) {
            div = document.createElement("div");
            div.innerHTML = "我是弹窗内容";
            div.style.display = 'none';
            document.body.appendChild(div);
        }
        return div;
    }
})();
document.getElementById("Id").onclick = function(){
    // 点击后先创建一个div元素
    var win = createWindow();
    win.style.display = "block";
}
```

### 策略模式

> 定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。

优点：  
1. 策略模式利用组合，委托等技术和思想，有效的避免很多if条件语句。
2. 策略模式提供了开放-封闭原则，使代码更容易理解和扩展。
3. 策略模式中的代码可以复用。

一：使用策略模式计算奖金；
```js
//代码如下：
var obj = {
        "A": function(salary) {
            return salary * 4;
        },
        "B" : function(salary) {
            return salary * 3;
        },
        "C" : function(salary) {
            return salary * 2;
        } 
};
var calculateBouns =function(level,salary) {
    return obj[level](salary);
};
console.log(calculateBouns('A',10000)); // 40000
```

二：表单效验

定义一套验证规则校验表单。  
代码略。

### 观察者模式

> 发布---订阅模式又叫观察者模式，它定义了对象间的一种一对多的关系，让多个观察者对象同时监听某一个主题对象，当一个对象发生改变时，所有依赖于它的对象都将得到通知。

优点：
1. 支持简单的广播通信，当对象状态发生改变时，会自动通知已经订阅过的对象。
2. 发布者与订阅者耦合性降低，发布者只管发布一条消息出去，它不关心这条消息如何被订阅者使用。


