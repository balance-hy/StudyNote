# Js方法和题目

## 方法

### 数组

#### push 末尾添加

push（）方法接受一个或多个参数并按照它们出现的顺序将它们附加到数组的末尾。它返回数组的新长度。

```javascript
const arr1 = [1, 2, 3];
arr1.push(4, 5);//可以加数据

const arr2 = ["Stimpson", "J", "cat"];
arr2.push(["happy", "joy"]);//也可以加数组
```

#### unshift 开端添加

不仅可以 `shift`（移出）数组中的第一个元素，也可以 `unshift`（移入）一个元素到数组的头部。

`.unshift()` 函数用起来就像 `.push()` 函数一样，但不是在数组的末尾添加元素，`unshift()` 在数组的头部添加元素

#### pop 移除末尾元素

`.pop()` 函数用来弹出一个数组末尾的值。 我们可以把这个弹出的值赋给一个变量存储起来。 换句话说就是 `.pop()` 函数移除数组末尾的元素并返回这个元素。

数组中任何类型的元素（**数值**，**字符串**，**甚至是数组**）都可以被弹出来 。

#### shift 移除首元 素

`pop()` 函数用来移出数组中最后一个元素。 如果想要移出第一个元素要怎么办呢？

这时候我们就需要 `.shift()` 了。 它的工作原理就像 `.pop()`，但它移除的是第一个元素，而不是最后一个。

#### map 创建指定方法新数组，长度不变

`map()` 是 JavaScript 数组对象的一个方法，它用于创建一个新数组，新数组的元素是对原始数组中的每个元素应用指定函数后的结果。

**语法：**

```js
array.map(callback(currentValue[, index[, array]])[, thisArg])
```

**参数：**

- ```
  callback
  ```

  ：一个函数，用于对数组的每个元素执行操作。该函数接受以下参数：

  - `currentValue`：当前正在处理的元素的值。
  - `index`（可选）：当前正在处理的元素的索引。
  - `array`（可选）：调用 `map` 方法的数组。

- `thisArg`（可选）：可选参数，用于在执行 `callback` 函数时指定 `this` 的值。

**返回值：**

- 一个新的数组，其中包含对原始数组中的每个元素应用 `callback` 函数后的结果。

**示例：**

```js
const numbers = [1, 2, 3, 4, 5];
const squaredNumbers = numbers.map(function (value) {
  return value * value;
});

console.log(squaredNumbers); // 输出 [1, 4, 9, 16, 25]
```

**使用箭头函数的示例：**

```js
const numbers = [1, 2, 3, 4, 5];
const squaredNumbers = numbers.map(value => value * value);

console.log(squaredNumbers); // 输出 [1, 4, 9, 16, 25]
```

**使用场景：**

- `map()` 方法常用于对数组中的每个元素进行某种操作，例如转换、过滤或提取。
- 它可以用来创建一个新的数组，而不影响原始数组。
- 适用于需要对数组中的每个元素应用相同操作的情况，例如将数组中的每个元素都平方、加倍、格式化等。

#### filter()创建满足指定条件新数组，长度改变

`filter()` 是 JavaScript 数组对象的一个方法，它用于创建一个新数组，其中包含满足指定条件的原始数组元素。`filter()` 方法不会修改原始数组，而是返回一个新的数组，其中包含满足条件的元素。

**语法：**

```js
array.filter(callback(element[, index[, array]])[, thisArg])
```

**参数：**

- ```js
  callback
  ```

  ：一个函数，用于对数组的每个元素进行测试。该函数接受以下参数：

  - `element`：当前正在测试的数组元素的值。
  - `index`（可选）：当前正在测试的数组元素的索引。
  - `array`（可选）：调用 `filter` 方法的数组。

- `thisArg`（可选）：可选参数，用于在执行 `callback` 函数时指定 `this` 的值。

**返回值：**

- 一个新的数组，其中包含满足 `callback` 函数条件的原始数组元素。原始数组不受影响。

**示例：**

```js
const numbers = [1, 2, 3, 4, 5];
const evenNumbers = numbers.filter(function (value) {
  return value % 2 === 0;
});

console.log(evenNumbers); // 输出 [2, 4]
```

**使用箭头函数的示例：**

```js
const numbers = [1, 2, 3, 4, 5];
const evenNumbers = numbers.filter(value => value % 2 === 0);

console.log(evenNumbers); // 输出 [2, 4]
```

