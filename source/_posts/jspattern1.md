---
title: javascript常用设计模式笔记
date: 2015-3-20 11:30:20
tags: [javascript,coding]
desc: 
---

## **创建型设计模式**
处理对象创建的设计模式，通过某种方式控制对象的创建来避免基本对象创建可能导致设计上的问题或增加设计上的复杂度
### 简单工厂模式

代码复用是面向对象编程的一条准则。通过简单工厂来创建一些对象，可以让这些对象共用一些资源而又私有一些资源。

``` javascript
function createPop(type, content){
	var obj = new Object()
	//	公有方法
	obj.show = function(){}
	if(type === 'xx'){
		//	有差异的部分	
		obj.priFn = function(){}
	}
}
```

### 安全模式类
``` javascript
var Demo = function(){
	if(!(this instanceof Demo)){
		return new Demo()
	}
}

//	安全工厂方法
var Factory = function(type,content) {
	if(this instanceof Factory) {
		//	将实际创建对象工作推迟到子类中。核心类是抽象类。
		var s = this[type](content)
		return s
	} else {
		return new Factory(type, content)
	}
}
Factory.prototype = {
	Java: function(content) {
	}
}
```
### 抽象工厂模式
抽象类定义产品簇，声明必备方法，如果子类中没有重写就会报出错误，定制了类的结构；这也就区别于简单工厂模式创建单一对象，工厂方法模式创建多类对象。

### 建造者模式
将一个复杂对象的构建层与其表示层互相分离，建造者模式不仅可得到创建的结果，也参与了创建的具体过程，对于创建的具体实现细节也参与了干涉，创建的对象是更复杂的复合对象。
在建造者模式我们关心的是对象创建的过程，因此通常将创建对象的类模块化，这样使被创建的类的每一个模块都可以得到灵活的运用与高质量的复用。

**与桥接模式的区别**
桥接模式的特点是将实现层(如元素绑定事件)与抽象层(如修饰页面UI逻辑)解耦分离，使两部分可以独立变化。由此看出桥接模式是对结构之间的解耦。创建者模式和抽象工厂模式主要业务在于创建。通过侨联模式实现的解耦，使实现层与抽象层分开处理，避免需求的改变造成对象内部的修改。体现了面向对象对拓展的开放及对修改的关闭原则。

### 原型模式
原型模式是将原型对象指向创建对象的类，使这些类共享原型对象的方法与属性。
原型对象是一个共享的对象，那么无论是父类的实例对象或者是子类的继承，都是对它的一个指向引用，所以原型对象才会被共享。

### 单例模式
如果想让系统中只存在一个对象，单例模式是最佳方案。

### 惰性单例模式

``` javascript
var LazySingle = (function(){
	var _instance = null;
	function _single() {
		/* 这里定义私有属性和方法 */
		return {
			publicMethod: function() {},
			publicProperty: 'Cash',
		}
	}
	return function(){
		if(!_instance) {
			_instance = _single()
		}
		return _instance
	}

})()
```

## **结构性设计模式**
结构型设计模式关注如何将类或对象组合成更大、更复杂的结构，以简化设计。

### 外观模式
外观模式是对接口方法的外层包装，以供上层代码调用。因此有时外观模式封装的接口方法不需要接口的具体实现，只要按照接口使用规则使用即可。像对绑定事件的封装。

### 装饰者模式和适配器模式
适配器方法是对原有对象适配，添加的方法与原有方法大致相似，但是装饰者提供的方法与原来的方法功能项有一定的区别。再者，使用适配器我们新增的方法是要调用原来方法。装饰者不需要了解原有功能，并且原有方法照样可以原封不动地使用。

### 组合模式
组合模式又称部分-整体模式，将对象组合成树形结构以表示“部分整体”的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。要求是接口的统一。

