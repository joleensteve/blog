**版本**
```bash
python -V
3.10.3
```

#### 一、运算符

1. `+` 加
   - 注意：字符串相加为合并 `'a' + 'b' = 'ab'`
2. `-` 减
3. `*` 乘
   - 注意：数字 \* 字符串表示重复 `3 * 'a' = 'aaa'`
4. `/` 除
5. `//` 整除
6. `%` 求余
7. `**` 乘方 如:  `5 ** 2` 即 5 的平方
8. `=` 赋值

#### 二、数据类型

1. 字符串
   - `\` 转义字符
   - `\n` 换行
   - 不区分单引号和双引号
   - `r` 强制字符串内不使用转义字符 `r'hello \name'` 输出后即是 `'hello \name'` `\n` 在此不会转义成换行
   - `"""..."""` 和 ` '''...'''` 表示多行
   - 相邻字符串自动合并 `'a' 'b'` 会自动合成为 `'ab'`
   - 支持索引访问 `word = 'abc'` -> `word[0] 为 a `
   - 索引支持负数，负数从右向左数 `word[-1] 为 c`
   - 索引越界会报错
   - 支持切片 `word[0:2]`
   - 切片越界会自动处理
   - `len(s)` s 字符串的长度
   
2. 列表
   - `li = [1,2,3]` 声明
   
   - 支持索引和切片
   
   - `li.append(4)` 追加元素
   
   - 为切片赋值可以改变列表大小，甚至清空整个列表
     - `li[1:2] = [12,13] -> [1,12,13,4]`
     - `li[:] = [] -> []`
     
   - `len(li)` li 大小
   
   - 堆栈的实现可用列表自有 append() pop() 实现
   
   - 队列最好用 collections.deque
     ```python
     from collections import deque
     queue = deque(["Eric", "John", "Michael"])
     queue.append("Terry")
     queue.popleft()
     ```
     
   - 使用列表推导式生成列表
     
     ```python
     # 例1、以创建平方值为例子
     squares = []
     for x in range(10):
     	squares.append(x**2)
     # 0 1 4 9 16 25 36 49 64 81
     # 但是 x 变量在循环后依然存在，所以一般用下面的形式
     squares = list(map(lambda x: x**2, range(10)))
     # 上面等价于：
     squares = [x**2 for x in range(10)]
     
     # 例2、将两个列表中不相等的元素组合起来
     ne = [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
     # [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
     
     # 例2、嵌套推导式
     matrix = [[1, 2, 3, 4],[5, 6, 7, 8],[9, 10, 11, 12],]
     m = [[row[i] for row in matrix] for i in range(4)]
     # [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
     # 使用内置函数 zip，更为简单
     m = list(zip(*matrix))
     ```
     
   - del 语句
     
     ```python
     # 语句按索引，而不是值从列表中移除元素。与返回值的 pop() 方法不同， del 语句也可以从列表中移除切片，或清空整个列表（之前是将空列表赋值给切片）
     a = [-1, 1, 66.25, 333, 333, 1234.5]
     del a[0]
     del a[2:4]
     del a[:]
     # 甚至可以直接删除变量 此后，再引用 a 就会报错
     del a
     ```
   
3. 元组
   元组由多个用逗号隔开的值组成并有`()`包裹, 元组与列表很像，但使用场景不同，用途也不同。元组是 [immutable](https://docs.python.org/zh-cn/3/glossary.html#term-immutable) （不可变的），一般可包含异质元素序列，通过解包或索引访问。列表是 [mutable](https://docs.python.org/zh-cn/3/glossary.html#term-mutable) （可变的），列表元素一般为同质类型，可迭代访问。
   
   构造 0 个或 1 个元素的元组比较特殊：为了适应这种情况，对句法有一些额外的改变。用一对空圆括号就可以创建空元组；只有一个元素的元组可以通过在这个元素后添加逗号来构建（圆括号里只有一个值的话不够明确）。丑陋，但是有效。例如：
   
   ```python
   empty = ()
   singleton = 'hello'
   ```
   
4. 集合（类似于 java 的set）
   
   **集合是由不重复元素组成的无序容器**。基本用法包括成员检测、消除重复元素。集合对象支持合集、交集、差集、对称差分等数学运算。
   
   创建集合用花括号或 `set()` 函数。注意，创建空集合只能用 `set()`，不能用 `{}`，`{}` 创建的是空字典
   
5. 字典 （java map）

#### 三、流程控制

1. `if` 条件控制语句

   ```python
   if a < 0:
   	print("")
   elif a == 0:
   	print("")
   else:
   	print("")
   ```

2. `for` 循环语句

   ```python
   # 经典 for 循环
   for n in list:
   	print(n)
     
   # 循环对象
   knights = {'gallahad': 'the pure', 'robin': 'the brave'}
   for k, v in knights.items():
   	print(k, v)
     
   # 使用 enumerate 函数
   for i, v in enumerate(['tic', 'tac', 'toe']):
     print(i, v)
     
   # 同时循环多列表
   questions = ['name', 'quest', 'favorite color']
   answers = ['lancelot', 'the holy grail', 'blue']
   for q, a in zip(questions, answers):
   	print('What is your {0}?  It is {1}.'.format(q, a))
     
   # range len 函数组合循环
   a = ['Mary', 'had', 'a', 'little', 'lamb']
   for i in range(len(a)):
   	print(i, a[i])
     
   # 逆序循环
   a = ['Mary', 'had', 'a', 'little', 'lamb']
   for i in reversed(range(len(a))):
   	print(i, a[i])
     
   # 按指定顺序循环序列，可以用 sorted() 函数，在不改动原序列的基础上，返回一个重新的序列
   basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
   for i in sorted(basket):
   	print(i)
     
   # 使用 set() 去除序列中的重复元素。使用 sorted() 加 set() 则按排序后的顺序，循环遍历序列中的唯一元素
   basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
   for f in sorted(set(basket)):
   	print(f)
   ```

   遍历集合时修改集合内容，会很容易生成错误的结果。因此不能直接进行循环，而是应遍历该集合的副本或创建新的集合：

   ```python
   # Create a sample collection
   users = {'Hans': 'active', 'Éléonore': 'inactive', '景太郎': 'active'}
   
   # Strategy:  Iterate over a copy
   for user, status in users.copy().items():
       if status == 'inactive':
           del users[user]
   
   # Strategy:  Create a new collection
   active_users = {}
   for user, status in users.items():
       if status == 'active':
           active_users[user] = status
   ```

   `break` `continue` 与其他语言类似，中断最近的循环体/继续下一次循环

   **最后注意： python 中 循环后可以接 else 语句**

   ```python
   # for 循环中，可迭代对象中的元素全部循环完毕, 会执行 else 语句。但是走 break 语句，就不会再执行 else 语句了
   for x in range(10):
     if x == 0:
       print("xxx")
       break
   else:
     print('yyy')
   
   # while	条件为假时 会执行 else 语句，同 for 如果走 break 语句，就不会再执行 else 语句了
   a = 1
   while (a < 10):
     print(a)
     a++
   else:
     print("a>=10")
   ```


3. match 语句

   ```python
   # 演示代码
   def http_error(status):
       match status:
           case 400:
               return "Bad request"
           case 401 | 402 | 403 | 404:
               return "Not found"
           case 418:
               return "I'm a teapot"
           case _:
               return "Something's wrong with the internet"
             
   # 支持元组
   # point is an (x, y) tuple
   match point:
       case (0, 0):
           print("Origin")
       case (0, y):
           print(f"Y={y}")
       case (x, 0):
           print(f"X={x}")
       case (x, y):
           print(f"X={x}, Y={y}")
       case _:
           raise ValueError("Not a point")
   # 支持类
   class Point:
       x: int
       y: int
   
   def where_is(point):
       match point:
           case Point(x=0, y=0):
               print("Origin")
           case Point(x=0, y=y):
               print(f"Y={y}")
           case Point(x=x, y=0):
               print(f"X={x}")
           case Point():
               print("Somewhere else")
           case _:
               print("Not a point")
   
   # 支持列表
   points = [Point(0, 0), Point(1, 2)]
   match points:
       case []:
           print("No points")
       case [Point(0, 0)]:
           print("The origin")
       case [Point(x, y)]:
           print(f"Single point {x}, {y}")
       case [Point(0, y1), Point(0, y2)]:
           print(f"Two on the Y axis at {y1}, {y2}")
       case _:
           print("Something else")
           
   # 结合 if 为模式添加成为守护项的 if 子句。如果守护项的值为假，则 match 继续匹配下一个 case 语句块。注意，值的捕获发生在守护项被求值之前
   match point:
       case Point(x, y) if x == y:
           print(f"Y=X at {x}")
       case Point(x, y):
           print(f"Not on the diagonal")
           
   # 支持常量  但命名常量必须为带点号的名称以防止它们被解读为捕获变量
   from enum import Enum
   class Color(Enum):
       RED = 'red'
       GREEN = 'green'
       BLUE = 'blue'
   
   color = Color(input("Enter your choice of 'red', 'blue' or 'green': "))
   
   match color:
       case Color.RED:
           print("I see red!")
       case Color.GREEN:
           print("Grass is green")
       case Color.BLUE:
           print("I'm feeling the blues :(")
   ```

4. 函数

   ```python
   # 声明
   def funcName(a):
   	a += 10
   	return a 
   ```

   **参数**

   ```python
   # 声明-1 name 为必须参数，age 和 say 有默认值为可选参数
   def hello(name, age=10, say='good'):
     	print(f'{name} 年龄为 {10} 他喜欢说：{say}')
   # 调用函数可使用关键字参数,如下调用皆合法
   hello(name="lili")
   hello(age=20,name="lili")
   
   # 声明-2 * 代表可接受一个列表，** 代表可接受一个字典。两种参数同时存在 ** 只能放在 * 后面 
   def	hello(name, *likeSay, **other):
     pass
   # 调用
   hello("lili", "你好","hah", age=10, weight=100)
   
   # 声明-3 特殊参数 / 之前的参数只能按顺序传参
   def	hello(name, age, /):
     pass
   # 调用 合法
   hello("lili", 10)
   # 调用 错误
   hello(name = "lili", 10)
   
   # 声明-4 特殊参数 * 之后的参数只能按关键字传参
   def	hello(*, name, age):
     pass
   # 调用 合法
   hello(name = "lili", age = 10)
   # 调用 错误
   hello("lili", 10)
   
   # 声明-5 特殊参数 / * 混合使用，/ * 的规则和 声明-3、4 一样，不过二者中间的参数，可关键字，可按顺序传
   def	hello(name, age, /, weight, *, gender):
     pass
   # 调用 合法
   hello("lili", age = 10, 100, gender = "男")
   hello("lili", age = 10, weight = 100, gender = "男")
   # 调用 错误
   hello(name = "lili", age = 10, 100, gender = "男")
   
   # 传参冲突，kwds 也传 name 时
   def foo(name, **kwds):
       return 'name' in kwds
   # 调用 错误
   foo(1, **{'name': 2})
   # 修复 加上 / （仅限位置参数）后，就可以了。此时，函数定义把 name 当作位置参数，'name' 也可以作为关键字参数的键：
   def foo(name, /, **kwds):
       return 'name' in kwds
   
   # 文档字符串（接口说明文档）
   # 第一行应为对象用途的简短摘要。为保持简洁，不要在这里显式说明对象名或类型，因为可通过其他方式获取这些信息（除非该名称碰巧是描述函数操作的动词）。这一行应以大写字母开头，以句点结尾
   # 文档字符串为多行时，第二行应为空白行，在视觉上将摘要与其余描述分开。后面的行可包含若干段落，描述对象的调用约定、副作用等。
   def my_function():
   	"""Do nothing, but document it.
   
   	No, really, it doesn't do anything.
   	"""
   	pass
   
   # 函数注解（显式说明参数和返回值的类型）
   def f(ham: str, eggs: str = 'eggs') -> str:
     pass
   ```

5. 编码风格推荐

   - 缩进，用 4 个空格，不要用制表符。

     4 个空格是小缩进（更深嵌套）和大缩进（更易阅读）之间的折中方案。制表符会引起混乱，最好别用。

   - 换行，一行不超过 79 个字符。

     这样换行的小屏阅读体验更好，还便于在大屏显示器上并排阅读多个代码文件。

   - 用空行分隔函数和类，及函数内较大的代码块。

   - 最好把注释放到单独一行。

   - 使用文档字符串。

   - 运算符前后、逗号后要用空格，但不要直接在括号内使用： `a = f(1, 2) + g(3, 4)`。

   - 类和函数的命名要一致；按惯例，命名类用 `UpperCamelCase`，命名函数与方法用 `lowercase_with_underscores`。命名方法中第一个参数总是用 `self` (类和方法详见 [初探类](https://docs.python.org/zh-cn/3/tutorial/classes.html#tut-firstclasses))。

   - 编写用于国际多语环境的代码时，不要用生僻的编码。Python 默认的 UTF-8 或纯 ASCII 可以胜任各种情况。

   - 同理，就算多语阅读、维护代码的可能再小，也不要在标识符中使用非 ASCII 字符

#### 四、模块

```python
# 导入方式
import fibo # 直接导入模块
import fibo as fib # 直接导入并与 as 后的变量绑定
from fibo import fib, fib2 # 只导入模块中指定的方法
from fibo import fib as fibonacci # 只导入模块中指定的方法并与 as 后的变量绑定
from fibo import * # 导入模块中的所有不以_开头方法，不推荐这样做
```

#### 五、输入输出

1. 格式化字符串

   ```python
   # 字符串开头使用 F 或 f
   y = 2022
   f'今年是{y}年'
   
   # str.format() 方法
   '{} 小于  {}'.format(1, 2)
   ```

2. 读取文件

   ```python
   # mode 的值包括 'r' ，表示文件只能读取；'w' 表示只能写入（现有同名文件会被覆盖）；'a' 表示打开文件并追加内容，任何写入的数据会自动添加到文件末尾。'r+' 表示打开文件进行读写。mode 实参是可选的，省略时的默认值为 'r'。
   # 通常，文件以 text mode 打开，即，从文件中读取或写入字符串时，都以指定编码方式进行编码。如未指定编码格式，默认值与平台相关 (参见 open())。在 mode 中追加的 'b' 则以 binary mode 打开文件：此时，数据以字节对象的形式进行读写。该模式用于所有不包含文本的文件。
   # with 关键字。优点是，子句体结束后，文件会正确关闭，即便触发异常也可以
   with open('filepath', 'mode') as f：
   	data = f.read()
     
   # 文件对象常用方法
   f.read()
   f.readline()
   f.readlines()
   for line in f:
     pass
   f.write(string)
   ```

3. Json 序列化

   ```python
   import json
   a = {'b': 'B', 'a': 'A'}
   ajsonStr = json.dumps(a)
   # 写入/读取文件
   with open('filepath', 'w') as f:
     json.dump(a, f) 
     
   with open('filepath', 'r') as f:
     str = json.load(f)
   
   ```

#### 五、异常

1. 捕获异常

   ```python
   # 单个错误
   try:
       x = int(input("Please enter a number: "))
   except ValueError:
       print("ValueError")
   # 多个错误
   try:
       x = int(input("Please enter a number: "))
   except (RuntimeError, TypeError, NameError):
       print("RuntimeError, TypeError, NameError")
   # try ... except 语句具有可选的 else 子句，该子句如果存在，它必须放在所有 except 子句 之后。 它适用于 try 子句 没有引发异常但又必须要执行的代码
   try:
       x = int(input("Please enter a number: "))
   except RuntimeError as err:
       print("RuntimeError:{0}".format(err))
   except TypeError:
       print("TypeError")
   else:
     pass
   # finally 无论是否异常都会执行
   try:
       raise TypeError
   finally:
       print('Goodbye, world!')
       
   ```

2. 抛异常

   ```python
   raise NameError('HiThere')
   ```

#### 六、类

```python
class Dog:
  classVar = ''
  def __init__(self, name):
    self.name = name
  def say(self):
    print(self.name)
    
d = Dog("dog")
d.say()
```

