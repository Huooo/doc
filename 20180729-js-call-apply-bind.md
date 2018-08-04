经济基础决定上层建筑。

---

## 我来了
  - 学习，记录，备忘。
  - 感谢参考过的所有资料的作者。

---


## call

> The call() method calls a function with a given this value and arguments provided individually.
> A different this object can be assigned when calling an existing function. this refers to the current object, the calling object. With call, you can write a method once and then inherit it in another object, without having to rewrite the method for the new object.

```javascript
function.call(thisArg, arg1, arg2, ...)
```

- thisArg: 可选。`function` 中的 `this` 值。在非严格模式下，thisArg 为 `null` 和 `undefined` 会被全局对象替换，且原始值会转化为对象。
- arg1, arg2, ... : 可选。一个个参数。

返回使用指定的 this 和 arguments 调用函数的结果。

```javascript
function fun() {
  return this;
}

fun.call(null) === fun.call(window); // true
fun.call(undefined) === fun.call(window); // true

fun.call(1); // Number{...}
fun.call('hello'); // String{...}
```

**Using call to chain constructors for an object**

> You can use call to chain constructors for an object, similar to Java. 

这里有前辈译为：在一个子构造函数中，你可以通过调用父构造函数的 call 方法来实现继承，类似于 Java 中的写法。

```javascript
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}
var cheese = new Food('feta', 5);
var fun = new Toy('robot', 40);

// 在构造函数 Product 中给 this 定义属性 name 和 price 并赋值。
// 在构造函数 Food/Toy 中借用 call() 来给自己的 this 定义 name 和 price 并且赋值。
// 在实例化构造函数时，this 指向实例出的对象，
// 这些对象不仅有自己的构造函数定义的属性，还有 name 和 price 属性。
console.log(cheese.name); // feta 
console.log(fun.price); // 40
```

**Using call to invoke an anonymous function**

下面这个列子仅仅是为了展示匿名函数使用 `call()` 来指定 `this` 的值，并不是一定要用这种方法。循环中，使用 `call()` 来指定匿名函数的 `this` 的值为数组中的每一项 `animals[i]`，并且将下标索引 `i` 作为参数传入以供使用。

```javascript
var animals = [
  { species: 'Lion', name: 'King' },
  { species: 'Whale', name: 'Fail' }
];

for (var i = 0; i < animals.length; i++) {
  (function(i) {
    this.print = function() {
      console.log('#' + i + ' ' + this.species
                  + ': ' + this.name);
    }
    this.print();
  }).call(animals[i], i);
}
// #0 Lion: King
// #1 Whale: Fail
```

**Using call to invoke a function and specifying the context for 'this'**

这应该是比较常用到 `call()` 的一种情况。使用 `call()` 来调用一个 `function` 并改变这个 `function` 中 `this` 的上下文。

```javascript
function greet() {
  var reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
  console.log(reply);
}

var obj = {
  animal: 'cats', sleepDuration: '12 and 16 hours'
};

// 这个时候，this 值指向对象 obj
greet.call(obj);  // cats typically sleep between 12 and 16 hours
```

**Using call to invoke a function and without specifying the first argument**

使用 `call()` 来调用一个 `function` ，但是不传入第一个参数。这个时候 this 指向全局对象。

```javascript
var sData = 'Wisen';
            
function display(){
  console.log('sData value is %s ', this.sData);
}

// 这个时候，this 指向 window
display.call();  // sData value is Wisen 
```

**将类数组转为数组**

```javascript
function convertArray() {
  return Array.prototype.slice.call(arguments)
}

convertArray(1, 2, 3, 4, 5) // [1, 2, 3, 4, 5]
```

---

## apply
> The apply() method calls a function with a given this value, and arguments provided as an array (or an array-like object).
> You can assign a different this object when calling an existing function. this refers to the current object, the calling object. With apply, you can write a method once and then inherit it in another object, without having to rewrite the method for the new object.

