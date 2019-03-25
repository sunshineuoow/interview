## Q: js中的四种继承方式

## A:

### 原型链继承

```javascript
const Parent = function(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
};

Parent.prototype.sayName = function() {
  console.log(this.name);
};

const Child = function(name) {
  this.name = name;
};

Child.prototype = new Parent();

const child = new Child('foo');
const child1 = new Child('bar');

console.log(child instanceof Parent); // true
child.sayName(); // foo
child.colors.push('black');
console.log(child1.colors); // ['red', 'blue', 'green', 'black']
```

原型链继承导致原型上引用属性会在实例间通用，修改一个原型属性会导致所有实例访问到该属性改变。


### 借助构造函数继承

```javascript
const Parent = function(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
};

Parent.prototype.sayName = function() {
  console.log(this.name);
};

const Child = function(name) {
  Parent.call(this, name);
};

const child = new Child('foo');
const child1 = new Child('bar');

child.colors.push('black');
console.log(child1.colors); // ['red', 'blue', 'green']
console.log(child instanceof Parent); // false
child.sayName(); // TypeError: child.sayName is not a function
```

借助构造函数继承可以解决原型上引用属性在实例间通用的问题，但是子类并没有继承父类。


### 组合继承
因此常用的继承方式为将借助构造函数继承和原型链继承集合，成为组合继承。

```javascript
const Parent = function(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
};

Parent.prototype.sayName = function() {
  console.log(this.name);
};

const Child = function(name) {
  Parent.call(this, name);
};

Child.prototype = new Parent();

const child = new Child('foo');
const child1 = new Child('bar');

child.colors.push('black');
console.log(child1.colors); // ['red', 'blue', 'green']
console.log(child instanceof Parent); // true
child.sayName(); // foo
```

解决了引用类型的属性在子类实例中共用问题，同时继承了父类的方法。
唯一不足的是，原型链上拥有多余的父类初始化属性。

### 原型式继承
```javascript
const Parent = function() {
  this.name = 'parent';
};

Parent.prototype.sayName = function() {
  console.log(this.name);
};

const Child = function(name) {
  this.name = name;
};

Child.prototype = Object.create(Parent.prototype);

const child = new Child('foo');

console.log(child instanceof Parent); // true
child.sayName(); // foo
```

### 寄生式继承
寄生式继承是在原型式继承的基础上对原型进行进一步修改。

```javascript
const Parent = function() {
  this.name = 'parent';
};

Parent.prototype.sayName = function() {
  console.log(this.name);
};

const Child = function(name) {
  this.name = name;
};

Child.prototype = Object.create(Parent.prototype, {
  colors: {
    writable: true,
    configurable: true,
    value: ['red', 'blue', 'green']
  }
});

const child = new Child('foo');
const child1 = new Child('bar');

console.log(child instanceof Parent); // true
child.sayName(); // foo
child1.colors.push('black');
console.log(child.colors); // ['red', 'blue', 'green', 'black']
```

寄生式继承依旧无法解决实例共用引用类型的原型属性的问题。


### 寄生组合式继承
因此将寄生式继承和组合继承结合，实现了最终的继承解决方案。

```javascript
const Parent = function(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
};

Parent.prototype.sayName = function() {
  console.log(this.name);
};

const Child = function(name) {
  Parent.call(this, name);
};

Child.prototype = Object.create(Parent.prototype);

const child = new Child('foo');
const child1 = new Child('bar');

console.log(child instanceof Parent); // true
child.sayName(); // foo
child1.colors.push('black');
console.log(child.colors); // ['red', 'blue', 'green']
```


### class继承
ES6中提供了class API，可以比较方便的进行继承。

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
};

class Child extends Parent {
  constructor(name) {
    super(name)
  }
}

const child = new Child('foo');
console.log(child instanceof Parent);
child.sayName();
```