**注意事项：**

- `filter()` 方法不会修改原始数组，它返回一个新的数组，其中包含满足条件的元素。
- 回调函数 `callback` 应该返回一个布尔值，用于确定元素是否应该包含在结果数组中。如果回调函数返回 `true`，则元素将包含在结果中，否则将被排除。
- 可以使用 `thisArg` 参数来指定回调函数中的 `this` 值。

#### join 数组转字符串

`join()` 是 JavaScript 数组对象的一个方法，它用于将数组中的所有元素连接成一个字符串，并返回这个字符串。

**语法：**

```js
array.join([separator])
```

**参数：**

- `separator`（可选）：指定用于分隔数组元素的字符串。如果不提供该参数，数组元素将默认使用逗号 `,` 分隔。

**返回值：**

- 一个字符串，其中包含数组中的所有元素，这些元素通过指定的分隔符连接起来。

**示例：**

```js
const fruits = ["apple", "banana", "cherry"];
const joinedString = fruits.join(", ");

console.log(joinedString); // 输出 "apple, banana, cherry"
```

**注意事项：**

- `join()` 方法不会修改原始数组，而是返回一个新的字符串。
- 如果数组中的元素是数字、布尔值或其他数据类型，它们会被自动转换为字符串形式，然后再连接起来。
- 如果不提供分隔符参数或分隔符参数为 `undefined`，数组中的元素将默认使用逗号 `,` 分隔。

#### concat 连接数组

concat() 方法用于连接两个或多个数组。

该方法不会改变现有的数组，而是返回一个新的数组。

#### splice 连续删除或替换

如果我们想删除数组中间的一个元素， 或者想一次删除多个元素，该如何操作呢？ 这时候我们就需要使用 `splice()` 方法了， `splice()` 可以让我们从数组中的任意位置**连续删除任意数量的元素**。

`splice()` 最多可以接受 3 个参数，但现在我们先关注前两个。 `splice()` 的前两个参数是整数，表示数组中调用 `splice()` 的项的索引或位置。 别忘了，数组的索引是*从 0 开始的*，所以我们要用 `0` 来表示数组中的第一个元素。 `splice()` 的第一个参数代表从数组中的哪个索引开始移除元素，而第二个参数表示要从数组中的这个位置开始删除多少个元素。 例如：

```js
let array = ['today', 'was', 'not', 'so', 'great'];

array.splice(2, 2);
```

这里我们移除 2 个元素，首先是第三个元素（索引为 2）。 `array` 会有值 `['today', 'was', 'great']`。

`splice()` 不仅会修改调用该方法的数组，还会返回一个包含被移除元素的数组：

```js
let array = ['I', 'am', 'feeling', 'really', 'happy'];

let newArray = array.splice(3, 2);
```

`newArray` 值为 `['really', 'happy']`。

还记得在上个挑战中我们提到 `splice()` 方法最多可以接收 3 个参数吗？ 第三个参数可以是**一个或多个元素**，这些元素会被添加到数组中。 **这样，我们能够便捷地将数组中的一个或多个连续元素换成其他的元素。**

```js
const numbers = [10, 11, 12, 12, 15];
const startIndex = 3;
const amountToDelete = 1;

numbers.splice(startIndex, amountToDelete, 13, 14);
console.log(numbers);
```

第二个 `12` 已被删除，我们在同一索引处添加 `13` 和 `14`。 `numbers` 数组现在将会是 `[ 10, 11, 12, 13, 14, 15 ]`。

在上面的代码中，数组一开始包含了若干数字。 接着，我们调用 `splice()` 方法，索引为 (3) 的地方开始删除元素，删除的元素数量是 (1)。然后，(13, 14) 是在删除位置插入的元素。 可以在 `amountToDelete` 后面传入任意数量的元素（以逗号分隔），每个都会被插入到数组中。

#### sort 排序

`sort()` 方法是 JavaScript 数组对象的一个方法，用于对数组元素进行排序。默认情况下，`sort()` 方法将元素视为字符串并按照 Unicode 码点顺序进行排序。以下是关于 `sort()` 方法的详细解释：

**语法：**

```js
array.sort([compareFunction])
```

**参数：**

