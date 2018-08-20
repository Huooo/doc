经济基础决定上层建筑。

---

## 我来了
  - 学习，记录，备忘。
  - 感谢参考过的所有资料的作者。

---


## Closures

> A closure is the combination of a function and the lexical environment within which that function was declared. 

闭包是函数以及声明该函数的词法环境的组合。

**Lexical scoping**

```javascript
// example 1
function init() {
  var name = 'Mozilla'; // name is a local variable created by init
  function displayName() { // displayName() is the inner function, a closure
    alert(name); // use variable declared in the parent function    
  }
  displayName();    
}

init();
```

`init()` 函数创建了一个变量 `name` 和一个函数 `displayName()`。`displayName()` 是一个定义在 `init()` 里面的内部函数，且只能在 `init()` 函数内部使用。`displayName()` 没有自己的内部变量。然而，因为内部函数可以访问外部函数的变量，`displayName()` 可以访问声明在父函数 `init()` 中的变量 `name`。然而，如果 `displayName()` 内部拥有同名变量，则会使用内部变量。

这是一个 **lexical scoping** 的例子。它描述了嵌套函数中解析变量的过程。"lexical" 一词指的是，词法范围使用源码中声明的变量来决定变量可用的位置。嵌套函数可以访问它们外部函数的变量。

**Closure**

```javascript
// example 2
function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
```

example 2 和 example 1 运行效果相同。有意思的是，两者不同的地方在于 example 2 中的，`displayName()` 内部函数在执行之前从外部函数返回，即函数 `makeFunc()` 中的返回值是函数 `displayName()`。

咋一看，无法直观的看出这段代码依然有效。在某些编程语言中，一个函数内部的变量仅在这个函数执行期间有效。一旦函数 `makeFunc()` 已经结束执行，那么你可能期望不再允许访问变量 `name`。然而，显然在 JavaScript 中不是这种情况，因为它依然能够访问变量 `name`。

这是因为函数在 JavaScript 中形成了闭包。闭包是一个函数和定义这个函数的词法环境的组合。这个环境包含创建闭包时所在的范围中的任何变量。这种情况下，`myFunc` 是一个引用，这个引用指向 `makeFunc` 运行创建的函数 `displayName` 的实例。函数 `displayName` 的实例依然保留着对这个词法环境的引用，这个词法环境中存在变量 `name`。因此，当调用 `myFunc` 时，变量 `name` 依然能够被访问，且通过 `alert` 显示其值为 `Mozilla`。

下面这个列子更有趣：

```javascript
// example 3
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```

example 3 中，我们定义了一个函数 `makeAdder(x)`，`makeAdder(x)` 需要一个参数 `x`，并且返回了一个新的函数。新函数中需要一个参数 `y`，并且返回结果 `x + y`。

本质上，`makeAdder` 是一个函数工厂，它创建了可以添加一个特定参数值的函数。在 example 3 中，我们用这种函数工厂创建了两个新的函数：一个是 `add5` 并且传入 5 作为参数值，另一个是 `add10` 并且传入 10 作为参数值。

`add5` 和 `add10` 都是闭包。**它们使用相同的函数体定义出来，但是存储的却是不同的词法环境**。在 `add5` 的词法环境中，`x = 5`，而在 `add10` 的词法环境中，`x = 10`。
 
**Practical closures**

闭包是有用的，因为它们将特定词法环境中的变量和函数关联起来，并且使得函数可以操作这些变量。这和面向对象开发有着很明显的相似之处，面向对象开发中，对象允许我们将对象的属性和方法关联起来。

Consequently, you can use a closure anywhere that you might normally use an object with only a single method.(暂时不懂这句话的意思)

在 web 开发中常常用到闭包。在前端 JavaScript 中编写的大部分代码是基于事件的，我们定义一些行为，然后将这些行为附加到用户触发的事件中(如点击或者按键事件)。我们的代码通常作为一个回调函数添加进事件中：一个独立的函数在响应一个事件时被执行。