```javascript
function.apply(thisArg, [argsArray])
```

- thisArg: 可选。`function` 中的 `this` 值。在非严格模式下，thisArg 为 `null` 和 `undefined` 会被全局对象替换，且原始值会转化为对象。
- argsArray: 可选。An array-like object, specifying the arguments with which func should be called, or null or undefined if no arguments should be provided to the function. (此处暂时不明如何译)

返回使用指定的 this 和 arguments 调用函数的结果。

**Using apply to append an array to another**

我们可以通过 `push` 给一个数组追加元素。并且，`push` 接受可变数量的参数，我们也可以一次添加多个元素。但是，如果我们通过 `push` 添加一个数组，那么这个数组会被作为一个元素添加进去，而不是添加一个个元素，这样我们最后会得到一个数组里面有一个数组。那么如果我们不想要这样的结果呢？我们还可以用 `concat`，`concat` 确实可以实现我们想要的一个数组 concat 另一个数组，但是 `concat` 会合并之后返回一个新的数组，而不是在已有数组上追加。那么想对已有数组追加另一个数组怎么办呢？可以用 `apply` 解决。

```javascript
// push/concat/apply
var arr = [1, 2];

// case 1: push
// Can push one or more elements to an array.
// But, if we pass an array to push, it will actually add that array as a single element, 
// instead of adding the elements individually, so we end up with an array inside an array.
arr.push(3); // arr = [1, 2, 3]
arr.push(4, 5); // arr = [1, 2, 3, 4, 5]
arr.push([6, 7]); // arr = [1, 2, 3, 4, 5, [6, 7]]
arr.pop();

// case 2: concat
// Can append an array to another.
// But it does not actually append to the existing array but creates and returns a new array.
arr.concat([6, 7]); // return [1, 2, 3, 4, 5, 6, 7], arr = [1, 2, 3, 4, 5]

// case 3: apply
// Can append an array to another.
// Syntax: Array.prototype.push.apply(arr, [6, 7]) === arr.push.apply(arr, [6, 7])
Array.prototype.push.apply(arr, [6, 7]); // arr = [1, 2, 3, 4, 5, 6, 7]
```

**Using apply and built-in functions**

聪明的使用 `apply` 允许你在某些任务上使用内置函数，不然你可能就需要写循环来实现。

```javascript
// min/max number in an array
var numbers = [5, 6, 2, 3, 7];

// using Math.min/Math.max apply
var max = Math.max.apply(null, numbers); // 7
// This about equal to Math.max(numbers[0], ...)
// or Math.max(5, 6, ...)
var min = Math.min.apply(null, numbers); // 2
```

```javascript
// simple loop based algorithm
function getMinAndMax(arr) {
	var min = +Infinity;
	var max = -Infinity;

	for (var i = 0, len = arr.length; i < len; i++) {
    if (arr[i] < min) {
			min = arr[i];
		}
		if (arr[i] > max) {
			max = arr[i];
		}
	} 
	
	return [min, max];
}

getMinAndMax([10, 2, 1, 8, 20, 0, 15]) // [0, 20]
```

但是要注意的是：在使用 `apply` 的时候，你的任务不能超过 JavaScript 引擎的 argument 长度限制。使用 `apply` 调用一个函数并且传入太多的 arguments 时（假设有成千上万个参数）产生的结果，是由引擎来决定的（JavaScriptCore 的编码参数限制为 65536），因为并没有明确的限制（实际上甚至是任何过度大堆栈行为的本质）。部分引擎会抛出异常。更不好的是，其他引擎会随意限制 the applied function 的参数个数。

为了避免这种不友好的情况，我们可以主动提供一个参数限制，来保证引擎的运行不出现不可控制的问题。

