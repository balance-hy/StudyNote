# Python

## 输入输出

Python 提供了 [input() 内置函数](https://www.runoob.com/python3/python3-func-input.html)从标准输入读入一行文本，默认的标准输入是键盘。

```python
name = input()
```

用`print()`在括号中加上字符串，就可以向屏幕上输出指定的文字。比如输出`'hello, world'`，用代码实现如下：

```python
print('hello, world')
```

下面的代码读入用户输入的值，并打印输出

```py
name = input()
print(name)
```

**注意**：`input()`**返回的数据类型是`str`**，`str`不能直接和整数比较，必须先把`str`转换成整数。Python提供了`int()`函数来完成这件事情，`int()`函数发现一个字符串并不是合法的数字时就会报错

## 缩进问题

Python的语法比较简单，采用缩进方式，写出来的代码就像下面的样子：

```py
# print absolute value of an integer:
a = 100
if a >= 0:
    print(a)
else:
    print(-a)
```

**以`#`开头的语句是注释**，注释是给人看的，可以是任意内容，解释器会忽略掉注释。其他每一行都是一个语句，当语句以冒号`:`结尾时，缩进的语句视为代码块。

缩进有利有弊。好处是强迫你写出格式化的代码，但没有规定缩进是几个空格还是Tab。**按照约定俗成的惯例，应该始终坚持使用*4个空格*的缩进。**

## 基本数据类型

### 整数

Python可以处理任意大小的整数，当然包括负整数，在程序中的表示方法和数学上的写法一模一样，例如：`1`，`100`，`-8080`，`0`，等等。

计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便，**十六进制用`0x`前缀**和0-9，a-f表示，例如：`0xff00`，`0xa5b4c3d2`，等等。

**对于很大的数，例如`10000000000`，很难数清楚0的个数。Python允许在数字中间以`_`分隔**，因此，写成`10_000_000_000`和`10000000000`是完全一样的。十六进制数也可以写成`0xa1b2_c3d4`

### 浮点数

浮点数也就是小数，之所以称为浮点数，是因为按照科学记数法表示时，一个浮点数的小数点位置是可变的，比如，1.23x109和12.3x108是完全相等的。

**浮点数可以用数学写法，如`1.23`，`3.14`，`-9.01`，等等。但是对于很大或很小的浮点数，就必须用科学计数法表示，把10用e替代，1.23x109就是`1.23e9`，或者`12.3e8`，0.000012可以写成`1.2e-5`，等等。**

### 字符串

字符串是**以单引号`'`或双引号`"`括起来的任意文本**，如果`'`本身也是一个字符，那就可以用`""`括起来，比如`"I'm OK"`包含的字符是`I`，`'`，`m`，空格，`O`，`K`这6个字符。

如果字符串内部既包含`'`又包含`"`怎么办？可以用转义字符`\`来标识，比如：

```
'I\'m \"OK\"!'
```

表示的字符串内容是：

```
I'm "OK"!
```

**转义字符`\`可以转义很多字符，比如`\n`表示换行，`\t`表示制表符，字符`\`本身也要转义，所以`\\`表示的字符就是`\`**

```py
>>> print('I\'m ok.')
I'm ok.
>>> print('I\'m learning\nPython.')
I'm learning
Python.
>>> print('\\\n\\')
\
\
```

如果字符串里面有很多字符都需要转义，就需要加很多`\`，为了简化，**Python还允许用`r''`表示`''`内部的字符串默认不转义**

```py
print('\\\t\\')
print(r'\\\t\\')
```

```
\	\
\\\t\\
```

**如果字符串内部有很多换行，用`\n`写在一行里不好阅读，为了简化，Python允许用`'''...'''`的格式表示多行内容**

```py
print('''line1
line2
line3''')
```

```
line1
line2
line3
```

### 布尔值

在Python中，可以直接用`True`、`False`表示布尔值（请注意大小写）

布尔值可以用`and`、`or`和`not`运算。

`and`运算是与运算，只有所有都为`True`，`and`运算结果才是`True`

```py
>>> True
True
>>> False
False
>>> 3 > 2
True
>>> 3 > 5
False
```

`or`运算是或运算，只要其中有一个为`True`，`or`运算结果就是`True`

```py
>>> True or True
True
>>> True or False
True
>>> False or False
False
>>> 5 > 3 or 1 > 3
True
```

`not`运算是非运算，它是一个单目运算符，把`True`变成`False`，`False`变成`True`

```py
>>> not True
False
>>> not False
True
>>> not 1 > 2
True
```

### 空值

**空值是Python里一个特殊的值，用`None`表示**。`None`不能理解为`0`，因为`0`是有意义的，而`None`是一个特殊的空值。

### 变量

变量的概念基本上和初中代数的方程变量是一致的，只是在计算机程序中，变量不仅可以是数字，还可以是任意数据类型。

变量在程序中就是用一个变量名表示了，**变量名必须是大小写英文、数字和`_`的组合，且不能用数字开头**

### 常量

所谓常量就是不能变的变量，比如常用的数学常数π就是一个常量。**在Python中，通常用全部大写的变量名表示常量**：

```
PI = 3.14159265359
```

**但事实上`PI`仍然是一个变量，Python根本没有任何机制保证`PI`不会被改变**，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量`PI`的值，也没人能拦住你。

### 两种除法

在Python中，有两种除法，**一种除法是`/`**：

```
>>> 10 / 3
3.3333333333333335
```

**`/`除法计算结果是浮点数**，即使是两个整数恰好整除，结果也是浮点数：

```
>>> 9 / 3
3.0
```

**还有一种除法是`//`，称为地板除**，两个整数的除法仍然是整数：

```
>>> 10 // 3
3
```

你没有看错，整数的地板除`//`永远是整数，即使除不尽。要做精确的除法，使用`/`就可以。

**因为`//`除法只取结果的整数部分**，所以**Python还提供一个余数运算`%`**，可以**得到两个整数相除的余数**：

```
>>> 10 % 3
1
```

无论整数做`//`除法还是取余数，结果永远是整数，所以，整数运算结果永远是精确的。

## 字符串和编码

ASCII编码和Unicode编码的区别：ASCII编码是1个字节，而Unicode编码通常是2个字节。

- 字母`A`用ASCII编码是十进制的`65`，二进制的`01000001`；
- 字符`0`用ASCII编码是十进制的`48`，二进制的`00110000`，注意字符`'0'`和整数`0`是不同的；
- 汉字`中`已经超出了ASCII编码的范围，用Unicode编码是十进制的`20013`，二进制的`01001110 00101101`。
- 你可以猜测，如果把ASCII编码的`A`用Unicode编码，只需要在前面补0就可以，因此，`A`的Unicode编码是`00000000 01000001`。

如果统一成Unicode编码，乱码问题从此消失了。但是，如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。

**本着节约的精神，又出现了把Unicode编码转化为“可变长编码”的`UTF-8`编码**。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间：

**UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。**

在最新的**Python 3版本中，字符串是以Unicode编码的**，也就是说，Python的字符串支持多语言

在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言，例如：

```py
>>> print('包含中文的str')
包含中文的str
```

**对于单个字符的编码**，Python提供了**`ord()`函数获取字符的整数表示，`chr()`函数把编码转换为对应的字符**：

```py
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

如果知道字符的整数编码，还可以用十六进制这么写`str`，`\u`表示unicode编码：

```py
>>> '\u4e2d\u6587'
'中文'
4:0100
e:1110
2:0010
d:1101
```

两种写法完全是等价的。

**要计算`str`包含多少个字符，可以用`len()`函数**

```py
>>> len('ABC')
3
>>> len('中文')
2
```

**`len()`函数计算的是`str`的字符数，如果换成`bytes`，`len()`函数就计算字节数：**

```py
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

可见，1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。

**由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：**

```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

申明了UTF-8编码并不意味着你的`.py`文件就是UTF-8编码的，必须并且要确保**文本编辑器正在使用UTF-8 without BOM编码**：

### 网络传输

Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。

**Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示**：

```py
x = b'ABC'
```

要注意区分`'ABC'`和`b'ABC'`，前者是`str`，后者虽然内容显示得和前者一样，但`bytes`的每个字符都只占用一个字节。

#### encode str->bytes

以Unicode表示的`str`通过`encode()`方法可以编码为指定的`bytes`，例如：

```py
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

纯英文的`str`可以用`ASCII`编码为`bytes`，内容是一样的，含有中文的`str`可以用`UTF-8`编码为`bytes`。含有中文的`str`无法用`ASCII`编码，因为中文编码的范围超过了`ASCII`编码的范围，Python会报错。

在`bytes`中，无法显示为ASCII字符的字节，用`\x##`显示。

#### decode bytes->str

反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是`bytes`。要把`bytes`变为`str`，就需要用`decode()`方法：

```py
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

如果`bytes`中包含无法解码的字节，`decode()`方法会报错：

```py
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果`bytes`中只有一小部分无效的字节，可以传入`errors='ignore'`忽略错误的字节：

```py
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

### 格式化字符串

在Python中，采用的格式化方式和C语言是一致的，用`%`实现，举例如下：

```py
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```

你可能猜到了，`%`运算符就是用来格式化字符串的。在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换，有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。

常见的占位符有：

| 占位符 | 替换内容     |
| ------ | ------------ |
| `%d`   | 整数         |
| `%f`   | 浮点数       |
| `%s`   | 字符串       |
| `%x`   | 十六进制整数 |

其中，格式化整数和浮点数还可以指定**是否补0**和**整数与小数的位数**

```py
print('%2d-%02d' % (3, 1))
```

`%2d`意味着整数至少占两位，不足前面加空格

`%02d`意味着整数至少占两位，不足前面加0

所以输出为

```
 3-01
```

对于小数的位数

```py
print('%.2f' % 3.1415926)
```

`%.2f`中的.2表示保留两位小数

```
3.14
```

如果你不太确定应该用什么，**`%s`永远起作用，它会把任何数据类型转换为字符串：**

```py
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
```

有些时候，**字符串里面的`%`是一个普通字符怎么办？这个时候就需要转义，用`%%`来表示一个`%`**：

```py
>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'
```

#### format()

另一种格式化字符串的方法是使用字符串的`format()`方法，它会用传入的参数依次替换字符串内的占位符`{0}`、`{1}`……，不过这种方式写起来比%要麻烦得多：

```py
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

#### f-string

最后一种格式化字符串的方法是使用以`f`开头的字符串，称之为`f-string`，它和普通字符串不同之处在于，字符串如果包含`{xxx}`，就会以对应的变量替换：

```py
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```

上述代码中，`{r}`被变量`r`的值替换，`{s:.2f}`被变量`s`的值替换，**并且`:`后面的`.2f`指定了格式化参数（即保留两位小数）**，因此，`{s:.2f}`的替换结果是`19.62`。

类似于js中`${xxx}`

## 列表

### list 可变的有序表

Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

比如，列出班里所有同学的名字，就可以用一个list表示：

```py
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
```

变量`classmates`就是一个list。**用`len()`函数可以获得list元素的个数**：

```py
>>> len(classmates)
3
```

用索引来访问list中每一个位置的元素，记得索引是从`0`开始的：

```py
>>> classmates[0]
'Michael'
>>> classmates[1]
```

如果要取最后一个元素，**除了计算索引位置外，还可以用`-1`做索引，直接获取最后一个元素：**

以此类推，可以获取倒数第2个、倒数第3个,也就是用负数下标

#### append

append 往list中追加元素到末尾

```py
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```

#### insert

insert 把元素插入到指定的位置，比如索引号为`1`的位置

```py
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

#### pop

pop 删除list末尾的元素

```py
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```

删除指定位置的元素，用`pop(i)`方法，其中`i`是索引位置

```py
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

### tuple 不可变有序表

另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改

**tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向`'a'`，就不能改成指向`'b'`，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！**

比如同样是列出同学的名字：

```py
>>> classmates = ('Michael', 'Bob', 'Tracy')
```

现在，**classmates这个tuple不能变了，它也没有append()，insert()这样的方法**。其他获取元素的方法和list是一样的，你可以正常地使用`classmates[0]`，`classmates[-1]`，**但不能赋值成另外的元素**。

定义一个tuple，**元素大于等于2**：

```py
>>> t = (1, 2)
>>> t
(1, 2)
```

定义一个**空的**tuple

```py
>>> t = ()
>>> t
()
```

**定义一个只有1个元素的tuple**

如果你这么定义：

```py
>>> t = (1)
>>> t
1
```

定义的不是tuple，是`1`这个数！这是因为括号`()`既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是`1`。

**所以，只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义**：

```py
>>> t = (1,)
>>> t
(1,)
```

Python在显示只有1个元素的tuple时，也会加一个逗号`,`，以免你误解成数学计算意义上的括号。

## 条件判断与循环

### if

`if`语句的完整形式

```
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

比如

```py
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:
    print('adult')
else:
    print('kid')
```

**注意上面的缩进以及语句没有括号以及语句结尾的冒号**

### for in

for...in循环，依次把list或tuple中的每个元素迭代出来，看例子：

```py
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

执行这段代码，会依次打印`names`的每一个元素：

```py
Michael
Bob
Tracy
```

所以`for x in ...`循环就是把每个元素代入变量`x`，然后执行缩进块的语句。

Python提供一个**`range()`函数，可以生成一个整数序列**，再通过**`list()`函数可以转换为list**。比如`range(5)`生成的序列是从0开始小于5的整数：

```py
>>> list(range(5))
[0, 1, 2, 3, 4]
```

### while

while循环，只要条件满足，就不断循环，条件不满足时退出循环

比如我们要计算100以内所有奇数之和，可以用while循环实现：

```py
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```

在循环内部变量`n`不断自减，直到变为`-1`时，不再满足while条件，循环退出。

**注意条件无括号，语句结尾的冒号**

在循环中，`break`语句可以提前退出循环。

在循环过程中，也可以通过`continue`语句，跳过当前的这次循环，直接开始下一次循环。

### match 类似于switch

使用`match`语句时，我们依次用`case xxx`匹配，并且可以在最后（且仅能在最后）加一个`case _`表示“任意值”

```py
age = 15

match age:
    case x if x < 10:
        print(f'< 10 years old: {x}')
    case 10:
        print('10 years old.')
    case 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18:
        print('11~18 years old.')
    case 19:
        print('19 years old.')
    case _:
        print('not sure.')
```

在上面这个示例中，第一个`case x if x < 10`表示当`age < 10`成立时匹配，且赋值给变量`x`，第二个`case 10`仅匹配单个值，第三个`case 11|12|...|18`能匹配多个值，用`|`分隔。

**`match`语句还可以匹配列表**

我们假设用户输入了一个命令，用`args = ['gcc', 'hello.c']`存储，下面的代码演示了如何用`match`匹配来解析这个列表：

```py
args = ['gcc', 'hello.c', 'world.c']
# args = ['clean']
# args = ['gcc']

match args:
    # 如果仅出现gcc，报错:
    case ['gcc']:
        print('gcc: missing source file(s).')
    # 出现gcc，且至少指定了一个文件:
    case ['gcc', file1, *files]:
        print('gcc compile: ' + file1 + ', ' + ', '.join(files))
    # 仅出现clean:
    case ['clean']:
        print('clean')
    case _:
        print('invalid command.')
```

第一个`case ['gcc']`表示列表仅有`'gcc'`一个字符串，没有指定文件名，报错；

第二个`case ['gcc', file1, *files]`表示列表第一个字符串是`'gcc'`，第二个字符串绑定到变量`file1`，后面的任意个字符串绑定到`*files`（符号`*`的作用将在[函数的参数](https://www.liaoxuefeng.com/wiki/1016959663602400/1017261630425888)中讲解），它实际上表示至少指定一个文件；

第三个`case ['clean']`表示列表仅有`'clean'`一个字符串；

最后一个`case _`表示其他所有情况。

## dict 和 set

### dict 即map

Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

用Python写一个dict如下：

```py
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```

当构建好dict，可以直接添加元素，通过指定键值的形式

```py
>>> d['Adam'] = 67
>>> d['Adam']
67
```

**如何检查key是否在dict中呢？**

方式一：通过`in`判断key是否存在

```py
>>> 'Thomas' in d
False
```

方式二：通过dict提供的`get()`方法，如果key不存在，可以返回`None`，或者自己指定的value

```py
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

注意：返回`None`的时候Python的交互环境不显示结果。

**如何删除key呢**

删除一个key，用`pop(key)`方法，对应的value也会从dict中删除

```py
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}
```

请务必注意，dict内部存放的顺序和key放入的顺序是没有关系的。

和list比较，dict有以下几个特点：

- 查找和插入的速度极快，不会随着key的增加而变慢；
- 需要占用大量的内存，内存浪费多。

而list相反：

- 查找和插入的时间随着元素的增加而增加；
- 占用空间小，浪费内存很少。

所以，**dict是用空间来换取时间**的一种方法。

### set

set和dict类似，也是一组**key的集合，但不存储value**。由于key不能重复，所以，**在set中，没有重复的key**。

**要创建一个set，需要提供一个list作为输入集合：**

```py
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

注意，传入的参数`[1, 2, 3]`是一个list，而显示的`{1, 2, 3}`只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的。

重复元素在set中自动被过滤：

```py
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
```

**add方法向set中添加元素，可以重复添加，但不会有效果**

```py
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

**通过`remove(key)`方法可以删除元素：**

```py
>>> s.remove(4)
>>> s
{1, 2, 3}
```

**set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：**

```PY
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