例如，假设我们希望在页面中添加一些按钮来调节字体大小。一个思路时，我们首先以单位 `px` 来定义 `body` 的 `font-size` 值，然后其它元素的以单位 `em` 来定义 `font-size`。那么之后的调节，只需点击按钮之后修改 `body` 的 `font-size` 值即可。

html:

```html
<p>Some paragraph text</p>
<h1>some heading 1 text</h1>
<h2>some heading 2 text</h2>

<a href="#" id="size-12">12</a>
<a href="#" id="size-14">14</a>
<a href="#" id="size-16">16</a>
```

css:

```css
body {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 12px;
}

h1 {
  font-size: 1.5em;
}
h2 {
  font-size: 1.2em;
}
```

javascript:

```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```

**Emulating private methods with closures**

诸如 Java 之类的语言提供了将方法声明为私有的能力，这意味着它们只能由同一类中的其他方法调用。

JavaScript 并没有提供这种操作的原生实现，但是它能够利用闭包模拟私有方法。私有方法不仅仅能够限制代码的访问，它们还提供了一种强大的途径来管理你的全局命名空间，使得非必要方法不会将公共接口混乱到代码中。

example 4 说明了如何使用闭包来定义公用函数，这个公用函数能够访问私有函数和私有变量。

```javascript
// example 4
var counter = (function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  };   
})();

console.log(counter.value()); // logs 0
counter.increment();
counter.increment();
console.log(counter.value()); // logs 2
counter.decrement();
console.log(counter.value()); // logs 1
```

在 example 4 中，**每一个闭包都有它自己的词法环境**。但是，这里我们创建了一个独立的词法环境，这个词法环境被三个函数共享：`counter.increment`，`counter.decrement`，和 `counter.value`。

这个共享的词法环境在匿名函数体中创建的，这个匿名函数定义且立即执行。这个词法环境包含两个私有元素，一个变量 `privateCounter` 和一个函数 `changeBy`。这些私有元素是不能被匿名函数外部直接访问的。相反，他们只能通过由匿名函数包装器返回的三个公用函数访问到。

这三个公用函数共享一个词法环境。正是由于 JavaScript 的词法作用域，他们都可以访问变量 `privateCounter` 和函数 `changeBy`。

Note: example 4 是我们定义了一个立即执行的匿名函数，并且将这个匿名函数分配给变量 `counter` 生成一个计数器。我们也可以存储这个函数给另一个独立的变量 `makeCounter` 并且使用它创建不同的计数器。

```javascript
// example 5
var makeCounter = function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }  
};

var counter1 = makeCounter();
var counter2 = makeCounter();
alert(counter1.value()); /* Alerts 0 */
counter1.increment();
counter1.increment();
alert(counter1.value()); /* Alerts 2 */
counter1.decrement();
alert(counter1.value()); /* Alerts 1 */
alert(counter2.value()); /* Alerts 0 */
```

注意两个计数器 `counter1` 和 `counter2` 是如何分别保持各自的独立性。每个闭包通过自己的闭包引用不同版本的变量 `privateCounter` 值。每次调用其中一个计数器时，它的词法环境通过改变这个变量的值而发生改变。然而，在一个闭包中修改这个变量的值并不会影响另一个闭包里面的值。

**Closure Scope Chain**

每一个闭包都有三个作用域：
- Local Scope( Own Scope )
- Outer Functions Scope
- Global Scope

因此，闭包中我们可以访问这三个作用域，但是当使用嵌套函数的时候会常常犯错。请看下面的例子：

```javascript
// example 6
// global scope
var e = 10;
function sum(a){
  return function(b){
    return function(c){
      // outer functions scope
      return function(d){
        // local scope
        return a + b + c + d + e;
      }
    }
  }
}

console.log(sum(1)(2)(3)(4)); // log 20

// We can also write without anonymous functions:

// global scope
var e = 10;
function sum(a){
  return function sum2(b){
    return function sum3(c){
      // outer functions scope
      return function sum4(d){
        // local scope
        return a + b + c + d + e;
      }
    }
  }
}

var s = sum(1);
var s1 = s(2);
var s2 = s1(3);
var s3 = s2(4);
console.log(s3) //log 20
```