```javascript
function minOfArray(arr) {
  var min = Infinity;
  var QUANTUM = 32768;

  // 这里相当于将 arr 原数组根据你的限制长度，拆分成符合长度小数组，
  // 然后每个小数组求最小值，再对求解出的最小值求最小值
  for (var i = 0, len = arr.length; i < len; i += QUANTUM) {
    var submin = Math.min.apply(null, 
                                arr.slice(i, Math.min(i+QUANTUM, len)));
    min = Math.min(submin, min);
  }

  return min;
}

var min = minOfArray([5, 6, 2, 3, 7]); // 2
// 加入尝试将 QUANTUM 的限制设为 2，看一下怎么运行的？
// interesting...
```

**Using apply to chain constructors**

> You can use call to chain constructors for an object, similar to Java.

这个例子，官方 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 上有点复杂，暂不列出，建议理解参考 `call` 的 **Using apply to chain constructors**

---

## bind

> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

一位前辈的解释是：`bind()` 方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 `bind()` 方法的第一个参数作为 `this` ，传入 `bind()` 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

```javascript
function.bind(thisArg[, arg1[, arg2[, ...]]])
```

- thisArg: 当调用绑定函数时，这个参数值作为 `this` 值传给目标函数。如果绑定函数被当作构造器使用 `new` 操作符时，这个参数将会被忽略。
- arg1, arg2, ... : 当调用目标函数的时候，这些参数会被预先添加在绑定函数的参数之前，一起传给目标函数。

**Return Value: A copy of the given function with the specified this value and initial arguments.** 

**Description**

> The bind() function creates a new bound function (BF). A BF is an exotic function object (a term from ECMAScript 2015) that wraps the original function object. Calling a BF generally results in the execution of its wrapped function.

`bind()` 创建了一个**新的绑定函数(BF)**。BF 是一个包含了原始函数(目标函数)的外来函数对象（ECMAScript 2015的术语）。调用 BF 结果通常是执行它包装的函数。

A BF has the following internal properties:
- [[BoundTargetFunction]] - the wrapped function object.
- [[BoundThis]] - the value that is always passed as this value when calling the wrapped function.
- [[BoundArguments]] - a list of values whose elements are used as the first arguments to any call to the wrapped function.
- [[Call]] - executes code associated with this object. Invoked via a function call expression. The arguments to the internal method are a this value and a list containing the arguments passed to the function by a call expression.

When bound function is called, it calls internal method [[Call]] on [[BoundTargetFunction]], with following arguments Call(boundThis, args). Where, boundThis is [[BoundThis]], args is [[BoundArguments]] followed by the arguments passed by the function call.

A bound function may also be constructed using the new operator: doing so acts as though the target function had instead been constructed. The provided this value is ignored, while prepended arguments are provided to the emulated function.


**Polyfill**
 
（个人觉得这个 Polyfill 有助于理解 `bind` 的部分实现原理，看代码来的更加清晰一些。但是**不等同于内置**的 `bind`）

如果原生不支持 `bind`，在脚本最开始的地方加入以下代码段，可以帮助你你使用 `bind` 的**大部分**功能，来解决部分问题。

```javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if (typeof this !== 'function') {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
    }

    var aArgs   = Array.prototype.slice.call(arguments, 1),
        fToBind = this,
        fNOP    = function() {},
        fBound  = function() {
          return fToBind.apply(this instanceof fNOP
                 ? this
                 : oThis,
                 aArgs.concat(Array.prototype.slice.call(arguments)));
        };

    if (this.prototype) {
      // Function.prototype doesn't have a prototype property
      fNOP.prototype = this.prototype; 
    }
    fBound.prototype = new fNOP();

    return fBound;
  };
}

// -----------------------------------------------------------------------
// fToBind = this: 指目标函数(调用 bind 的函数)，是 bind 的操作对象。
// fBound: 调用 bind 产生的绑定函数。
// aArgs: bind 调用时传入的参数(第一个参数除外)，
//        后面又与绑定函数的调用传入的参数合并，
//        最终传给 apply 作为参数(第一个参数除外)。
// fNOP: 为什么会使用 fNOP 呢？个人理解
//       首先，这里的 fNOP 是一个很干净的 function ，
//       然后，fNOP 的原型用的是 this(目标函数) 的原型，这里是第一点，
//       继而，fBound(绑定函数) 的原型是 new fNOP()，这里是第二点，
//       这利用构造函数与实例的继承关系，使得绑定函数拥有目标函数的原型上的所有东西。
// 最后返回 fBound(绑定函数)
// ----------------------------------------
// ？其中的三目判断，不太理解？
// * 根据 thisArg 这个参数的定义可知，绑定函数有可能被作为构造函数，
// * 当使用 new 操作符作用于绑定函数的时候，thisArg 会被忽略，所以有这一个三目判断。
```

