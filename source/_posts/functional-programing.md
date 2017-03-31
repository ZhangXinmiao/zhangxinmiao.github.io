---
title: 函数式编程学习笔记
date: 2017-03-31 20:10:51
tags:
---

#### 什么是函数式编程

  [函数式编程](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B8%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80)是一种[编程范型](https://zh.wikipedia.org/wiki/%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B)，Javascript就支持这种编程范型。

#### 函数式编程的特点

##### 函数是一等公民


在 Javascript 中，我们可以像对其他类型一样对待函数，我们将函数存在数组里、将函数作为参数传递、作为返回值返回...没有什么特别的地方

为什么我们强调函数是一等公民呢，有这样一个例子

```js
// 太傻了  
var getServerStuff = function(callback){
  return ajaxCall(function(json){
    return callback(json);
  });
};

// 这才像样
var getServerStuff = ajaxCall;
```

在第一种写法中，给 ajaxCall 逃了一层是没有必要的，造成了代码冗余；还有一种情况是，当我们需要给 ajaxCall 添加一个参数的时候，包裹它的 getServerStuff 也需要跟着增加一个参数，这又使代码变得不利于维护。

第二种写法则一目了然，也不会出现添加一个参数需要修改多处的情况。相比起来，我们更认同第二种函数作为一等公民的写法。


##### 纯函数的方式编程

先来看看什么样的函数被认为是纯函数呢，slice 和 splice 是我们经常会用到的两个 Javascript 中的方法

```js
let arr = [1, 2, 3, 4, 5];
let newArr = arr.slice(0, 3);
console.log(arr);  // [ 1, 2, 3, 4, 5 ]
console.log(newArr);  // [ 1, 2, 3 ]

let newArr1 = arr.splice(0, 3);
console.log(arr); // [ 4, 5 ]
console.log(newArr1); // [ 1, 2, 3 ]
```

阅读这段代码，我们发现二者的效果是一样的，但是 slice 并没有改变原来的数组，而 splice 则永久的改变了原数组，如果我们的本意只是截取数组的一段，那么我们认为 splice 是不纯的，因为它做了多余的事情(改变原数组)。



再看这样一个例子

```js
// 不纯的
var minimum = 21;

var checkAge = function(age) {
  return age >= minimum;
};


// 纯的
var checkAge = function(age) {
  var minimum = 21;
  return age >= minimum;
};
```

第一种写法中的 checkAge 函数返回的结果依赖于外部的 minimum，这可能造成对于相同的 age 输入， 返回的结果可能不同，因为 minimum 是一个不确定的因素，第二种纯函数的写法就能避免这种困扰，checkAge 函数的返回值只依赖 age， 也就是说，只要传入值 age 是相同的，返回值也一定是相同的，这就是纯函数。

如果参数是一个对象，我们可以考虑把它转化成一个不可变的(Immutable)对象，也有Immutable.js的库来帮助我们做这件事情。

纯函数的概念也整对应着数学中的[函数](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B0)概念，在这种映射关系中，每个 x 中的元素只对应一个 y 中的元素

![1](functional-programing/function.png "图(1)")

- 引用透明性

引用透明性（referential transparency）: 如果一段代码可以替换成它执行所得的结果，而且是在不改变整个程序行为的前提下替换的，那么我们就说这段代码是引用透明的

基于使用纯函数输入相同输出相同的原则，我们可以得出——函数式编程是具有引用透明性的。

#### 柯里化（curry）

柯里化（curry）：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

下面是一段典型的 curry 函数的例子

```js
var add = function(x) {
  return function(y) {
    return x + y;
  };
};

var increment = add(1);
var addTen = add(10);

increment(2);  // 3

addTen(2);  // 12
```

同样，我们也可以借助工具完成函数 curry 化的工作，以下的例子利用 Lodash 库的 curry 方法

可以先大致看一下 Lodash 中 curry 的使用方法

```js
const curry = _.curry;

let func = (x, y) => {
	return [x, y, x + y];
}

let curryFunc = curry(func);

let a = curryFunc(1)(2);

let b = curryFunc(1, 2);
```

把我们需要 curry 化的函数传入 curry，返回的函数就被 curry 化处理了，我们可以把参数任意的组合分次传入。

这样，传入一个或者几个参数就能得到一个新的函数。



#### 函数组合（compose）

下面这段代码就是一个函数组合（compose）

```js
var compose = function(f,g) {
  return function(x) {
    return f(g(x));
  };
};
```

compose 使我们像搭乐高积木一样使用函数，随意选择两个函数，即可组合成一个新的函数，x 使数据在两个函数间传输。

compose 的一个例子

```js
// 取获得数组的第一个元素
var head = function(x) { return x[0]; };
// 获得一个反转的数组
var reverse = function(x) { return x.reverse()}
var toLowerCase = function(x) { return x.toLowerCase()}

// 组合 head 和 reserve，获得获取数组的最后一个元素的函数
var last = compose(head, reverse);

var result = last(['jumpkick', 'roundhouse', 'uppercut']);
console.log(result); // 'uppercut'

// 组合 toLowerCase 和 head，获得获取数组第一个元素并转化为小写
var snakeCase = compose(toLowerCase, head);
var result1 = snakeCase(["Q", "W", "E"]);
console.log(result1); // q
```

从这个例子可以发现，组合后是一个“从右向左”的过程

Lodash 为我们提供两种组合方式，分别是“从右向左”和“从左向右”

```js
function square(n) {
  return n * n;
}

var addSquare = _.flowRight([square, _.add]);
addSquare(1, 2);
// => 9

var addSquare = _.flow([_.add, square]);
addSquare(1, 2);
// => 9
```

用 Lodash 对上面的函数进行一下组合

```js
const composeRight = _.flowRight;
let newFunc = composeRight(toLowerCase, head, reverse);
let result = newFunc(["Q", "W", "E"]);
console.log(result); //e
```

可以看到，组合就像一个流水线一样，我们用函数指定分别做什么样的操作，经过流水线的加工，最后得出结果，具有很好的语义化。

#### 参考

- [JS函数式编程指南](https://www.gitbook.com/book/llh911001/mostly-adequate-guide-chinese/details)
- [阮一峰-函数式编程初探](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html)
