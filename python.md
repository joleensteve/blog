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

#### 二、基本类型

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
   - `list = [1,2,3]` 声明
   - 支持索引和切片
   - `list.append(4)` 追加元素
   - 为切片赋值可以改变列表大小，甚至清空整个列表
     - `list[1:2] = [12,13] -> [1,12,13,4]`
     - `List[:] = [] -> []`
   - `len(list)` list 大小

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
   
   