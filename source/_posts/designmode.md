---
title: 设计模式
permalink: /pages/c68878/
categories:
  - 设计模式
summary: 前端常用设计模式
tags:
  - 设计模式专题
abbrlink: 41682
date: 2020-12-01 10:03:12
---

# 设计模式

## 1.设计模式的理解

### 1.1什么是设计模式

设计模式是解决问题的一种思想，和语言无关，面向对象软件设计的工程中，针对特定的问题的一种解决方案。也就是说，设计模式就是符合魔种场景下的某个问题的解决方案，通过设计模式可以增加代码的可用性，可扩展型，可维护性，达到代码的高内聚、低耦合。

### 1.2设计模式的五大设计原则

- 单一职责：一个程序只需要做好一件事，若功能复杂则拆分，保证程序功能的独立
- 开放封闭原则：开放扩展，封闭修改，若增加需求，可以扩展新代码，而不是修改源代码。
- 里氏置换原则：子类能覆盖父类，父类能出现的地方子类也能出现
- 接口独立原则：保持接口的单一独立
- 依赖倒置原则：面试接口编程，依赖于抽象而不依赖于具体

### 1.3设计模式

#### 1.3.1工厂模式

不暴露创建对象的具体逻辑，而是将实现逻辑进行封装，这就是工厂，工厂模式又叫静态工厂模式，由一个工厂对象决定创建某一个类的实例。缺点是每增加一个类实例就需要增加一个工厂类，这样会增加内存压力和系统的复杂度。

例子：

一个服装厂可以生产不同类型的衣服，我们可以通过一个工厂方法类来模拟产出

```js
class DownJacket {
  production(){
    console.log('生产羽绒服')
  }
}
class Underwear{
  production(){
    console.log('生产内衣')
  }
}
class TShirt{
  production(){
    console.log('生产t恤')
  }
}
// 工厂类
class clothingFactory {
  constructor(){
    this.downJacket = DownJacket
    this.underwear = Underwear
    this.t_shirt = TShirt
  }
  getFactory(clothingType){
    const _production = new this[clothingType]
    return _production.production()
  }
}
const clothing = new clothingFactory()
clothing.getFactory('t_shirt')// 生产t恤
```

#### 1.3.2.抽象工厂

通过类的抽象使得业务适用于一个产品类簇的创建，而不是负责某一个类产品的实例的创建，抽象工厂相当于普通工厂的升级版，普通工厂

例子：基于上面的列子，模拟抽象类，同时约束继承子类的方法实现，最后再通过工厂函数返回指定类簇。

```js
/* 抽象类
js中abstract是个保留字，实现抽象类只能通过new.target进行验证，
防止抽象类被直接实例，另外如果子类没有覆盖指定方法，则抛出错误
*/
class ProductionFlow {
  constructor(){
    if(new.target === ProductionFlow){
      throw new Error('抽象类不能被实例')
    }
  }
  production(){
    throw new Error('production要被重写')
  }
  materials(){
    throw new Error('materials要被重写')
  }
}
class DownJacket extends ProductionFlow{
  production(){
    console.log(`材料:${this.materials()},生产羽绒服`)
  }
  materials(){
    return '鸭毛'
  }
}
class Underwear extends ProductionFlow{
  production(){
    console.log(`材料:${this.materials()},生产内衣`)
  }
  materials(){
    return '丝光棉'
  }
}
class TShirt extends ProductionFlow{
  production(){
    console.log(`材料:${this.materials()},生产t恤`)
  }
  materials(){
    return '纯棉'
  }
}

function getAbstractProductionFactory(clothingType){
  const clothingObj = {
    downJacket:DownJacket,
    underwear:Underwear,
    t_shirt:TShirt,
  }
  if(clothingObj[clothingType]){
    return clothingObj[clothingType]
  }
  throw new Error(`工厂暂时不支持生产这个${clothingType}类型的服装`)
}

const downJacketClass = getAbstractProductionFactory('downJacket')
const underwearClass = getAbstractProductionFactory('underwear')

const downJacket = new downJacketClass()
const underwear = new underwearClass()
downJacket.production() // 材料:鸭毛,生产羽绒服
```

#### 1.3.3.单例模式

