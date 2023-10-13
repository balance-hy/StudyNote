# 函数式编程

函数式编程是一种方案简单、功能独立、对作用域外没有任何副作用的编程范式：`INPUT -> PROCESS -> OUTPUT`。

函数式编程：

1）功能独立——不依赖于程序的状态（比如可能发生变化的全局变量）；

2）纯函数——同一个输入永远能得到同一个输出；

3）有限的副作用——可以严格地限制函数外部对状态的更改。

举例说明：

```js
const prepareTea = (value) => value;//匿名函数返回
```

首先，无法更改`prepareTea`的值，因为它由`const`修饰，其次可以复用它，传入什么`value`，就可以得到什么`value`。

再举一例，体现函数式编程的模块化和可复用性

```js
// 非函数式编程方式
function sumOfEvenSquares(arr) {
    let sum = 0;
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] % 2 === 0) {
            sum += arr[i] * arr[i];
        }
    }
    return sum;
}

const numbers = [1, 2, 3, 4, 5, 6];
const result = sumOfEvenSquares(numbers);
console.log(result); // 输出 56

// 函数式编程方式
const isEven = (num) => num % 2 === 0;
const square = (num) => num * num;
const sum = (arr) => arr.reduce((acc, num) => acc + num, 0);

const numbers = [1, 2, 3, 4, 5, 6];
const result = sum(numbers.filter(isEven).map(square));
console.log(result); // 输出 56
```

在函数式编程方式中，我们使用了`filter`和`map`高阶函数来操作数组，将计算的过程拆分成更小的纯函数，使代码更具表达性和可读性。同时，没有改变原始数据，保持了不可变性。这是一个简单示例，但它展示了函数式编程的一些核心概念。

## 函数式编程术语

**Callbacks 是被传递到另一个函数中调用的函数**。 你应该已经在其他函数中看过这个写法，例如在 `filter` 中，回调函数告诉 JavaScript 以什么规则过滤数组。

函数就像其他正常值一样，可以赋值给变量、传递给另一个函数，或从其它函数返回，这种函数叫做头等 first class 函数。 在 JavaScript 中，所有函数都是**头等函数**。

将函数作为参数或将函数作为返回值返回的函数叫作**高阶函数**。

**当函数被传递给另一个函数或从另一个函数返回时，那些传入或返回的函数可以叫作 lambda。**

## 命令式编程危害

使用函数式编程是一个好的习惯。 它使你的代码易于管理，避免潜在的 bug。 但在开始之前，先看看命令式编程方法，以强调你可能有什么问题。

在英语 (以及许多其他语言) 中，命令式时态用来发出指令。 同样，命令式编程是向计算机提供一套执行任务的声明。

命令式编程常常改变程序状态，例如更新全局变量。 一个典型的例子是编写 `for` 循环，它为一个数组的索引提供了准确的迭代方向。

相反，函数式编程是声明式编程的一种形式。 通过调用方法或函数来告诉计算机要做什么。

JavaScript 提供了许多处理常见任务的方法，所以你无需写出计算机应如何执行它们。 例如，你可以用 `map` 函数替代上面提到的 `for` 循环来处理数组迭代。 这有助于避免语义错误，如调试章节介绍的 "Off By One Errors"。

考虑这样的场景：你正在浏览器中浏览网页，并想操作你打开的标签。 下面我们来试试用面向对象的思路来描述这种情景。

窗口对象由选项卡组成，通常会打开多个窗口。 窗口对象中每个打开网站的标题都保存在一个数组中。 在对浏览器进行了如打开新标签、合并窗口、关闭标签之类的操作后，你需要输出所有打开的标签。 关掉的标签将从数组中删除，新打开的标签（为简单起见）则添加到数组的末尾。

代码编辑器中显示了此功能的实现，其中包含 `tabOpen()`，`tabClose()`，和 `join()` 函数。 `tabs` 数组是窗口对象的一部分用于储存打开页面的名称。