- `compareFunction`（可选）：一个可选的函数，用于确定元素的顺序。如果省略该参数，元素将被转换为字符串并按照 Unicode 码点顺序进行排序。
  - 如果 `compareFunction` 函数返回负数，表示第一个元素应该在第二个元素之前。
  - 如果 `compareFunction` 函数返回正数，表示第一个元素应该在第二个元素之后。
  - 如果 `compareFunction` 函数返回 0，表示两个元素相等，它们的顺序不变。

**返回值：**

- 返回排序后的数组。请注意，`sort()` 方法会修改原始数组，同时返回排序后的数组。

**示例：**

```js
const fruits = ["apple", "banana", "cherry", "date"];

// 默认情况下，按照 Unicode 码点顺序排序
const sortedFruits = fruits.sort();
console.log(sortedFruits); // 输出 ["apple", "banana", "cherry", "date"]

// 使用自定义比较函数，按照字符串长度排序
const sortedByLength = fruits.sort((a, b) => a.length - b.length);
console.log(sortedByLength); // 输出 ["date", "apple", "cherry", "banana"]
```

**注意事项：**

- 默认情况下，`sort()` 方法会将元素视为字符串，并按照 Unicode 码点顺序进行排序。这意味着它可能不会按您期望的方式对数字进行排序。
- 如果需要自定义排序逻辑，可以传递一个比较函数给 `sort()` 方法。比较函数接受两个参数，分别是要比较的元素，您可以在函数中定义比较逻辑以返回正确的排序顺序。
- 在比较函数中，返回负数表示第一个元素应该在第二个元素之前，返回正数表示第一个元素应该在第二个元素之后，返回 0 表示两个元素相等。
- 请注意，`sort()` 方法会修改原始数组，同时返回排序后的数组。如果不希望修改原始数组，可以使用 `slice()` 方法创建原数组的副本，然后对副本数组使用 `sort()`。

#### findIndex 查找满足条件第一个元素

`findIndex()` 方法是 JavaScript 数组对象的方法，用于查找数组中满足特定条件的第一个元素，并返回该元素的索引。如果没有找到满足条件的元素，则返回 -1。以下是关于 `findIndex()` 方法的详细解释：

**语法：**

```js
array.findIndex(callback(element[, index[, array]])[, thisArg])
```

**参数：**

- ```js
  callback
  ```

  ：一个用于测试每个元素的函数，接受以下参数：

  - `element`：当前正在测试的数组元素的值。
  - `index`（可选）：当前正在测试的数组元素的索引。
  - `array`（可选）：调用 `findIndex` 方法的数组。

- `thisArg`（可选）：可选参数，用于在执行 `callback` 函数时指定 `this` 的值。

**返回值：**

- 如果找到满足 `callback` 函数条件的元素，则返回该元素的索引；如果没有找到，则返回 -1。

**示例：**

```js
const numbers = [1, 2, 3, 4, 5];

// 查找第一个大于 2 的元素的索引
const index = numbers.findIndex(function (value) {
  return value > 2;
});

console.log(index); // 输出 2，因为第一个大于 2 的元素是 3，它在数组中的索引是 2
```

**使用箭头函数的示例：**

```js
const numbers = [1, 2, 3, 4, 5];

// 查找第一个大于 2 的元素的索引
const index = numbers.findIndex(value => value > 2);

console.log(index); // 输出 2，因为第一个大于 2 的元素是 3，它在数组中的索引是 2
```

**注意事项：**

- `findIndex()` 方法在数组中查找满足条件的第一个元素，并返回该元素的索引。如果没有满足条件的元素，则返回 -1。
- `callback` 函数应该返回一个布尔值，用于确定元素是否满足条件。如果 `callback` 返回 `true`，则表示找到了满足条件的元素，`findIndex()` 返回该元素的索引。
- `findIndex()` 方法会遍历数组，从头开始查找，找到满足条件的元素后立即返回，不会继续查找下一个匹配的元素。
- 可以使用 `thisArg` 参数来指定回调函数中的 `this` 值。

### 字符串

#### parseInt 字符串转整数

`parseInt()` 函数解析一个字符串返回一个整数。 下面是一个示例：

```js
const a = parseInt("007");
```

上述函数将字符串 `007` 转换为整数 `7`。 如果字符串中的第一个字符不能转换为数字，则返回 `NaN`

它还可以传入**第二个参数**，指定了字符串中数字的基数(待转换的进制)。 基数可以是 2 到 36 之间的整数。