在 example 6 中，有一系列嵌套函数可以访问外层函数的作用域，但会误解为只能访问最近的外层函数作用域。这种情况下，可以认为，所有闭包可以访问声明他们的所有外层函数作用域。



**Creating closures in loops: A common mistake**

在 ECMAScript 2015 的 `let` 关键词出现之前，在一个循环里面创建闭包会普遍出现类似的问题。如下，想实现一个表单中，聚焦不同的输入框时提供不同的提示信息。

```html
<p id="help">Helpful notes will appear here</p>
<p>E-mail: <input type="text" id="email" name="email"></p>
<p>Name: <input type="text" id="name" name="name"></p>
<p>Age: <input type="text" id="age" name="age"></p>
```

```javascript
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
```

在数组 `helpText` 中配置了三条不同的提示信息信息，每条数据对应不同的输入框提示信息。通过循环数组来将提示信息与对应的输入框唯一关联，并在聚焦不同的输入框时展示对应的提示信息。但是上面的代码并不符合预期效果。因为，分配给输入框的聚焦事件的是闭包，这些闭包是由函数的定义和 `setupHelp` 的函数作用域的捕获环境组成。循环创建了三个闭包，但是这三个闭包是共享一个词法环境，这个词法环境包含一个可变值的变量 `item.help` 。当聚焦事件的回掉函数被执行的时候，`item.help` 的值已经确定是何值了。因为在聚焦事件执行的时候，循环已经执行完成，`item` 这个对象（三个闭包共享）已经指向数组 `helpText` 的最后一项，即 `item.help = Your age (you must be over 16)`


```javascript
// 方法一：
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function makeHelpCallback(help) {
  return function() {
    showHelp(help);
  };
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
  }
}

setupHelp();
```

方法一中，利用的是，同一个函数，多次调用会生成不同的独立的词法环境。聚焦事件上分配的是调用函数 `makeHelpCallback` 的 `return` 值，所以三次聚焦事件上的是三个不同词法环境的 `return` 值，而这三个词法环境传入的 `item.help` 值也是不相同的，互不影响。

```javascript
// 方法二：
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    (function() {
       var item = helpText[i];
       document.getElementById(item.id).onfocus = function() {
         showHelp(item.help);
       }
    })(); // Immediate event listener attachment with the current value of item (preserved until iteration).
  }
}

setupHelp();
```

方法二中，利用匿名函数可以生成一个函数作用域，即调即用，每次匿名调用生成不同的函数作用域，保留住不同的 `item.help` 值给闭包使用，其实这种写法的 `item` 值也是独立不受影响的。

```javascript
// 方法三：
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    let item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
```

利用 ECMAScript 2015 的 `let` 关键词声明一个块级变量 `item` ，闭包使用的 `item` 是不同块级作用域的值，因此不需要额外的闭包来保留不同的循环值。

```javascript
// 方法四：
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];
  
  helpText.forEach(function(text) {
    document.getElementById(text.id).onfocus = function() {
      showHelp(text.help);
    }
  });
}

setupHelp();
```

方法四中，利用的是 `forEach` 的回调函数，其实原理也是利用函数形成不同的词法作用域。

**Performance considerations**

如果没有需要闭包的任务，那么就不必在一个函数里面创建函数，因为闭包会给运行速度和内存消耗带来负面效果。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

例如，当创建一个新的对象或者类的时候，通常会在对象的原型上创建方法而不是在对象的构造函数中创建。这是因为，如果在构造函数中创建方法，那么每次调用构造函数时，都会重新分配方法。

```javascript
// example 7
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
  this.getName = function() {
    return this.name;
  };

  this.getMessage = function() {
    return this.message;
  };
}
```

在 example 7 中，虽然使用了闭包，但是并没有真正用到闭包的好处。可以用下面的写法。

```javascript
// example 8
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}
MyObject.prototype.getName = function() {
  return this.name;
};
MyObject.prototype.getMessage = function() {
  return this.message;
};
```

在 example 8 中，同一个构造函数 `new` 出的对象共享继承原型，不需要每一个对象都去定义相同的方法。

  

## 参考
- [MDN-Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)


---

好记性不如烂笔头。