---
title: Python综合课设时的小练习
date: 2021-02-20 16:05:21
tags: 
	Python

---





---

# 这里时综合课设时对于python基础语法的小学习嗷！！！

<!--more-->





# 综合课程设计自学python内容（第一周）



## 1. 运算符:

/除	%取模	**取幂	*乘

Question:过去三个月的水费是53元、32元和64元。这三个月的每月平均水费是多少？请写一个表达式来计算均值，并使用 print() 查看结果

```python
def main():
    a = 53
    b = 32
    c = 64
    print("The average price is "+str(a+b+c/3))


```


## 2. 变量:

在任何情况下，无论左侧项是什么，在赋值后，都是右侧值的变量名称。一旦将变量值赋值给变量名称，你便可以通过该名称访问这个值。

> ### 2.1 赋值方式:
>
> 常用 x = 3 y = 4 z = 5 和 x, y, z = 3, 4, 5
>
> ### 2.2 变量命名:
>
> - 变量中不能含有字母，不能包含空格，并且需要以字母或者下划线开头。
> - 不能使用保留字活内置标识符
> - 在python中，变量的命名方式是全部使用小写字母，并且用下划线区分单词
>
> ```python
> #正确示例：
> my_height = 58
> my_lat = 40
> my_long = 105
> #错误示例：
> my height = 58
> MYLONG = 40
> MyLat = 105
> #虽然最后两个在 python 中可以运行，但是它们并不是在 python 中命名变量的推荐方式。
> #推荐命名变量的方式称之为 snake case，因为会使用下划线连接单词。
> ```
>
> - 变量有很多种类型，只有数字的变量，通常有integers (int), floats (float), 等. 所有文本类型的变量类型是string (str)型. 我们可以用type(变量)函数来查看变量的类型
>
> ### 2.3 字符串:
>
> 在python中字符串是支持索引的，在java中不是的哦
>
> 有一些固定的用法啊，比如: split, + , count , * , len , index ...
>
> ### 2.4 类型和类型转换:
>
> int(str)或者str(int)等方式可以进行类型转换

## 3. 字符串方法:

好多好多方法，多用多看才能熟练使用嗷！

## 4. 布尔型运算符:

与c中差不多，有区别的是连接的时候，用的是 and 和 or 还有 not

## 5.列表

> - 列表在Python中可以存储一系列有序的内容，包括数字或者文本
> - 可以使用方括号创建列表。
> - 列表可以包含我们到目前为止所学的任何数据类型并且可以混合到一起。
>
> 1. 索引：在 python 中，所有有序容器（例如列表）的起始索引都是 0。
> 2. 切片：在使用切片功能时，务必注意，起始索引包含在内，终止索引排除在外。
>
> ```python
> list_x=[1, 3.4, 'a string', True]
> print ("list_x[:]: ", list_x[:])
> print ("list_x[2:]: ", list_x[2:])
> 
> print ("list_x[1:3]: ", list_x[1:3])
> print ("list_x[:-1]: ", list_x[:-1])
> ```
>
> 3. 其余常用操作:
>
> - len()
> - x[0] = y (Replacing items in a list)
> - list a = list b + list c(列表合并)
> - 还可以使用 in 和 not in 返回一个布尔值，表示某个元素是否存在于列表中，或者某个字符串是否为另一个字符串的子字符串。
> - (a==b) 和 (a is b)的区别嗷！
> - len() 返回列表中的元素数量。
> - max() 返回列表中的最大元素。最大元素的判断依据是列表中的对象类型。数字列表中的最大元素是最大的数字。字符串列表中的最大元素是按照字母顺序排序时排在最后一位的元素。因为 max() 函数的定义依据是大于比较运算符。如果列表包含不同的无法比较类型的元素，则 max() 的结果是 undefined。
> - min() 返回列表中的最小元素。它是 max() 函数的对立面，返回列表中的最小元素。
> - sorted() 返回一个从最小到最大排序的列表副本，并使原始列表保持不变。
> - append() 会将元素添加到列表末尾。
> - join() Join 是一个字符串方法，将字符串列表作为参数，并返回一个由列表元素组成并由分隔符字符串分隔的字符串
>
> ```python
> new_str = "\n".join(["fore", "aft", "starboard", "port"])
> print(new_str)
> ```

## 6. 字符串和列表的可变性对比

