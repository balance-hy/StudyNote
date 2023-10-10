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