```js
// tabs 是在窗口中打开的每个站点的 title 的数组
const Window = function(tabs) {
  this.tabs = tabs; // 我们记录对象内部的数组
};

// 当你将两个窗口合并为一个窗口时
Window.prototype.join = function(otherWindow) {
  this.tabs = this.tabs.concat(otherWindow.tabs);
  return this;
};

// 当你在最后打开一个选项卡时
Window.prototype.tabOpen = function(tab) {
  this.tabs.push('new tab'); // 我们现在打开一个新的选项卡
  return this;
};

// 当你关闭一个选项卡时
Window.prototype.tabClose = function(index) {

  // 只修改这一行下面的代码

  const tabsBeforeIndex = this.tabs.splice(0, index); // 点击之前获取 tabs
  const tabsAfterIndex = this.tabs.splice(index + 1); // 点击之后获取 tabs

  this.tabs = tabsBeforeIndex.concat(tabsAfterIndex); // 将它们合并起来

  // 只修改这一行上面的代码

  return this;
 };

// 我们创建三个浏览器窗口
const workWindow = new Window(['GMail', 'Inbox', 'Work mail', 'Docs', 'freeCodeCamp']); // 你的邮箱、Google Drive 和其他工作地点
const socialWindow = new Window(['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium']); // 社交网站
const videoWindow = new Window(['Netflix', 'YouTube', 'Vimeo', 'Vine']); // 娱乐网站

// 现在执行打开选项卡，关闭选项卡和其他操作
const finalTabs = socialWindow
  .tabOpen() // 打开一个新的选项卡，显示猫的图片
  .join(videoWindow.tabClose(2)) // 关闭视频窗口的第三个选项卡，并合并
  .join(workWindow.tabClose(1).tabOpen());
console.log(finalTabs.tabs);
```

在编辑器中运行代码。 它使用了有副作用的方法，导致输出错误。 存储在 `finalTabs.tabs` 中的打开标签的最终列表应该是 `['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium', 'new tab', 'Netflix', 'YouTube', 'Vine', 'GMail', 'Work mail', 'Docs', 'freeCodeCamp', 'new tab']`，但输出会略有不同。

**问题出在 `tabClose()` 函数里的 `splice`。 不幸的是，`splice` 修改了调用它的原始数组，所以第二次调用它时是基于修改后的数组，才给出了意料之外的结果。**

## 使用函数式编程避免变化

### 原则一不改变任何东西

这是一个小例子，还有更广义的定义——在变量，数组或对象上调用一个函数，这个函数会改变对象中的变量或其他东西。

**函数式编程的核心原则之一是不改变任何东西。 变化会导致错误。 如果一个函数不改变传入的参数、全局变量等数据，那么它造成问题的可能性就会小很多**。

前面的例子没有任何复杂的操作，但是 `splice` 方法改变了原始数组，导致 bug 产生。

回想一下，在函数式编程中，改变或变更叫做 mutation，这种改变的结果叫做“副作用”（side effect）。 理想情况下，函数应该是不会产生任何副作用的 pure function。

**让我们尝试掌握这个原则：不要改变代码中的任何变量或对象。**

填写 `incrementer` 函数的代码，使其返回值为全局变量 `fixedValue` 增加 1 。

```js
// 全局变量
let fixedValue = 4;

function incrementer() {
  // 只修改这一行下面的代码
  return fixedValue+1;
  // 只修改这一行上面的代码
}
```

上一个挑战是更接近函数式编程原则的挑战，但是仍然缺少一些东西。

虽然我们没有改变全局变量值，但在没有全局变量 `fixedValue` 的情况下，`incrementer` 函数将不起作用。

### 原则二 明确参数，不越界使用

**函数式编程的另一个原则是：总是显式声明依赖关系。 如果函数依赖于一个变量或对象，那么将该变量或对象作为参数直接传递到函数中。**

这样做会有很多好处。 其**中一点是让函数更容易测试，因为你确切地知道参数是什么，并且这个参数也不依赖于程序中的任何其他内容。**

其次，这样做可以让你更加自信地更改，删除或添加新代码。 因为你很清楚哪些是可以改的，哪些是不可以改的，这样你就知道哪里可能会有潜在的陷阱。

最后，无论代码的哪一部分执行它，函数总是会为同一组输入生成相同的输出。

更新 `incrementer` 函数，明确声明其依赖项。编写 `incrementer` 函数，获取它的参数，然后将值增加 1。

```js
// 全局变量
let fixedValue = 4;

// 只修改这一行下面的代码
function incrementer(fixedValue) {
  return fixedValue+1;
  // 只修改这一行上面的代码
}
```

### 在函数中重构全局变量-例子

目前为止，我们已经看到了函数式编程的两个原则：

1. 不要更改变量或对象 - 创建新变量和对象，并在需要时从函数返回它们。 提示：**使用类似 `const newArr = arrVar` 的东西，其中 `arrVar` 是一个数组，只会创建对现有变量的引用，而不是副本。 所以更改 `newArr` 中的值会同时更改 `arrVar` 中的值。**
2. 声明函数参数 - 函数内的任何计算仅取决于参数，而不取决于任何全局对象或变量。

给数字增加 1 不够有意思，但是我们可以在处理数组或更复杂的对象时应用这些原则。

重构代码，使全局数组 `bookList` 在函数内部不会被改变。 `add` 函数可以将指定的 `bookName` 增加到数组末尾并返回一个新的数组（列表）。 `remove` 函数可以从数组中移除指定 `bookName`。

**注意：** 两个函数都应该返回一个数组，任何新参数都应该在 `bookName` 参数之前添加。