> 1. 字符串对象无法更改，只能创建全新的对象，而列表中的元素则是可以更改的。
>
> 2. 无论是前面学习的字符串对象，列表对象，还是以后学习到的其他的每种数据类型， 都需要注意两个事项：
>
>    - 可变吗？
>    - 有序吗？ 字符串和列表都是有序的。但是，后面将会学习某些数据类型则是无序的。 对于接下来要遇到的每种数据类型，有必要理解如何设定索引，是否可变，是否有序
>
>    此外，还会发现每种数据类型有不同的方法，因此为何使用一种数据类型（而不是另一种）在很大程度上取决于这些特性
>
> 3. 赋值: 注意指针的感觉，指向的是否是同一个对象嗷！

## 7. 元组

元组是Python里另外一种容器，是一种不可变有序元素数据类型。

- 元组和列表相似，它们都存储一个有序的对象集合，并且可以通过索引访问这些对象。
- 但是与列表不同的是，元组不可变，也就无法向元组中添加项目或从中删除项目，或者直接对元组排序。

元组还可以用来以紧凑的方式为多个变量赋值: 

```python
tuple_x = (3.0, "hello")
tuple_y = 3.0, "hello"
print (tuple_x)
print (tuple_y)
```

元组支持去进行拼接甚至切片，只是内部已有元素无法进行修改而已。

## 8. 集合

集合是一个包含唯一元素的可变无序集合数据类型。 集合常用于快速删除列表中的重复项。(leetcode里面经常见到嗷)

集合的运算：

- in 运算符
- add 方法添加元素、
- pop 方法删除元素（注：当你从集合中拿出元素时，会随机删除一个元素）
- remove( x ) 删除指定元素
- clear()清空集合

注意和列表不同，集合是无序的，因此没有“最后一个元素”。

```python

fruit = {"apple", "banana", "orange", "grapefruit"} 
print("watermelon" in fruit) 

fruit.add("watermelon")  
print(fruit)

print(fruit.pop()) 
print(fruit)
```

集合可以快速执行（快速是相对于其他的几种数据容器）类似对数学集合执行的操作：

- union 返回一个新的 set 包含 s 和 t 中的每一个元素
- intersection 返回一个新的 set 包含 s 和 t 中的公共元素
- difference 返回一个新的 set 包含 s 中有但是 t 中没有的元素

## 9. 字典:

字典存储的是键值对，是唯一键到值的映射，也就是字典里面不能有重复的键值。字典是可变数据类型.

字典的键可以是任何的数据类型，不一定是str型，例如可以是int,float,但是前提是必须是不可变的类型，即所属的类型必须不可变。

```
# 创建一个字典
goku = {"name": "Goku",
        "eye_color": "brown"}
print (goku)
print (goku["name"])
print (goku["eye_color"])
```

```
# 改变值
goku["eye_color"] = "green"
print (goku)
```

```
# 增加新的键值对，也是使用[]操作符
goku["age"] = 24
print (goku)
```

可以使用关键字 in 检查值是否在字典中。和in类似，有一个方法叫做get。get会在字典中查询值，但是和方括号不同，如果没有找到键，get 会返回 None（或者你所选的默认值）。 如果预计查询有时候会失败，get 可能比普通的方括号查询更合适，因为错误可能会使程序崩溃。

```
print("carbon" in goku)
print(goku.get("eye_color"))
goku.get('kryptonite', 'There\'s no such element!')
```

```
# Length of a dictionary
print (len(goku))
```

## 10. 复合结构

我们可以在其他容器中包含容器，以创建复合数据结构。例如嵌套的字典，嵌套的字典有点像C语言里的二维数组，嵌套多层的字典，可以想象为多维数组

```
structures = {"hydrogen": {"number": 1,
                         "weight": 1.00794,
                         "symbol": "H"},
              "helium": {"number": 2,
                         "weight": 4.002602,
                         "symbol": "He"}}
```

```
#TODO 访问helium的字典


#TODO 访问hydrogen的weight

#TODO 添加一个新的键值对到hydrogen里
```

## 11. 控制流结构:

- if 语句以关键字 if 开始，然后是要检查的条件，接着是英文冒号。
- 条件用布尔表达式指定，结果为 True 或 False。
- 之后是一个条件为 true 时将执行的**缩进代码块。**

多条件判断：

- if：if 语句必须始终以 if 条件开始，其中包含第一个要检查的条件。如果该条件为 True，Python 将运行这个 if 块中的缩进代码，然后跳到 if 语句之后的剩余代码。
- elif：elif 条件用来检查其他条件（前提是 if 语句中之前的条件结果为 False）。可以从示例中看出，可以使用多个 elif 块处理不同的情形。
- else：最后是 else 条件，它必须位于 if 语句的末尾。该条件语句不需要条件。如果 if 语句中所有前面的语句结果都为 False 时，将运行 else 块中的代码。