这个算法与原生的算法有一些不同的地方，以下列出的也仅是部分，并未详尽无遗。
- 这个算法依赖了 `Array.prototype.slice()` , `Array.prototype.concat()`, `Function.prototype.call()` and `Function.prototype.apply()` 这些内置方法。
- 这个算法创建的函数并没有实现 `caller` 以及会在 get, set, deletion 上抛出 `TypeError` 错误的 `arguments` 属性这两个不可变的"毒药"。（如果这个算法支持 `Object.defineProperty` 的话，或许这个没有实现的地方后面会被添加，又或者如果这个算法支持 `__defineGetter__` 和 `__defineSetter__` 的扩展，就可以部分是实现[指没有抛出异常这个事情]）
- 这个算法创建的函数有 `prototype` 属性。（正确的绑定函数并没有）（此处也不是很明白）
- 这个算法创建的绑定函数的 `length` 属性没有遵循 ECMA-262 标准：它的 `length` 是 0，而在原生算法中，根据目标函数的 `length` 属性和预先指定的参数个数计算，可能返回一个非零的 `length` 属性。

如果你选择使用这个算法，**一定不能依赖于ECMA-262，但是ECMA-5是可以的**。在根据规范实现的 `bind()` 被广泛支持之前， 在某些情况下这个算法可以作为一个过渡（也可以做出相应修改以适应特定需求）

**Creating a bound function**

一个简单的 `bind()` 使用是创建一个新函数，无论如何调用须得传进一个指定的 `this` 值。JavaScript初学者常见的一个误解就是，从一个对象中提取该对象的一个方法，之后再调用这个提取出的函数时，期望依然能够保留原始对象作为函数中的 `this` 值。然而，如果没有特别熟知这些，通常会丢失原始对象。对该函数进行绑定原始对象，即可巧妙的解决这个问题。

```javascript
var name = '-window-';

var apple = {
  name: 'apple',
  sayName: function() {
    console.log(this.name);
  }
}

// 基于原始对象的基础上调用，this 指向该原始对象
apple.sayName() // apple

// 从原始对象中提取 sayName 方法，单独调用 globalSayName，this 指向全局对象
var globalSayName = apple.sayName
globalSayName() // -window-

// 利用 bind 创建一个新的绑定函数，且该绑定函数的 this 值永久指向 apple
var boundSayName = globalSayName.bind(apple)
boundSayName() // apple
```

**Partially applied functions**

一个简单的 `bind()` 使用是创建一个新函数，且传进预先指定的初始 arguments 对象。这些 arguments 跟在 `this` 值后面，然后一起被插入目标函数的 arguments 之前，所有的参数会被传给绑定函数，无论绑定函数何时被调用。

```javascript
function list() {
  return Array.prototype.slice.call(arguments);
}

var list1 = list(1, 2, 3); // [1, 2, 3]

// Create a function with a preset leading argument
var leadingThirtysevenList = list.bind(null, 37);

var list2 = leadingThirtysevenList(); 
// [37]

var list3 = leadingThirtysevenList(1, 2, 3);
// [37, 1, 2, 3]
```

**With setTimeout**