```js
var bookList = ["The Hound of the Baskervilles", "On The Electrodynamics of Moving Bodies", "Philosophiæ Naturalis Principia Mathematica", "Disquisitiones Arithmeticae"];

function add(arr, bookName) {
  let newArr = [...arr]; //利用展开运算符复制数组，当然使用其他方法返回新数组也可以比如map、filter、slice等等
  newArr.push(bookName); //其实直接[...arr,bookName]即可
  return newArr; 
}

function remove(arr, bookName) {
  let newArr = [...arr]; //这里直接使用filter函数即可，返回元素不为bookname的那些元素
  if (newArr.indexOf(bookName) >= 0) {
    newArr.splice(newArr.indexOf(bookName), 1);
    return newArr;
  }
}
var newBookList = add(bookList, 'A Brief History of Time');
var newerBookList = remove(bookList, 'On The Electrodynamics of Moving Bodies');
var newestBookList = remove(add(bookList, 'A Brief History of Time'), 'On The Electrodynamics of Moving Bodies');

console.log(bookList);
```

## 在原型上实现map

```js
Array.prototype.myMap = function(callback,index,array) {
  const newArray = [];
  // 只修改这一行下面的代码
  for(let i=0;i<this.length;i++){
      newArray.push(callback(this[i],i,this));
  }
  // 只修改这一行上面的代码
  return newArray;
};
```

之前用到了 `Array.prototype.map()` 方法（即 `map()`），通过 `map` 返回一个与调用它的数组长度相同的数组。 只要它的回调函数不改变原始数组，它就不会改变原始数组。

换句话说，`map` 是一个纯函数，它的输出仅取决于输入的数组和作为参数传入的回调函数。 此外，它接收另一个函数作为它的参数。

实现一个 `map`，加深对它的了解。 你可以用 `for` 循环或者 `Array.prototype.forEach()` 方法。

## filter+map

`watchList` 变量中包含一组存有多部电影信息对象。 结合 `filter` 和 `map` 返回一个 `watchList` 只包含 `title` 和 `rating` 属性的新数组。 新数组只包含 `imdbRating` 值大于或等于 8.0 的对象。 请注意，`rating` 值在对象中保存为字符串，你可能需要将它转换成数字来执行运算。

```js
const watchList = [
  {
    "Title": "Inception",
    "Year": "2010",
    "Rated": "PG-13",
    "Metascore": "74",
    "imdbRating": "8.8"
  },
  {
    "Title": "Interstellar",
    "Year": "2014",
    "Rated": "PG-13",
    "Metascore": "74",
    "imdbRating": "8.6"
  }
];

// 只修改这一行下面的代码
const filteredList =watchList.map(watchlist=>({title:watchlist.Title,rating:watchlist.imdbRating})).filter(list=>parseInt(list.rating)>=8.0);
// 只修改这一行上面的代码
console.log(filteredList);
```

以上是自己的解法，提供一个通过解构运算的解法，值得学习

```js
const filteredList = watchList
  .filter(({ imdbRating }) => imdbRating >= 8.0)
  .map(({ Title: title, imdbRating: rating }) => ({ title, rating }));

console.log(filteredList);

//注意{ title, rating }，花括号会自动将变量名转为 变量名：变量值 的形式
```

### 在原型上实现filter

```js
Array.prototype.myFilter = function(callback) {
  const newArray = [];
  // 只修改这一行下面的代码
  for(let i=0;i<this.length;i++){
    if(callback(this[i],i,this)){
        newArray.push(this[i]);
    }
  }
  // 只修改这一行上面的代码
  return newArray;
};
```

## 柯里化和偏应用简介

函数的元数是它需要的参数的数量。函数柯里化（Currying）是指将一个 N 个函数转换为 N 个个数为 1 的函数。

换句话说，它重构一个函数，使其接受一个参数，然后返回另一个接受下一个参数的函数，依此类推。

这是一个例子：

```js
function unCurried(x, y) {
  return x + y;
}

function curried(x) {
  return function(y) {
    return x + y;
  }
}

const curried = x => y => x + y

curried(1)(2)
```

`curried(1)(2)`会回来的`3`。

如果您无法一次向函数提供所有参数，这在您的程序中非常有用。您可以将每个函数调用保存到一个变量中，该变量将保存返回的函数引用，该引用在下一个参数可用时采用该函数引用。这是使用上面示例中的柯里化函数的示例：

```js
const funcForY = curried(1);
console.log(funcForY(2)); // 3
```

类似地，部分应用可以描述为一次将几个参数应用于一个函数，并返回另一个应用于更多参数的函数。这是一个例子：

```js
function impartial(x, y, z) {
  return x + y + z;
}

const partialFn = impartial.bind(this, 1, 2);
partialFn(10); // 13
```