**注意缩进**

- 在 Python 中，使用缩进来封装代码块。例如，if 语句使用缩进告诉 Python 哪些代码位于不同条件语句里面，哪些代码位于外面。
- 缩进通常是四个空格一组。更改缩进会完全更改代码的含义。

## 循环:

**For 循环的组成部分**

循环的第一行以关键字 for 开始，表示这是一个 for 循环

例如：for i in [0, 1, 2]:

i是迭代变量，in之后的是可迭代的对象，用i这个迭代变量表示当前正在被处理的可迭代对象的元素。 在此示例中，迭代变量 i 在第一次迭代时将是0，在第二次迭代时将是1。

for 循环头部始终以英文冒号 : 结束。

**range()函数**

range() 是一个内置函数，用于创建不可变的数字序列。for循环常会用到range()函数，便于

它有三个参数，必须都为整数：

range(start=0, stop, step=1)

Start是该序列的第一个数字，stop比该序列的最后一个数字大 1，step是该序列中每个数字之间的差。如果未指定的话，start默认为 0，step 默认为 1（即上述 =0和 =1）。

- 如果你在 range() 的括号里指定一个参数，它将用作 'stop' 的值，另外两个参数使用默认值。

  例如：list(range(4)) 返回 [0, 1, 2, 3]

- 如果你在 range() 的括号里指定两个参数，它们将用作 'start' 和 'stop' 的值，'step' 将使用默认值。

  例如：list(range(2, 6)) 返回 [2, 3, 4, 5]

- 可以为三个参数 'start、stop' 和 'step' 均指定一个值。 E.g. list(range(1, 10, 2)) 返回 [1, 3, 5, 7, 9]

**注：**因为 range 本身的输出是一个 range 对象。我们可以通过将其转换为列表（如上面的例子）或在 for 循环中遍历它，查看 range 对象中的值集合。

```python
for i in range(5)
```

例题：

测试：

遍历names列表，然后创建 usernames 列表的 for 循环。要为每个用户通过names中的姓名创建用户名，用户名有限制，不能有空格，不能有大写，因此需要对names中的元素进行处理，使姓名全小写并用下划线代替空格。

names = ["Bob Tribbiani", "Jerry Geller", "Fernando Bing", "Laffy Buffay"] 也就是例如"Bob Tribbiani"要处理为"bob_Tribbiani"

在这个过程中，需要用到两个str的方法：

- replace(被替换的内容,替换的内容) 替换函数
- lower() 大写变小写函数

```mysql
def main():
    names = ["Bob Tribbiani", "Jerry Geller", "Fernando Bing", "Laffy Buffay"]
    for i in range(len(names)):
        names[i] = names[i].replace(' ','_')
        names[i] = names[i].lower()
    print(names)
```

### while循环

while 循环是一种无限迭代循环，无限迭代循环是指循环重复未知次数，并在满足某个条件时结束。

While 循环的组成部分：

- 第一行以关键字 while 开始，表示这是一个 while 循环。
- 然后是要检查的条件。
- while 循环头部始终以冒号 : 结束。
- 该头部之后的缩进部分是 while 循环的主体。如果 while 循环的条件为 true，该循环的主体将被执行。每次运行循环主体时，条件将被重新评估。这个检查条件然后运行循环的流程将重复，直到该表达式变成 false。
- 循环的缩进主体应该至少修改测试表达式中的一个变量。如果测试表达式的值始终不变，就会变成无限循环

### Break、Continue控制循环

有时候需要更精准地控制何时循环应该结束，或者跳过某个迭代。在这些情况下，可以使用关键字break和continue，这两个关键字可以用于 for 和 while 循环。

- break 使循环终止
- continue 跳过循环的一次迭代

## 12. 函数

**函数头部**

我们从函数头部开始，即函数定义的第一行。 `def add_two(x):`

- 函数头部始终以关键字 def 开始，表示这是函数定义。
- 函数名称（在此例中是 add_two，因为函数名是要一个单词，所以需要用_进行连接），遵循的是和变量一样的命名规范。
- 名称之后是括号，其中可能包括用英文逗号分隔的参数（在此例中是 x）。形参（或实参）是当函数被调用时作为输入传入的值，用在函数主体中。如果函数没有参数，这些括号留空。
- 头部始终以英文冒号 : 结束。

**函数主体**

- 函数的剩余部分包含在函数主体中，也就是函数完成操作的部分。

  ```
  return x
  ```