### 享元模式（Flyweight）
运用共享技术有效地支持大量的细粒度对象，避免对象间拥有相同的内容造成的多余开销。
**应用时要找准内部状态(数据与方法)和外部状态(数据与方法)，这样才能合理地提取分离。**
``` javascript
var Flyweight = function(){
	var created = []
	function create() {
		var dom = document.createElement('div')
		document.getElementById('container').appendChild(dom)
		created.push(dom)
		return dom
	}
	return {
		getDiv(){
			if( created.length < 5){
				return created()
			}else {
				var div = created.shift()
				created.push(div)
				return div
			}
		}
	}
}()
```
## **行为型设计模式**
行为性设计模式用于不同对象之间职责划分或者算法抽象，行为型设计模式不仅仅涉及类和对象，还涉及类或对象之间的交流模式并加以实现。

### 模板方法模式
核心在于方法重用，将核心方法封装在基类中，让子类继承基类的方法，实现基类方法的共享，达到方法共用。通常基类中封装的是不变算法、或者有稳定的调用方式。

### 命令模式
将执行命令封装，解决命令发起者与命令执行者之间的耦合。每一条命令实际上是一个操作。命令的使用者不必了解命令的执行者（命令对象）的命令接口是如何实现、如何接受、如何执行。所有命令存储在命令对象中。

``` javascript
var viewCommend = (function(){
		var Action = {
		create(data) {},
		display(container, data ) {}
	}
	return function execute(msg){
		msg.param = Object.prototype.toString.call(msg.param) === '[object Array]' ? msg.param : [msg.param];
		this[msg.action] && this[msg.action].apply(Action, msg.param)
	}
})()
```

### 访问者模式
访问者模式解决数据与数据操作方法之间的耦合，将数据的操作方法独立于数据，使其可以自由化演变。因此访问者更适合于数据稳定但是操作方法易变的环境。操作环境改变时，可以自由修改操作方法以适应操作环境，而不用修改原数据，实现操作方法的拓展。像lodash中的方法。
``` javascript
var Visitor = (function(){
	return {
		splice: function() {
			var args = Array.prototype.splice.call(arguments, 1)
			return Array.prototype.splice.apply(arguments[0], args)
		},
		push: function() {
			var args = this.splice(arguments, 1)
			var len = arguments[0].length || 0
			arguments[0].length = len + arguments.length - 1
			return Array.prototype.push.apply(arguments[0], args)
		},
		pop: function() {
			return Array.prototype.pop.apply(arguments[0])
		}
	}
})()
```
### 中介者模式（mediator）-媒婆
通过中介者对象封装一系列对象间的交互，使对象间不再互相引用，降低他们之间的耦合。有时中介者对象也可以改变对象之间的交互。中介者模式的订阅者是单向的，只能是消息的订阅者。消息统一由中介者对象发布。所有订阅者对象简介地被中介者管理
``` javascript
var Mediator = (function(){
	var msg = {}
	
	return {
		register(type,fn) {
			if	(msg[type]) {
				msg[type].push(fn)
			} else {
				msg[type] = []
				msg[type].push(fn)
			}
		},
		send(type) {
			if(msg[type]){
				for(var i = 0, len = msg[type].length; i < len; i++) {
					msg[type][i] && msg[type][i]()
				}
			}
		}
	}
})()
```
**与外观模式的区别**
中介者模式是对多个对象交互的封装，且这些对象一般处于同一层面，并且封装的交互在中介者内部，外观模式的封装目的是为了提供更简单易用的接口，而不添加其他功能。

## **架构型设计模式**
结构性框架是一类框架结构，通过提供一些子系统，指定他们的职责，并将他们条理清晰地组织在一起。
### 同步模块模式
将复杂的系统分解为高内聚低耦合的模块，使系统开发变得可控、可维护、可拓展，提高模块的使用率。
而且减少了多人开发中变量、方法名被覆盖的问题。
**排队开发说明代码架构不合理**


**合理的模块划分尤为重要**

在mvp中，实现何种需求(创建哪种页面)的主动权在管理器，要通过创建管理器实现需求。操作管理器成本大。

mvvm  通过视图中自定义属性值为元素绑定javascript行为