```js
parseInt(string, radix);

const a = parseInt("11", 2);
```

变量 radix 表示 `11` 是在二进制系统中。 这个示例将字符串 `11` 转换为整数 `3`。

#### split 分割字符串

**String.prototype.split()方法通过传入的参数符号，将一个字符串对象分割成一个字符串数组，这个传入的参数符号决定着字符串在什么位置进行分割。**

```js
str.split([separator[, limit]])
```

**参数**

**separator | 可选**
该参数指定字符串在什么地方进行分割。separator参数可以是一个字符串或者一个正则表达式。如果separator是一个字符串，那在分割时必须完全匹配separator字符串才执行分割。如果separator参数没有传或者separator参数没有在被调用的字符串中匹配到，返回值将是一个包含整个被调用字符串的数组。如果separator**参数是空字符串，split()函数将会把被调用字符串分割成一个个字符组成的数组**。
**limit | 可选**
limit参数是一个整数，他决定了split()函数的分割次数。如果limit参数存在，返回的数组的数组长度将小于等于limit。如果在分割时还有未分割完的字符串，但是数组长度已经到达了limit的值，那么剩余的字符串将被丢弃，不会在结果中返回。

#### toUpperCase 字母转大写

- `toUpperCase()` 方法不会修改原始字符串，而是返回一个新的字符串，其中的字母都是大写的。
- 对于非字母字符（如数字、标点符号和空格），`toUpperCase()` 方法不会进行任何变化，**它只会影响字母字符**。
- 该方法不会更改原始字符串的长度，只是改变字符的大小写。

#### toLowerCase 字母转小写.

- `toLowerCase()` 方法不会修改原始字符串，而是返回一个新的字符串，其中的字母都是小写的。
- 对于非字母字符（如数字、标点符号和空格），`toLowerCase()` 方法不会进行任何变化，**它只会影响字母字符**。
- 该方法不会更改原始字符串的长度，只是改变字符的大小写。

#### replace 字符串中查找并替换

**语法：**

```js
string.replace(searchValue, replaceValue)
string.replace(regexp, replaceValue)
```

**参数：**

- `searchValue`：要查找和替换的子字符串或正则表达式。
- `regexp`：一个正则表达式，用于查找和替换匹配的部分。
- `replaceValue`：用于替换匹配项的字符串或一个替换函数。替换函数接受多个参数，包括匹配项、匹配项的偏移量和原始字符串。

**返回值：**

- 一个新的字符串，其中匹配的子字符串被替换为指定的值。

**示例：**

```js
const originalString = "Hello, World!";
const replacedString = originalString.replace("World", "JavaScript");

console.log(replacedString); // 输出 "Hello, JavaScript!"
```

**正则表达式示例：**

```js
const text = "The quick brown fox jumps over the lazy dog";
const replacedText = text.replace(/brown|lazy/g, "red");

console.log(replacedText); // 输出 "The quick red fox jumps over the red dog"
```

**替换函数示例：**

```js
const text = "The quick brown fox jumps over the lazy dog";
const replacedText = text.replace(/brown|lazy/g, (match, offset, original) => {
  return match === "brown" ? "red" : "active";
});

console.log(replacedText); // 输出 "The quick red fox jumps over the active dog"
```

**注意事项：**

- `replace()` 方法不会修改原始字符串，而是返回一个新的字符串。
- 如果 `searchValue` 或 `regexp` 匹配多个项，只有第一个匹配项会被替换。要替换所有匹配项，可以使用正则表达式中的全局标志（例如 `/g`）。
- 如果 `searchValue` 或 `regexp` 匹配不到任何项，原始字符串将保持不变。
- 如果 `replaceValue` 是一个函数，该函数可以灵活地根据匹配项的内容和位置来确定替换的值。

`replace()` 方法在字符串处理和文本替换方面非常常用，可以用于替换文本中的特定部分，例如搜索和替换操作、文本清理、模板替换等。

### 数组、字符串共有

#### length

你可以通过在字符串变量或字符串或数组后面写上 `.length` 来获得其长度。

```javascript
console.log("Alan Peter".length);
```