- 函数主体是在头部行之后缩进的代码。

- 在此主体中，可以引用参数并定义新的变量，这些变量只能在这些缩进代码行内使用。

- 主体将经常包括 return 语句，用于当函数被调用时返回输出值。return 语句包括关键字 return，然后是经过评估以获得函数输出值的表达式。如果没有 return 语句，函数直接返回 None（例如内置 print() 函数）。

**函数的命名规范**

- 函数名称遵守和变量一样的命名规范。

- 仅在函数名称中使用普通字母、数字和下划线。不能有空格，需要以字母或下划线开头

- 和变量名字一样，不能使用在 Python 中具有重要作用的保留字或内置标识符

- ## 变量作用域

  是指可以在程序的哪个部分引用或使用某个变量。

  在函数中使用变量时，务必要考虑作用域。

  如果变量是在函数内创建的，则只能在该函数内使用该变量，此时无法从该函数外面访问该变量

  ```
  def some_function():
      word = "hello"
  
  def another_function():
      word = "goodbye"
  ```

  像这样在函数之外定义的变量依然可以在函数内访问。

  ```
  word = "hello"
  
  def some_function():
      print(word)
  ```

```python
#TODO 下面代码运行会发生错误，
# 因为Python 不允许函数修改不在函数作用域内的变量，也就是egg_count在函数体内被修改会报错。
# 这条原则仅适用于整数和字符串
# 列表、字典、集合、类可以在子程序（子函数）中通过修改局部变量达到修改全局变量的目的
# 请修改下面的代码，直到可以正确运行

egg_count = 0

def buy_eggs():
    egg_count += 12 # purchase a dozen eggs

buy_eggs()
```

## Lambda 表达式

使用 Lambda 表达式创建匿名函数，也就是没有名称的函数。

lambda 表达式非常适合快速创建在代码中以后不会用到的函数。常用于高阶函数或将其他函数作为参数的函数

示例：

```
def multiply(x, y):
    return x * y
```

用Lambda改写为：

```
double = lambda x, y: x * y
```

**组成部分**

- 关键字 lambda 表示这是一个 lambda 表达式。
- lambda 之后是该匿名函数的一个或多个参数（用英文逗号分隔），然后是一个英文冒号 :。和函数相似，lambda 表达式中的参数名称是随意的。
- 最后一部分是在该函数中返回的表达式，和在函数中看到的 return 语句很像。
- lambda 表达式不太适合复杂的函数，但是非常适合简短的函数。

**练习：Lambda 与 Map**

本测试将要求使用map()函数，这是一个高阶内置函数，它接受函数和可迭代对象作为输入，并返回一个将该函数应用到**可迭代对象的每个元素**的迭代器。

下面的代码要求使用 map() 计算 numbers 中每个列表的均值，并创建列表 averages。

测试运行这段代码，看看结果如何。

通过将 mean 函数（mean函数用于计算均值）替换为在 map() 的调用中定义的 lambda 表达式，重写这段代码，使代码更简练。

In [ ]

```
numbers = [
              [34, 63, 88, 71, 29],
              [90, 78, 51, 27, 45],
              [63, 37, 85, 46, 22],
              [51, 22, 34, 11, 18]
           ]

def mean(num_list):
    return sum(num_list) / len(num_list)

#TODO 注释掉mean函数，然后利用Lambda重写均值计算
```

## 13. 类:

自己下去复习嗷，还有继承之类的东西

## 14. 实用函数和列表推导式：

## zip函数

zip 和 enumerate 是实用的内置函数，可以在处理循环时用到。

zip 返回一个将多个可迭代对象组合成一个元组序列的迭代器。每个元组都包含所有可迭代对象中该位置的元素。例如，

```
list(zip(['a', 'b', 'c'], [1, 2, 3])) 
> [('a', 1), ('b', 2), ('c', 3)].
```

正如 range() 一样，我们需要将其转换为列表或使用循环进行遍历以查看其中的元素。

In [ ]

```
#可以用 for 循环拆分每个元组。
letters = ['a', 'b', 'c']
nums = [1, 2, 3]

for letter, num in zip(letters, nums):
    print("{}: {}".format(letter, num))
    
#可以把zip组合后的迭代器创建为一个字典
cast=dict(zip(letters, nums))
print(cast)
```

In [ ]

```
#除了可以将两个列表组合到一起之外，还可以使用星号拆分列表。
some_list = [('a', 1), ('b', 2), ('c', 3)]
letters, nums = zip(*some_list)
```