在默认的 `window.setTimeout()` 中，`this` 指向全局对象 `window` (`global`)。当调用类的方法时需要 `this` 指向类的实例，为了能够保留这个实例，可以显示使用 `bind` 来绑定 `this` 到回掉函数。

```javascript
function LateBloomer() {
  this.petalCount = Math.floor(Math.random() * 12) + 1;
}

// Declare bloom after a delay of 1 second
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.declare = function() {
  console.log('I am a beautiful flower with ' +
    this.petalCount + ' petals!');
};

var flower = new LateBloomer();
flower.bloom();  
// after 1 second, triggers the 'declare' method
```

**Bound functions used as constructors**

个人认为，这种情况没有什么必要，完全可以操作目标函数。MDN 也不建议在生产环境这样使用，即不多说明。

**Creating shortcuts**

`bind()` 在需要为一个指定 `this` 值的函数创建一个快捷方式是非常有用的。

```javascript
// 将类数组对像转为真正的数组
var slice = Array.prototype.slice;
slice.apply(arguments)
```
With bind(), this can be simplified. In the following piece of code, slice is a bound function to the apply() function of Function.prototype, with the this value set to the slice() function of Array.prototype. This means that additional apply() calls can be eliminated:

```javascript
// same as "slice" in the previous example
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.apply.bind(unboundSlice);

// ...

slice(arguments);
```



---

## call/apply/bind 区别

**call/apply**

`apply` 和 `call` 用法非常相似，除了支持的参数格式不同以外。使用 `apply` 的时候可以不必知道被调用对象的参数个数。

？另有人提到，`call` 的性能比 `apply` 好。待查？

- [规范](http://www.ecma-international.org/ecma-262/5.1/#sec-15.3.4.3)
- apply 需要处理异常情况以及 [argsArray] 的处理成合适的参数格式
- call 接收的参数本身就是合适的参数格式

call
```javascript
function.call(thisArg, arg1, arg2, ...)
```
apply
```javascript
function.apply(thisArg, [argsArray])
```

顺一道面试题来，哈哈哈哈哈哈

定义一个 log 方法，让它可以代理 console.log 方法：

```javascript
// 传入一个参数
function log(msg) {
  console.log(msg);
}

log('log 1'); // log 1
log('log 2'); // log 2
```

```javascript
// 不限制参数个数
function log() {
  console.log.apply(console, arguments);
}

log('log 1', 'log 2', 'log 3'); // log 1 log 2 log 3
```

```javascript
// 不限制参数个数，且可以添加前缀 'app: '
function log() {
  var args = Array.prototype.slice.call(arguments); // 将类数组对象转为数组
  args.unshift('app:');

  console.log.apply(console, args);
}

log('log 1', 'log 2', 'log 3'); // app: log 1 log 2 log 3
```

**call/apply/bind**

```javascript
var apple = {
  name: 'apple',
  sayName: function sayName() {
    console.log(this.name);
  }
};

var banana = {
  name: 'banana'
}

apple.sayName.call(banana) // banana
apple.sayName.apply(banana) // banana
apple.sayName.bind(banana)() // banana
// 当你希望改变上下文环境之后（就是 this 的 context）
// 并非立即执行，可以使用 `bind`，
// 而 call/apply 则会立即执行。
```

- apply、call、bind 三者都是可以用来改变函数的 this 对象的指向；
- apply、call、bind 三者第一个参数都是 this 要指向的对象，也就是想指定的上下文；
- apply、call、bind 三者都可以利用后续参数传参；
- bind 是返回绑定函数，便于稍后调用，绑定函数的 this 会被永久绑定；apply、call 则是立即调用。





## 参考
- [MDN-call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
- [MDN-apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
- [MDN-bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
- [【优雅代码】深入浅出 妙用Javascript中apply、call、bind](http://www.cnblogs.com/coco1s/p/4833199.html)
- [How does the JavaScript .apply method work?](https://stackoverflow.com/questions/34802982/how-does-the-javascript-apply-method-work)

---

好记性不如烂笔头。