值 `10` 将显示在控制台中。 请注意，“Alan” 和 “Peter” 之间的空格字符也被计算在内。**注意：** 数组名与方括号之间不应该有任何空格，比如 `array [0]` 。 尽管 JavaScript 能够正确处理这种情况，但是当其他程序员阅读你写的代码时，这可能让他们感到困惑

#### indexOf 查询元素位置

JavaScript 为我们提供了内置方法 `indexOf()`。 这个方法让我们可以方便地检查某个元素是否存在于数组/字符串中。 `indexOf()` 方法接受一个元素作为输入参数，并返回该元素在数组/字符串中的位置（索引）；若该元素不存在于数组中则返回 `-1`。

```js
let fruits = ['apples', 'pears', 'oranges', 'peaches', 'pears'];

fruits.indexOf('dates');
fruits.indexOf('oranges');
fruits.indexOf('pears');
```

`indexOf('dates')` 返回 `-1`，`indexOf('oranges')` 返回 `2`，`indexOf('pears')` 返回 `1` (每个元素存在的第一个索引)。

#### slice 截取子数组/子字符串

**`slice()`** 方法返回一个新的数组对象，这一对象是一个由 `start` 和 `end` 决定的原数组的[浅拷贝](https://developer.mozilla.org/zh-CN/docs/Glossary/Shallow_copy)（包括 `start`，不包括 `end`），其中 `start` 和 `end` 代表了数组元素的索引。原始数组不会被改变。

**js中 [].slice 与 Array.prototype.slice 有什么区别?**

> 主要是this指向不同，详见https://www.zhihu.com/question/46724226

`slice()` 只接收 2 个输入参数：**第一个是开始提取元素的位置（索引）（默认为0）**，第二个是提取元素的结束位置（索引）**（默认为结尾）**。 提取的元素中不包括第二个参数所对应的元素。 如下示例：

```js
let weatherConditions = ['rain', 'snow', 'sleet', 'hail', 'clear'];

let todaysWeather = weatherConditions.slice(1, 3);
```

`todaysWeather` 值为 `['snow', 'sleet']`，`weatherConditions` 值仍然为 `['rain', 'snow', 'sleet', 'hail', 'clear']`。

在上面的代码中，我们从一个数组中提取了一些元素，并用这些元素创建了一个新数组。

**JavaScript 中的字符串有一个 `slice()` 方法**，它用于提取字符串中的一部分，并返回一个新的字符串，而不修改原始字符串。`slice()` 方法接受两个参数，分别是开始位置和结束位置（可选）。它的基本语法如下：

```js
string.slice(start)
string.slice(start, end)
```

- `start`：要提取的子字符串的起始位置（包含该位置的字符）。
- `end`：要提取的子字符串的结束位置（不包含该位置的字符）。`end` 参数是可选的，如果未提供，`slice()` 会提取从 `start` 位置到字符串的末尾的所有字符。

如果 `start` 参数为负数，则它将被视为从字符串末尾开始的位置，例如 `-1` 表示倒数第一个字符。

```js
const str = "Hello, World!";

const sliced3 = str.slice(-6); // 从字符串末尾开始提取的倒数第 6 个字符到末尾
console.log(sliced3); // 输出 "World!"
```

`slice()` 方法在字符串操作中非常有用，可以用于提取子字符串，例如截取文件扩展名、截取 URL 中的路径等等。

### 其他方法

#### typeof 验证类型

```javascript
if(typeof myGlobal != "undefined"){
    
}
```

**提示** 在 JavaScript 中，你可以使用 `typeof` 运算符确定变量或值的类型，如下所示：

```js
typeof 3
typeof '3'
```

`typeof 3` 返回字符串 `number`，`typeof '3'` 返回字符串 `string`。

#### Math.random 随机数

在 JavaScript 中，可以用 `Math.random()` 生成一个在`0`（包括 0）到 `1`（不包括 1）之间的随机小数。 因此 `Math.random()` 可能返回 `0`，但绝不会返回 `1`。

#### Math.floor 向下取整

`Math.floor()` 向下取整，获得它最近的整数。

#### freeze 保护对象不会改变

`const` 声明并不会真的保护数据不被改变(它只是不能改变对象的地址，里面数据是可以变的，例如const数组)。 为了确保数据不被改变，JavaScript 提供了一个函数 `Object.freeze`

任何更改对象的尝试都将被拒绝，如果脚本在严格模式下运行，将抛出错误。

```js
let obj = {
  name:"FreeCodeCamp",
  review:"Awesome"
};
Object.freeze(obj);
obj.review = "bad";
obj.newProp = "Test";
console.log(obj); 
```

`obj.review` 和 `obj.newProp` 赋值将导致错误，因为我们的编辑器默认在严格模式下运行，控制台将显示值 `{ name: "FreeCodeCamp", review: "Awesome" }`。

#### Object.keys(obj) 返回对象所有键的数组

使用如下：

```js
let users = {
  Alan: {
    age: 27,
    online: false
  },
  Jeff: {
    age: 32,
    online: true
  },
  Sarah: {
    age: 48,
    online: false
  },
  Ryan: {
    age: 19,
    online: true
  }
};

function getArrayOfUsers(obj) {
  return Object.keys(obj);
}
console.log(getArrayOfUsers(users));
```

```json
[ 'Alan', 'Jeff', 'Sarah', 'Ryan' ]
```

## js题目

### 逆转字符串

反转提供的字符串并返回反转后的字符串。

例如， `"hello"` 应该变成 `"olleh"`。

**注意：**js中string为不可变对象，所以没办法直接修改，重新声明一个字符串，然后用+号连接。

```js
function reverseString(str) {
    let returnStr="";
    for(let i=str.length-1;i>=0;i--){
        returnStr+=str[i];
    }
    return returnStr;
}

console.log(reverseString("hello"));
```

或者调用函数来解决

```js
function reverseString(str) {
  return str
    .split("")
    .reverse()
    .join("");
}
```

代码说明
我们的目标是获取输入 str 并反向返回它。

 我们的第一步是使用 split("") 按字符拆分字符串。 请注意，我们在单引号之间没有留下任何内容，这告诉函数按每个字符分割字符串。

使用 split() 函数会将字符串转换为字符数组，在我们继续前进时请记住这一点。

接下来我们链接reverse()函数，它获取我们的字符数组并反转它们。

最后，我们链接 join("") 将字符重新组合成一个字符串。 再次注意，我们在连接参数中没有留下空格，这确保了字符数组由每个字符重新连接在一起。

### 字符串中最长的子字符串

简单写法

```js
function findLongestWordLength(str) {
    const returnStr=str.split(" ");
    let sum=0;
    for(let i=0;i<returnStr.length;i++){
        if(returnStr[i].length>sum){
            sum=returnStr[i].length;
        }
    }
    return sum;
}
```

复杂写法,留到以后学了相关知识再模仿

```js
function findLongestWordLength(s) {
  return s
    .split(' ')
    .reduce((longest, word) => Math.max(longest, word.length), 0);
}
```

```js
function findLongestWordLength(str) {
  return Math.max(...str.split(" ").map(word => word.length));
}
```

### 找出多个数组中的最大数字

简单写法

```js
function largestOfFour(arr) {
    let sum;
    const returnArr=[];
    for(let i=0;i<arr.length;i++){
        sum=arr[i][0];
        for(let j=0;j<arr[i].length;j++){
            if(arr[i][j]>sum){
                sum=arr[i][j];
            }
        }
        returnArr.push(sum);
    }
    return returnArr;
}
```

复杂写法,留到以后学了相关知识再模仿

```js
function largestOfFour(arr) {
  return arr.map(function(group) {
    return group.reduce(function(prev, current) {
      return current > prev ? current : prev;
    });
  });
}
```

```js
function largestOfFour(arr) {
  return arr.map(Function.apply.bind(Math.max, null));
}
```

```js
function largestOfFour(arr, finalArr = []) {
  return !arr.length
    ? finalArr
    : largestOfFour(arr.slice(1), finalArr.concat(Math.max(...arr[0])))
}
```

### 确认结尾子字符串

检查字符串（第一个参数 `str`）是否以给定的目标字符串（第二个参数 `target`）结束。

第一时间想到正则化，但可惜需要new来动态创建，才能用变量形式使用，之后学习一下

```js
function confirmEnding(str, target) {
    target=target+"$";
    let regex=new RegExp(target);
    return regex.test(str);
}

confirmEnding("Bastian", "n");
```

或者利用slice方法,因为没给end参数，所以slice从target位置到末尾再和target比较即可

```js
function confirmEnding(str, target) {
  // "Never give up and good luck will find you."
  // -- Falcor

  return str.slice(str.length - target.length) === target;
}

confirmEnding("He has to give me a new name", "name");
```

当然也可使用负数，从后往前截取

```js
function confirmEnding(str, target) {
  return str.slice(-target.length) === target
}

confirmEnding("Bastian", "n");
```

### Slice 与 Splice

本挑战的输入参数为两个数组和一个索引值。

将第一个数组中的所有元素依次复制到第二个数组中。

请注意，你需要从第二个数组索引值为 `n` 的地方开始插入。

最后，请返回插入元素后的数组。 作为输入参数的两个数组在函数执行前后应保持不变。

简单解法

```js
function frankenSplice(arr1, arr2, n) {
    const arr=[];
    let i=0;
    for(i=0;i<n;i++){
        arr.push(arr2[i]);
    }
    for(let j=0;j<arr1.length;j++){
        arr.push(arr1[j]);
    }
    for(;i<arr2.length;i++){
        arr.push(arr2[i]);
    }
    return arr;
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

高级

```js
function frankenSplice(arr1, arr2, n) {
  // It's alive. It's alive!
  let localArray = arr2.slice();
  for (let i = 0; i < arr1.length; i++) {
    localArray.splice(n, 0, arr1[i]);
    n++;
  }
  return localArray;
}

function frankenSplice(arr1, arr2, n) {
  // It's alive. It's alive!
  let localArr = arr2.slice();
  localArr.splice(n, 0, ...arr1);
  return localArr;
}

function frankenSplice(arr1, arr2, n) {
  return [...arr2.slice(0, n), ...arr1, ...arr2.slice(n)];
}
```

### 过滤数组中的假值

从数组中移除所有假值（falsy values）。 返回一个新数组；不要改变原始数组。

JavaScript 中的假值有 `false`、`null`、`0`、`""`、`undefined`、`NaN`。

提示：可以考虑将每个值都转换为布尔值（boolean）。

```js
function bouncer(arr) {
  const returnArr=[];
  for(let i=0;i<arr.length;i++){
    if(arr[i]){
      returnArr.push(arr[i]);
    }
  }
  return returnArr;
}

bouncer([7, "ate", "", false, 9]);
```

高级

```js
function bouncer(arr) {
  return arr.filter(Boolean);
}
```

### 找出元素在排序后数组中的索引

数组（第一个参数）在排序后，将一个值（第二个参数）插入该数组，并使数组保持有序。返回这个新插入元素的最小索引值。 返回值应为一个数字。

例如，`getIndexToIns([1,2,3,4], 1.5)` 应该返回 `1` 因为1.5 大于 `1`（索引为 0）且小于 `2`（索引为 1）。

同样地，`getIndexToIns([20,3,5], 19)` 应该返回 `2`。 因为数组排序后会变成 `[3,5,20]`，而 `19` 小于 `20`（索引为 2）且大于 `5`（索引为 1）。

```js
function getIndexToIns(arr, num) {
    if(arr.length==0){
        return 0;
    }
    for(let i=0;i<arr.length-1;i++){
        for(let j=0;j<arr.length-1-i;j++){
            if(arr[j]>arr[j+1]){
                [arr[j],arr[j+1]]=[arr[j+1],arr[j]];
            }
        }
    }
    for(let i=0;i<arr.length;i++){
        if(arr[i]>=num){
            arr.splice(i,0,num);
            return i;
        }
    }
    arr.push(num);
    return arr.length-1;
}

getIndexToIns([40, 60], 50);
```

高级

```js
function getIndexToIns(arr, num) {//值得学习
  arr.sort((a, b) => a - b);//排序
  for (let i = 0; i < arr.length; i++) {//找位置
    if (arr[i] >= num) return i;
  }
  return arr.length;
}

function getIndexToIns(arr, num) {//未排序，仅仅是返回了小于num的数组元素总长度
  return arr.filter(val => num > val).length;
}

function getIndexToIns(arr, num) {//先排序，再找位置 值得学习
  // sort and find right index
  let index = arr
    .sort((curr, next) => curr - next)
    .findIndex(currNum => num <= currNum);
  // Returns index or total length of arr
  return index === -1 ? arr.length : index;
}

function getIndexToIns(arr, num) {//先加入，再排序，返回num所在位置 值得学习
  return arr
    .concat(num)
    .sort((a, b) => a - b)
    .indexOf(num);
}
```