**zip最有意思的功能：可以对矩阵进行转置**

```
data = ((0, 1, 2), (3, 4, 5), (6, 7, 8), (9, 10, 11))

data_transpose = tuple(zip(*data))

print(data_transpose)
```

**测试：**

使用 zip 写一个 for 循环，该循环会创建一个字符串，指定每个点的标签和坐标，并将其附加到列表 points。每个字符串的格式应该为 label: x, y, z。例如，第一个坐标的字符串应该为 F: 23, 677, 4。

In [ ]

```
x_coord = [23, 53, 2, -12, 95, 103, 14, -5]
y_coord = [677, 233, 405, 433, 905, 376, 432, 445]
z_coord = [4, 16, -6, -42, 3, -6, 23, -1]
labels = ["F", "J", "A", "Q", "Y", "B", "W", "X"]
points = []

#TODO
```

## Enumerate函数

enumerate 是一个会返回元组迭代器的内置函数，这些元组包含列表的索引和值。

当需要在循环中同时获取可迭代对象的每个元素及其索引时，将经常用到该函数

In [ ]

```
letters = ['a', 'b', 'c', 'd', 'e']
for i, letter in enumerate(letters):
    print(i, letter)
```

**测试：**

使用 enumerate 修改列表students，使每个元素都包含姓名，然后是角色的对应身高。例如，第一个元素应该从 "Barney Stinson" 更改为 "Barney Stinson 72”。

In [ ]

```
students = ["Barney Stinson", "Robin Scherbatsky", "Ted Mosby", "Lily Aldrin", "Marshall Eriksen"]
heights = [72, 68, 72, 66, 76]
#TODO
```

## 列表推导式

列表推导式可以用于快速的创建列表。使用起来很简洁。 例如：

```
capitalized_cities = []
for city in cities:
    capitalized_cities.append(city.title())
```

可以用列表推导式改写为：

```
capitalized_cities = [city.title() for city in cities]
```

**列表推导式的方法：**

- 使用方括号 [] 创建列表推导式，括号里使用for循环，或者可能会包含判断条件。

  上述列表推导式对 cities 中的每个元素 city 调用 city.title()，以为新列表 capitalized_cities 创建每个元素

- 还可以向列表推导式添加条件语句。在可迭代对象之后，你可以使用关键字 if 检查每次迭代中的条件。

  ```
  squares = [x**2 for x in range(9) if x % 2 == 0]
  ```

  上述代码将 squares 设为等于列表 [0, 4, 16, 36, 64]，因为仅在 x 为偶数时才评估 x 的 2 次幂。

- 此时如果想添加 else，将遇到语法错误。

  ```
  squares = [x**2 for x in range(9) if x % 2 == 0 else x + 3]
  ```

- 如果你要添加 else，则需要将条件语句移到列表推导式的开头，直接放在表达式后面，如下所示。

```
squares = [x**2 if x % 2 == 0 else x + 3 for x in range(9)]
```

In [ ]

```
#TODO 使用列表推导式创建一个列表 multiples_3，能够计算出 1 - 20 这 20 个整数中分别乘以 3 之后的结果。
```

In [ ]

```
#TODO 使用列表推导式创建新的列表 first_names，其中仅包含names中的名字（小写形式）。
# 需要会用到split()函数
names = ["Rick Sanchez", "Morty Smith", "Summer Smith", "Jerry Smith", "Beth Smith"]
```

In [ ]

```
#TODO 使用列表推导式创建一个 passed 的姓名列表，其中仅包含得分至少为 65 分的名字。
scores = {
             "Rick Sanchez": 70,
             "Morty Smith": 35,
             "Summer Smith": 82,
             "Jerry Smith": 23,
             "Beth Smith": 98
          }
```

## 15. 文件读写

这个笔记本上有，自己下来可以康康

import函数:

其他常用库：

- csv：对于读取 csv 文件来说非常便利
- collections：常见数据类型的实用扩展，包括 OrderedDict、defaultdict 和 namedtuple
- random：生成假随机数字，随机打乱序列并选择随机项
- string：关于字符串的更多函数。此模块还包括实用的字母集合，例如 string.digits（包含所有字符都是有效数字的字符串）。
- re：通过正则表达式在字符串中进行模式匹配
- math：一些标准数学函数
- os：与操作系统交互
- os.path：os 的子模块，用于操纵路径名称
- sys：直接使用 Python 解释器
- json：适用于读写 json 文件（面向网络开发）
- datetime: 获取时间日期
- zipfile：从 zip 文件中提取所有文件