保证一个类只能被实例一次，每次获取的时候，如果该类已经创建过实例则直接返回该实例，否则创建一个实例保存并返回。核心是保证创建一个唯一的对象。缺点是违反了单一职责，一个类应该只关心内部逻辑，而不用去关心外部实现。

场景应用：常见的登陆弹窗，要么显示要么隐藏，不可能同时出现两个弹窗

```js
class LoginFrame {
    static instance = null
    constructor(state){
        this.state = state
    }
    show(){
        if(this.state === 'show'){
            console.log('登录框已显示')
            return
        }
        this.state = 'show'
        console.log('登录框展示成功')
    }
    hide(){
        if(this.state === 'hide'){
            console.log('登录框已隐藏')
            return
        }
        this.state = 'hide'
        console.log('登录框隐藏成功')
    }
    // 通过静态方法获取静态属性instance上是否存在实例，如果没有创建一个并返回，反之直接返回已有的实例
    static getInstance(state){
        if(!this.instance){
            this.instance = new LoginFrame(state)
        }
        return this.instance
    }
}
const p1 = LoginFrame.getInstance('show')
const p2 = LoginFrame.getInstance('hide')
console.log(p1 === p2) // true
```

#### 1.3.4.代理模式

代理模式的关键在于当客户不方便直接访问一个对象或不满足需要的时候，提供一个代理对象来控制对这个对象的访问，客户实际上访问的是代理对象，代理对象对请求做出一些处理之后，再把请求转交给本体对象。

例子：员工通过秘书去找领导处理事务，

```js
// 员工
class Staff {
  constructor(affairType){
    this.affairType = affairType
  }
  applyFor(target){
    target.receiveApplyFor(this.affairType)
  }
}
// 秘书
class Secretary {
  constructor(){
    this.leader = new Leader()
  }
  receiveApplyFor(affair){
    this.leader.receiveApplyFor(affair)
  }
}
//领导
class Leader {
  receiveApplyFor(affair){
    console.log(`批准:${affair}`)
  }
}

const staff = new Staff('升职加薪')
staff.applyFor(new Secretary()) // 批准:升职加薪
```

#### 发布订阅模式和观察者模式的区别

两者所需要的角色数量不一样，观察者模式需要两个角色(观察者和消费者)便可成型，发布订阅需要三个角色(发布者，订阅者，第三方发布订阅中心)。不同应用场景的设计模式实现方法不尽相同。

<img src="../images/image-20220601141406185.png" alt="image-20220601141406185" style="zoom: 67%;" />

#### 1.3.5.发布订阅模式

定义：基于一个事件（主题）通道，希望接收通知的对象 Subscriber通过自定义事件订阅主题，被激活事件的对象 Publisher 通过发布主题事件的方式通知各个订阅该主题的 Subscriber 对象。

> 对象间一种一对多的依赖关系，当目标对象指定的动作发生改变时，订阅该动作的依赖对象会收到相应的通知。订阅者在发布订阅中心EventSubPub订阅/注册事件和回调函数，发布者通过中心发布/触发某一事件，会调用回调函数通知订阅/注册该事件的人

```js
// 用ES6的方式来写
class EventSubPub {
    constructor() {
        this.events = {};// 定义一个事件对象，存储订阅事件和回调函数(方法)
        // 如： {click : [handler1, handler2, ...], sleep : [handler1, handler2]...} 每一个事件有各种方法,形成方法数组
    }
    // 订阅事件/注册事件以及事件的回调函数
    on(eventName, handler) {// 参数为参数名和回调函数(就是触发该事件会执行某些方法)，这些方法可以是多个
        if (!this.events[eventName]) {// 如果本来没有注册事件
            this.events[eventName] = [handler]// 则创建数组并添加第一个回调函数
        } else {
            this.events[eventName].push(handler);// 如果本来注册过事件，则存入接下来的回调函数
            // this.event[eventName] 这是一个数组 是上面所创建的
        }
    }
    // 发布事件/触发事件回调 参数：事件名和事件回调函数参数
    emit(eventName, args) {// 需要传入事件名和参数
        if (!this.events[eventName]) {// 事件未注册 直接抛出错误
            return new Error('该事件未注册')
        }
        // 遍历对应事件里数组中的方法(回调函数) 并 执行所有回调函数(需要传入参数)
        this.events[eventName].forEach(handler => { handler(args) })
    }
    // 事件移除 参数 事件名和回调函数
    off(eventName, handler) {
        if(this.events[eventName]) {// 判断是否注册过该事件
            if(!handler) {// 判断是否传入了事件对应的回调函数
              delete this.events[eventName];// 没有传入直接删除事件
            } else {
                let index = this.events[eventName].indexOf(handler);// 在数组中找到第一个出现该回调函数handler的下标
                this.events[eventName].splice(index, 1);// 
                // slice 可以传入两个参数 若只传入一个 则返回该索引到数组末尾所有元素，若传入两个(a,b) 则切割索引a到b 不包括b的数组元素(左闭右开)
                // splice可以传入三个参数，第一个为开始位置，第二个为删除个数，第三个(还可以任意多个)为插入元素
            }
        }
    }
}
// 测试
const events = new EventSubPub()
function fn1() {
    console.log("fn1");
}
function fn2() {
    console.log("fn2");
}
function fn3() {
    console.log("fn3 ");
}
events.on("event1", fn1)
events.on("event1", fn2)
events.on("event2", fn2)
events.on("event3", fn3)// 注册事件和回调函数
events.emit("event1")// fn1 fn2
events.off("event1", fn1);// 关闭订阅 移除事件及对应的回调函数
events.emit("event1")// fn2
events.emit("event2")// fn2
events.emit("event3")// fn3
events.off("event2")// 关闭、移除整个事件event2
events.off("event3")// 关闭、移除整个事件event3
events.emit("event2")// 无
events.emit("event3")// 无
```

#### 1.3.6.观察者模式

定义：当对象之间存在一对多的依赖关系时，其中一个对象的状态发生改变，所有依赖它的对象都会收到通知，这就是观察者模式。

> 对象间一种一对多的依赖关系，当目标对象 Subject 的状态发生改变时，所有依赖它的对象 Observer 都会得到通知。被观察者Subject将观察者添加到自己的观察者列表，

```js
// Subject 被观察对象
function Subject() {
    this.observers = [];// 创建数组用于存储观察者
}
Subject.prototype = {// 在被观察者的原型上添加方法
    add(observer) {  // 添加观察者
        this.observers.push(observer);
    },
    notify() {  // 添加通知方法
        var observers = this.observers;// 获取所用添加进来的观察者
        for (var i = 0; i < observers.length; i++) {// 遍历调用观察者的更新方法 通知观察者们更新
            observers[i].update();
        }
    },
    remove(observer) {  // 删除观察者
        var observers = this.observers;
        for (var i = 0; i < observers.length; i++) {// 遍历所有观察者
            if (observers[i] === observer) {// 如果当前观察者和传进来的一样
                observers.splice(i, 1);// 则删除传进来的观察者
            }
        }
    },
}
// Observer 观察者对象
function Observer(name) {
    this.name = name;
}
Observer.prototype = {// 在观察者的原型上添加更新方法
    update() {  // 更新
        console.log('my name is ' + this.name);
    }
}
var sub = new Subject();
var obs1 = new Observer('guang1');
var obs2 = new Observer('guang2');
sub.add(obs1);
sub.add(obs2);
// 通知所有观察者
sub.notify();  //my name is guang1 my name is guang2
```

#### 1.3.7装饰器模式

装饰器模式能够在不更改源代码自身的情况下，对其进行职责添加

例子：在编写飞机大战的游戏中，飞机对象的攻击方式只有普通子弹攻击，如何在不更改原代码的情况下，为它其他的攻击方式，如激光武器，导弹武器？

```js
class Aircraft {
    ordinary(){
        console.log('发射普通子弹')
    }
}
class AircraftDecorator {
    constructor(aircraft){
        this.aircraft = aircraft
    }
    laser(){
        console.log('发射激光')
    }
    guidedMissile(){
        console.log('发射导弹')
    }
    ordinary(){
        this.aircraft.ordinary()
    }
}
const aircraft = new Aircraft()
const aircraftDecorator = new AircraftDecorator(aircraft)
aircraftDecorator.ordinary() // 发射普通子弹
aircraftDecorator.laser() // 发射激光
aircraftDecorator.guidedMissile() // 发射导弹
// 可以看到在不更改源代码的情况下对它进行了装饰扩展
```

