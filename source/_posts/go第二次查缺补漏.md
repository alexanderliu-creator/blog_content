---
title: Go究极李文周网课学习
date: 2021-02-20 16:05:21
tags: 
	后端


---



# 这儿是对于Go语言整体系统的学习，是b站李文周老师的教程的整理的内容和自己的学习心得体会，记录在这儿

---



<!--more-->





# 关于变量的声明

- 变量声明可以用

```go
var(
	str string
	a   int
	wheater bool
)
```

- 变量如果声明了不使用会报错
- go fmt filename 这种方式可以使文件格式化
- 类型变量和短变量声明:

```go
var s1 = "this is rabbit"
s2 := "this is rabbit too"
```

- 匿名变量"_" ， 可以忽略一些值，不占用空间也不分配内存，接收值之后就直接扔掉了。
- := 等赋值不能在函数外赋值
- 同一个作用域不能重复声明同一个变量



# 关于常量

- `const pi = 3.14159`常量的声明
- 批量声明:

```go
const(
	statusOK = 200
	notfound = 404
)
```

```go
const(
	n1 = 100
	n2
	n3
)
```

如果没有写值的话默认和上面是一样的。

- iota是go语言的常量计数器，只能在常量表达式中使用。

多一个常量，iota++ , 默认值为0, 每出现一次const, iota就重置为0，每多`一行`常量声明，iota在这一行后就会+1。

```go
const{
	a1 = iota
    _
	a2
	a3
    a4 = 100
    a5
}// a1 = 0 , 由于后面没有标，默认也是iota嗷
```

- 定义数量级:

```go
const(
	_ = iota
	KB = 1 << (10*iota)
	MB = 1 << (10*iota)
	GB = 1 << (10*iota)
    TB = 1 << (10*iota)
)
```



# 基本数据类型

- int8 , int16 , int32 ,int 64
- uint8 , uint16...
- uint类型会根据操作系统自己决定
- int也是这样
- uintptr 存放一个指针嗷！
- float32 , float64
- bool - true , false



- 二进制数在go语言中无法直接定义，只能转换
- 八进制前面带有0 ， 十六进制带有0x ，是可以直接声明的。

%d , %o , %x这三个都是常用的十进制，八进制和十六进制,

%b 是二进制。

%#v 会自动帮你的类型加一个双引号。

- 查看变量类型: `%T`
- 强制类型转换: `a := int8(9)`
- %v 打印输出的是变量的值，不用管type , 很有用哦。



- 浮点数: float32 , float64

```go
math.Mathfloat32
math.Mathfloat64
```

默认也是64



# 字符串

- go语言中的字符串只能用`""`来包裹
- go语言中的`''`用来包裹字符
- 一个中文一般占三个字节
- 转义符用法一样的



- 多行字符串：

```go
s2 : = `
	a
	b
	c
`
```

- 反引号\`\`中的东西原样子输出，是不会变的。



- 字符串的拼接: 用加号
- `Printf`和`Sprintf`第一个直接在终端答应输出，第二个是以字符串的形式返回。
- strings包里有好多的像是Split和Contains和HasPrefix , Index , lastIndex , Join等函数方法用来调用。
- 字符串不能修改的，只能转换成切片然后再进行转换哦。
- `""`和`''`对应的类型是不一样的嗷
- go 使用了rune 类型来处理Unicode , 所以如果要处理中文等，都转换成rune类型嗷。



# if和for等结构

- 非常特别：

```go
if a := 19; age > 18{
	body
}
```

- go语言中只有for循环:

  - ```go
    for i := 0; i<10 ; i++{
    	body
    }
    ```

  - ```go
    i := 0
    for ;i<10;i++{
    	body
    }
    ```

  - ```go
    i := 0
    for i<10{
        body
        i++
    }
    ```

  - ```go
    for{
        body
        if(condition){
            break
        }
    }
    ```

  - range好重要！！！

  ```go
  s := "hello沙河"
  for i,v := range s{
  	body
  }
  ```

  i获得的是索引，v获得的是value





# switch和goto

- ```go
  switch n:=1 ; n{
  	body
  }
  ```

- ```go
  switch n{
      case n<26:
      	body...
  }
  ```

- fallthrough可以像c语言一样下穿，就是为了满足c语言。（最好不要用嗷！）

- goto可以跳到某些代码块儿上，也不常用了。

- ```go
  xx:
  	fmt.Println("asjdkfs")
  	
  goto xx // 这样就完成了跳转
  ```
  
- 同样 break 和 continue 也可以接上goto语句





# 运算符

- `a++`里面是单独的语句，不能放在等号的右边赋值
- 位运算:

```go
& 按位与
| 按位或
<< 左移n位
>> 右移n位
^ 按位异或
```

默认是二进制进行计算的

- 要注意范围哦，像是int8你如果左移了10位就可能出问题，但是16位就不一定会嗷。
- 同样的也可以进行赋值的: 

```go
+= -=  =  *=  /=  %=  <<=2 >>=2  &=2 |=2  ^=2
```





# 复合数据类型:

## 数组：

- 长度和类型
- 就类似于其他语言中的列表

```go
var name [num]type
```

- 对用的type就是[3]int
- 初始化：

默认元素都是零值

1. ```go
   a1 = [3]int{0,1,2}
   ```

2. ```go
   a2 := [...]int{1,2,34,545,456...}
   ```

3. ```go
   a3 := [5]int{1,2}   //自动补零
   ```

4. ```go
   a3 := [5]int{0:5,2:8}
   ```

- 数组的传递:

```go
x := [3]int{1,2,3}
y := x
//这个时候得到的y是x的一个副本嗷呜！！！
```



- 数组的遍历：

```go
for i:=0;i<len(array);i++{
	body
}
```

```go
for i,v := range ArrayName{
	body
}
```

- 多维数组:

  - ```go
    var all [3][2]int
    all = [3][2]int{
        [2]int{1,2},
        [2]int{3,4},
        [2]int{5,6}
    } //和numpy差不多嗷呜
    ```

  - 多维数组的遍历:

  ```go
  for _, v1 := range all{
  	fmt.Println(v1)
  	for _,v2 := range v1{
  		fmt.Println(v2)
  	}
  } //两层遍历，这里的v1实际上是内层数组
  ```

  - 多维数组也可以这样定义

  `var a1 [...][2]int` , 最外层才可以... , 内层是不可以加的哦。

- 在go语言中，对于数组的赋值，实质上是值应用，相当于复制了一个数组给我们要引用的对象。





## 切片：

### 切片基础操作：

- 可变长度的，并且是引用类型

```go
var s []int   // 切片不存在为nil
s1 := []int()   // 切片存在且len和cap都为0
s2 := make([]int,0)   //和上面一样没啥区别
```

由于是可变的，声明切片的时候可以这样声明

- 切片和数组最大的区别就是切片声明的时候是不带长度的，因为切片长度是可变的，初始化的时候就得以体现。
- nil类似于其他语言中的空
- 切片没有初始化时默认为空，即没有分配内存空间
- len长度，cap为容量
- 初始化:

```go
1. 
var s []int
s = []int{1,2,3}
// 本质也是造一个数组，返回切片给你
2.
s1 = [...]int{1,2,3,4,5,6}
s = s1[0:4] //和py中一样，左开右闭。
3.
s2 := s3[3:]
// 切片的切片
```

- 切片容量是从切片第一个元素对应底层数组到末尾的元素的个数
- 切片是引用，指向一个指定的数组，切片和数组都是相互影响的。
- make函数构造切片:

```go
s1 := make([]int,5,10) //默认切片元素都为0，如果不加第二个参数，默认cap和len相同，本质也是底层数组的分配。
```

- 切片的内存: 一块儿连续的内存
- 一个nil值的切片是没有底层数组的，切片的长度和容量都是0
- 切片的遍历:

1. ```go
   //和c语言里面一样
   for i:=0;i<len(s);i++{
       body
   }
   ```

2. ```go
   for i,v := range s{
   	fmt.Println(l,v)
   }
   ```

### 切片高级操作：

- 切片的扩容：

  - 相关操作:

  ```go
  s := []string("北京","上海")
  s[2] = "上海" // 这个是错误的,index out of range
  s := s.append(s,"广州","成都")
  // 这个时候底层数组的长度直接翻倍，找了一个新的地址直接分配新的内存，原来的底层数组就gg了，重新用了一块儿新的，s如果不接收新的数组的话，就废了，所以最好就是用原来的变量接收新的值。拓展容量有自己的方法的,感兴趣可以百度嗷！
  
  ```

s1 := []string("武汉","成都")
  s = s.append(s.s1...)// 这样也可以哦, ...表示拆开，把每个单独的元素拿出来，这样也可以达到拓展的目的

  - 调用append必须要用原来的变量接收返回值。
  
- 切片的复制:

  - copy(destination , source)

  ```go
  var a3 []int
  a1 := []int(1,3,5)
  copy(a3,a1)
  ```

- 元素的删除:

  - 可以通过切片来实现，没有专门的实现方法。

  ```go
  a1 = append(a[:1].a1[2:]...)
  ```

  - 注意传递切片的时候要拆开。
  - 一般不是直接操作数组，因为数组长度固定不可变，所以一般都是操作切片，常用的操作方法有:

  ```go
  a1 = s[:]
  ```

  然后再对a1进行操作。

  - 经典例子:

  ```go
  x1 = [...]int{1,3,5}
  s1 = x1[:]
  s1 = s1.append(s1[:1],s1[2:]...)
  //这个时候x1就变成了(1,5,5)
  ```

  由于底层是切片，这样s1变成了[1,5]，按照顺序保存在数组的位置上，但数组的元素个数是不变的，相当于切片有改变了前两个元素。

  - 面试题:

  ```go
  var a = make([]int,5,10)
  
  for i:=0;i<10;i++{
      a = a.append(a,i)
  }
  
  //结果为[0,0,0,0,0,0,1,2,3,4,5,6,7,8,9]
  ```

  一开始初始化make时就已经存在a为[0,0,0,0,0]了，再往后append就是这个结果。







## 指针

- 比较简单（不存在指针操作）
- `&`和`*`就这两个操作
- bugs:
  - `var a *int`这个声明了一个指针，但没有初始化，默认指向nil , `*a = 100`这样赋值是不可以的。因为这个时候，a指针指向一个空地址，连地址指向哪里都不知道无法赋值。
- 用new函数分配内存地址:

```go
var a = new(int)
*a = 100
```

默认给a申请了指针的空间

- new一般用于给基本数据类型申请内存，返回的是对应类型的指针。

- make也是用来分配地址的：
  - 用于为slice , map , chan申请内存 , make返回的是对应类型本身。
- go语言中指针有个用法，就是用指针指示成员变量的时候是可以类似于成员对象一样调用的。







## map

- 映射关系，map是应用类型
- map[key]value

```go
var m1 map[string]int // 这样不行，默认map是空，因为没有为map分配内存
m1 = make(map[string]int,10) // 初始化要估算好内存，最好一次到位，避免动态扩容。
m1["rabbit"] = 18
```

- `v, ok = m1["rabbit"]`因为自动返回两个值嗷
- map的遍历:

```go
for k, v := range m1{
	body
}
// k是key , v是value
// 注意可以用_来取得我们需要的值即可
```

- 删除:

`delete(mapname,keyname)`

- 其他一些用法：

比如按照指定顺序遍历map:

```go
var keys = make([]string,0,200)
for key := range scoreMap{
	keys = append(keys,key)
}

sort.Strings(keys)

for _, key := range keys{
	fmt.Println(key,scoreMap[key])
}
```

利用切片对于map元素进行处理。(利用循环单独提取key存在切片里面，然后对于切片进行操作嗷呜！)。

- map可以和切片等其他的数据类型相组合：

  - 注意初始化!!!

  ```go
  var s1 = make([]map[int]string,10,10)
  s1[0] = make(map[int]string,1)
  //注意多重结构多重初始化哦，和java中一样样的，基本不用，但是复杂结构要哦。
  ```

  - ```go
    var m1 = make(map[string][]int,10)
    m1["homework"] = []int{1,2,3}
    ```

- 切片和map在一起一定要记得初始化嗷。





# 函数（非常非常重要嗷呜！！！）

- ```go
  func sum(x, y int) (ret int){
  	body
  }
  ```

- ```go
  func sum() int{
  	body
  }
  ```

- 两个对比

```go
func sum(x, y int) (ret int){
	ret = x+y
	return
}
//使用命名返回值可以省略
func sum(x, y int) int{
	ret := x+y
	return ret
}
```

- 可变长参数:
  - `fun f7(x string, y ...int){}`
  - 这里的y...处可以传入零个或者多个参数，非常有用
  - y这个时候为一个slice
  - 可变长参数必须放在参数的最后
- go语言中没有默认参数这个概念。
- go语言中的函数传递的都是值哦，都是传递的副本，和java中差不多嗷
- go语言中支持多个返回值嗷呜！
- 返回值要不都不命名要不都命名。

### 重点难点: defer语句

- 多个defer的时候其实上是根据栈来执行的，先进后出的原则嗷呜！
- 多用于函数结束之前释放资源的时候使用，还用于记录时间啊，文件关闭啊等等。
- go语言中的return不是原子操作，在函数中先返回值赋值，然后执行defer语句，最后才真正结束函数。

```go
func f() (x int){
	defer func(){
		x++
	}()
	return 5
}
//最终的答案是6哦

func f() (y int){
    x := 5
	defer func(){
		x++
	}()
	return x
}
//答案是5

func f() (x int){
	defer func(){
		x++
	}(x)
	return 5
}
//答案是5
```

- `defer calc("1",a,b,calc("10",a,b))`

这里先调用内层函数算出内部确切的值，例如内部的a和clac都赋予了此刻值，然后才会把外层函数defer延迟调用，就是说defer作用只有一层，其余的依旧会在本步实现。





### 变量作用域

- 和其他的函数差不多
- 查找变量从内向外查找

### 函数作为返回值

- 函数类型的区分，按照参数和返回值来区分的‘
- 函数接收函数:

```go
func f(x []int){
	body
}

func f(x func(int,int)int){
	//直接调用函数
}
```

- 函数还可以作为返回值:

```go
func f(x func()int) func(int,int) int{
	body
} // 满足参数要求的都可以返回。
```

### 匿名函数与立即执行函数

- 定义一个匿名函数

```go
var f1 = func(x,y int){
	body
}
```

- 函数内部不能再定义函数（但是匿名函数可以），用于这种情况。通常和java中一样用于只使用一次的函数或者使用次数较少的函数。
- 立即执行函数：

```go
fun main(){

	func(){
		fmt.Println("hello this is here")
	}()
	
}
```

//这里的fun函数声明之后立刻使用，只使用一次，之后就找不到了。

- 函数内部定义函数一般用匿名函数，只使用一次的直接用立即执行函数

### 闭包

- 包装函数，使得函数之间的接口相匹配。
- 函数名加上了()代表函数的执行

```go
func adder(x int) func(int) int{
	return fun(y int) int{
		x += y
		return x
	}
}

func main(){
	ret := adder(100)
	ret2 = ret(200)
}
```

- 人为构造函数接口，函数内部定义的函数
- 闭包是： 
  - 一个函数
  - 函数包含其外部作用域的一个函数，把x包含进去了，再接下来的过程中再继续调用。
  - 闭包 = 函数 + 外部变量的引用
- 原理：
  - 函数可以作为返回值
  - 查找变量的顺序
- 外部传入的变量被包起来了，在调用内层函数的时候实现再次访问。

```go
fun f1(f func()){
	f()
}

func f2(x,y int){
	fmt.Println x+y
}

//想把f2传入f1中但模式不匹配，这时候要把f2包起来，把参数的接收分开来！f1(f2)
//构建闭包函数f3，目标是返回一个能传入f1的函数，但是又要保留f2的参数
func f3(f2(int,int),x,y int) func(){
    temp := func(){
        f2(x,y)
    }
    return temp
}
// 传入目的f2，进行处理后得到能被f1接收的函数
// f3老子帮你f2要处理的数据处理好，然后帮你构造好，以你需要的形式给你吐出来！！！
```





# 内置函数简介

## panic和defer和recover(错误处理)

- 大致有`close len new make append panic recover`
- go语言中没有异常机制
- panic直接让程序崩溃退出
- 防止程序占用资源可以用defer来解决panic , recover则可以尝试恢复错误

```go
defer func(){
	err := recover()
	fmt.Println(err)
	fmt.Println("释放网络连接")
}()

panic("出现了严重的错误。")
```

recover会尝试着修复panic，跳过panic的部分然后往后走。recover尽量少用嗷呜！！！该panic还是要panic

- recover()一定要搭配defer使用，defer一定要在可以能会panic的语句之前调用

## fmt标准库简介

- fmt主要用于打印输出和获取输入的时候使用的嗷！

- `Print Printf Println` , `printf`用于格式化输出字符串
- 注意一下 `%T %v %#v %% %+v`等等（这几个是通用的，不管什么类型都可以用）
- `%t %p`等类似于布尔值，指针等类型
- `fmt.Scan fmt.Scanf fmt.Scanln` 

```go
var s string
fmt.Scan(&s)

fmt.Scanf("%s %d %s\n",&a,&b,&c)
// 扫描完换行

fmt.Scanln(&a,&b,&c)
//注意这里不能接收格式化的值嗷
```

- Sprint 拼接



# 递归

- 老递归了，乖娃子好家伙你差点整死你爹。
- 和别的语言差不多，找到规律和递归出口，节能写出来，没啥区别orz



# 自定义类型and类型别名

- type是用来定义类型的

```go
type myInt int     //自定义类型
type yourInt = int //类型别名
```

- 别名会使得默写情况下表达更加清晰（例如`rume`和`int32`）



# 结构体

- 定义一个结构体的方法：

```go
type 类型名 struct{
	body
}

var varname 类型名
```

- 打印输出的话是按顺序输出每个字段的值

- 匿名结构体：

  - ```go
    var s = struct{
    	name string
        age int
    }
    ```

  - 临时使用的时候用的到。

  - 结构体要用花括号。

  - 这样也可以临时声明一个结构体并且返回给一个变量。

- 和c语言一样，函数是传值操作，结构体同样，如果要改变结构体中的变量的话，一样要传递结构体指针。

- 取得结构体的指针：

  - `var p2 = new(person)`(new直接返回对应类型的指针)
  - `%p`能够打印输出对应的指针的地址

  ```go
  fmt.Println("%p\n",&a)
  ```

- 结构体初始化：

  - 直接声明结构体然后依次赋值

   ```go
  var p = person(
      name: "rabbit"
      age: 19
  )
   ```

  - 使用值列表（按照声明的顺序来初始化）进行初始化:

  ```go
  p := person(
  	"rabbit"，
      19，
  )
  ```
```
  
- 结构体内的内存是连续的

- 可以设置一个构造函数来进行初始化也可以鸭

​```go
func newPerson(name string , age int) *Person{
	return &person{
		name: name
		age: age
	}
}
```

- 注意哦，这里返回的是原构造函数中的一个拷贝之后的版本，当占用内存多的时候，可以考虑使用结构体指针。

## 结构体匿名字段

- ```go
  type person struct{
  	string
  	int
  }//相同类型只能写一个
  ```

- 适用于字段少，简单且不常用的情况

- 访问特定元素时，例如访问string，这个时候把string作为成员的名字，可以通过.名字直接进行访问。

## 嵌套嗷呜!

- 禁止套娃？（结构体套用结构体）

- ```go
  type address struct{
  	province string
      city string
  }
  
  type person struct{
      name string
      age int
      addr address
  }
  
  p1 := person{
      name : ""
      age : 
      addr: address{
          province: ""
          city: ""
      }
  }
  ```

- 匿名嵌套结构体:(感觉很好用的样子嗷呜！！！)

  ```go
  type person struct{
  	province string
  	city string
  	address
  }
  
  
  p1 := person{
      name : ""
      age : 
      address: address{
          province: ""
          city: ""
      }
  }
  ```

  这样之后就能够直接通过`p1.city`等方式访问内部元素

  步过要注意，有可能产生匿名结构体的冲突，比如说几个都有city这个元素，嵌套一个可以，多个的话为了防止冲突最好不用。

- 构造结构体的函数，可以专门写一个构造函数`return struct`，然后每次调用这个构造函数就会十分的方便简单。

# 方法和接收者

- 有点像类的方法那种感觉啦(方法是作用于特定类型的函数)
- 声明: `func (d dng) wang()`，推荐用类型的首字母小写来声明这个类型的一个变量。
- go语言中如果标识符首字母是大写的，就表示对外部包可见，类似于java中的public。
- 注意，如果函数的首字母大写对外可见，那么函数的返回值首字母也要大写，不然类型就不匹配。
- 对于这种标识符都应该加上注释

```go
//Dog 是一个关于狗的结构体
type Dog struct{
	name string
}
```

- 接收者只能有一个嗷呜

- 同时还是要注意函数传值传址嗷呜，就算是方法传址也才能改变对象的值。

- 使用指针传递内容的情况：

  - 需要修改接收者中的值
  - 接收者是拷贝开销比较大的大对象
  - 一致性，某个方法传入指针，最后其他的都一样传入指针。

- 添加方法只能给自己定义的type添加方法，不能给别的包定义方法。比如说你想给int设置方法，是不可以的，但是你可以自己写一个对象为int，`type myInt int`这样构造一个然后再定义方法就可以。

- `var x int32 = 10`与`x := int32(10)`与`var a = myInt(100)`，这个东西是强制类型准换哈，这个不是方法函数。

- ```go
  s1 := map[string]int{
  	"liu" : 5,
  	"yi" : 10
  	"hao" : 23
  }
  ```






# 结构体模拟实现继承

- 

```go
func animal struct{
	name string
}

func (a animal) move(){
    fmt.Println("唱跳rap篮球")
}

type dog struct{
	feet uint8
	animal
}

func (d dog) wang(){
	fmt.Printf("%s在叫：汪汪汪~",d.name)
}
//可以用dog调用animal方法
//可以用dog来调用animal属性
```

- dog中拥有了animal就相当于继承了animal



# 结构体与JSON

- 把go语言中的结构体转换为json格式，并且把json格式能转回go语言
- `json.Marshal(struct)`
- 格式化功能在json包里面做的，要在json包里拿到属性和字段，注意要用大写嗷。或者还有一种做法：
- 反引号引号起来就可以以自定义的名字访问

```go
Name string 'json:"name" db:"name"'
```

- 反序列化: `json.Unmarshal(type,&target)`（这里要是指针哦，得到了一个值然后传入要地址鸭！）
- target为一个指针嗷呜！



# 接口

- 接口是一种类型，和普通的类型没有差别，可以作为返回值啊等等等等。（面向接口编程的趋势）

- 应用场景： 给出一个模板，需要的类实现这个模板，规定了变量有哪些方法

- ```go
  type speaker interface{
  	speak()  //只要实现了speak方法都是speaker类型
  }
  
  func (c cat) speak(){
      fmt.Println("miao~")
  }
  
  func da(x speaker){
      x.speak()
  }
  ```

- 只要实现了接口中规定的所有类型，那么这个变量就可以当作接口实现的类型的变量。

- 但你想到有多个对象有同一个方法的时候，就可用接口了，实现了接口就实现了方法。

- 甚至可以用接口变量来接收实现的接口,可以指向实现的接口。

- 多个类型可以实现一个接口，一个类型也可以实现多个接口。

- 接口可以嵌套！

## 使用值和指针接收者不同嗷

- 实现值接收者，结构体和结构体指针的变量都能存。
- 实现指针接收者，只能拿存结构体类型的变量。
- 用指针接收者往往更加常用嗷！

## 空接口

- `type xx interface{}`

- 接口中啥都没有，意味着所有类型都实现了这个接口，意味着这个接口能接收所有的对象。（有点像Object?）

- 空接口没有必要起名

- ```go
  m1 := map[string]interface{}
  m1 = make(map[string]interface{},10)
  m1["hobby"] = [...]string{"a","b"}
  ```

- 相当于多种类型的集合！！！传入啥类型都可以！！！

- 类型断言： 

  - ```go
    a.(type)
    
    func assign(a interface{}){
        switch v := a.(type){
            case string:
            	body1
            case int:
            	body2
            case bool:
            	body3
            default:
            	body4
        }
    }// switch a.(type){}也可以
    ```

  - int64与int是两个完全不同的类型

# package



- 组织go语言的一个单位。

- 只有main包才能组织成一个文件。
- 只有大写的元素和函数才能被跨包访问，小写表示私有。
- 导入的包名应该紧接在package后面。
- 禁止套娃，不能相互嵌套。
- `import 别名 "包的路径"`。
- 匿名导入包 `import _ "路径"`。
- 导入会自动触发init()函数，当import恶的时候会自动调用。
- init在导入的全局声明和main之间执行。
- 从外往里调用import本质上是从里往外调用`init()`方法。





# 文件操作(日志库作业)

- 接口：不同的模式对应的文件操作不一样，可以输出到终端和文件中

## 打开/关闭文件

- ```go
  // 打开文件
  fileObj,ok := os.Open("./main.go")// 返回一个文件指针，可以用绝对路径
  if ok != nil{
      fmt.Printf("Open file failed , err:%v",err)
      return
  }
  defer fileObj.close()
  ```

## 读写文件

### 读文件：

- ```go
  var temp = make([]byte,128) //指定读的长度
  // 或者是:var temp = [128]byte
  n , error = fileObj.Read(temp[:])// 一次读128个进入切片(n是读入的数，error是出的问题)
  if error != nil{
      body
  }
  fmt.Println(string(temp[:n]))读取文件需要一个切片作为参数，先构造切片，然后读入切片中即可
  ```

- `bufio`读取文件（一样要先`os`打开文件）

```go
reader := bufio.NewReader(fileObj)//创建对象
line , err := reader.ReadnString('\n')
if err == io.EOF{
    return
}
if err != nil{
    body
}
// 这里可以从reader中选择读取方式并返回
```

- `ioutil`读取文件：
     - `ret , err := ioutil.ReadFile(Filename)`直接读取整个文件 , 这里readfile后买你的参数直接接的就是文件，不需要像上面一样先打开文件哦。

### 写文件:

- ```go
  os.OpenFile(name,flag,mode)//打开文件的模式
  ```

- 这里的flag本质上为十六进制数（其实底层为二进制），可以指定多种状态`os.O_APPEND|os.O_CREATE `， 可用二进制中的"|"来表示，本质为二进制的位运算来确定模式。哪一位为1 ， 代表着激活了那种模式。

- 用`fileObj.write(切片)`和`fileObj.WriteString(sting)`可以写入文件

- 注意哈，文件开完了之后要关上的！

- 默认好多模式嗷，对着模式去选择就好啦

- 和上面读文件一样，这里需要写文件也有三种方式

```go
bufio.NewWriter(fileObj)
wr.WriteString("...")
wr.Flush() // 将缓存中的内容写入文件中
```

```
err := ioutil.WriteFile(filename,[]byte(str),0666)
```

- 注意, Scanln只要读到空白符就停止了，如果想读入空白符怎么做？

```go
var s string
reader := bufio.NewReader(os.Stdin)
s, _ = reader.ReadString('\n')
fmt.Printf("...")
```

- bufio.Reader本质上为一个接口类型

## 文件中间插入内容

- 原理用脑子想一想
- 可以在库中找到对应的方法嗷呜！
- 可以用fileObj.seek(跨过的长度，文件的初始位置)，光标移动到要插入内容的位置。
- `os.O_RDWR`设置文件可读可写，然后借助seek定位
- 再向文件中写入内容即可
- 切片不能用new初始化哦

```go
var s []byte
s = []byte{'c'}
```

- 新写的内容会覆盖原有的内容哦
- 要实现插入内容，一般都要新建中间文件，在中间文件中实现之后再导入到目标文件中。（读入之前的，插入目的数，紧接后面的）
- 文件关了之后再操作哦，比如可以把中间文件重命名为我要的目的文件即可，把以前的文件直接覆盖哦。

# time标准库

- 常用函数: `time.Now()/Year()/Date()/Hour()/Minute()/Second()/Day()`
- 时间戳: `.Unix()或者是.UnixNano()`,从1970年一月一日到现在的时间
- 时间间隔：

`.Second`啊等等属性

- 一些操作：
  - .Add   e.g.  `now.Add(24 * time.Hour)`
  - .Sub
  - .Equal
  - .Before
  - .After
- 定时器

`timer := time.Tick(time.Second)` // 每隔一秒钟执行一次

- 时间格式化：
  - 2006 1 2 3 4 5   这个是go诞生的时间，用于格式化时间

  - ```go
    fmt.Println(now.Format("2006-01-02"))
    ```

  - ```go
    fmt.Println(now.Format("2006/01/02 15:04:05"))
    ```

  - ```go
    fmt.Println(now.Format("2006/01/02 15:04:05 PM"))
    ```

  - ```
    fmt.Println(now.Format("2006/01/02 15:04:05.000"))
    ```

  - `.Parse`函数用于格式化把字符串转换为时间
  
  ```go
  time := time.Parse("2006-01-02 15:04:05","2020-10-27 19:12:39")
  ```





## time库的补充内容

- 一些标准化的内容:

```go
Microsecond
Millisecond
Second
Minute
Hour     //这些都是定义好的标准时间嗷！
```

- Sub函数的调用:

```go
nextYear , err := time.Parse("2006-01-02,2020-01-01")

now.Sub(nextYear)
```

注意，这里减的是一个时间对象嗷，不是减去时间。

- 时区指定

- `sleep(d duration)`(本质为int64)

- ```go
  n := 100
  time.Sleep(time.Duration(n))
  
  time.Sleep(100)
  ```

如果直接传入参数，直接帮你传。如果用变量则需要手动住那换一样嗷！！！

- ```go
  n := 5
  time.Sleep(time.Duration(n)*time.Second)
  ```

- 时区的确定和转换:

`Parse`这个玩意没有时区的概念，就是标准时间，不是东八区的时间，这个时候就要获取时区。

```go
loc , err := time.LoadLocation("Asia/Shanghai")
if err != nil{
    fmt.Println("Load loc failed, err:%v\n",err)
    return
}

timeObj , err := time.ParseInLoaction("2006-01-02 15:04:05","2019-08-04 14:41:50",loc)

fmt.Println(time.Obj)
```

只有在时区相同的情况下做加减计算才有意义啊！



# `runtime.Caller`

- ```go
  pc , file , line , ok := runtime.Caller(0)
  
  if !ok{
  	body
  }
  
  // 这里的0表示调用层数，0表示调用函数的第一层，1就表示调用函数的第二层（由内往外找层数）。
  // file表示调用的文件名 , line表示调用的函数所在的行，ok表示时候调用成功
funcname := runtime.FuncForPC(pc).Name()
  //拿到对应层的函数名
  path.Base(file)
  //拿到对应的文件名
  ```
  
- 这就是一个库能够拿到调用它的函数对应的行，还有函数名等等，比如在日志库中有相应的应用





# 反射

- 用的少但是原理要知道

- ```go
  func reflectType(x interface{}){
      v := reflect.TypeOf(x)
      name := v.Name()
      kind := v.Kind()
  }
  ```

- ```go
  v := reflect.ValueOf(x)
  v.Kind() // 值的类型种类
  v.Int() / v.Float()
  reflect.Int64 / reflect.Float32
  // 这里取了一个v作为value的种类,它可以去和reflect下的值比较，但是调用的时候，只有第三行两种形式，如果需要int64 , 还需要int64(v.Int())这种方式强制类型转换。
  ```

- 通过反射设置变量的值，一样要用指针

```go
func reflectSetValue(x interface{}){
	v := reflect.ValueOf(x)
    if v.Elem().kind() == reflect.Int64{
		v.Elem().SetInt(200)
	}
}
// 这里的x要是指针
```

- 还有类似的函数 : `isNil() , isValid()`

```
reflect.ValueOf(a).isValid()
```

- 结构体指针

# strconv标准库简介

- 用于类型转换，有的时候不能进行强制类型转换

```go
ret2 := string(97)
```

这里就是把97转换为对应的char类型，就有问题

```go
fmt.Sprintf("%d",97)
```

这里返回一个字符串嗷，可以获得数字对应的字符串

- ```go
  fmt.ParseInt(str,10,64)
  ```

这里表示把str转换为十进制六十四位 , 如果传入的是0的话就变成int

- `strconv.Atoi()`字符串转换为int类型可以直接用这个
- `strconv.Itoa()`int类型转换为字符串
- `boolValue , _ := strconv.ParseBool(boolstr)`
- `.ParseFloat()`

# 并发

## goroutine

- ```go
  func hello(){
  	fmt.Println("hello")
  }
  
  func main(){
      go hello()
      time.Sleep(time.Second)
      fmt.Println("main")
  }
  ```

- 传入go的要是一个包装好的函数哦

- main函数结束了，它生成的所有线程都会结束

- 见`vscode`中的例子有更加深入的理解哦

### 关于goroutine的一些底层的知识

- goroutine和线程:
  - goroutine和os线程的栈内存不一样，goroutine内存很小，但是可以扩容。（可扩展的栈）
  - GMP调度 ， g为goroutine的，存了goroutine信息和与P绑定的信息，这里p管理一组goroutine队列，为队列做调度，保证最大效率的利用goroutine队列。m是go运行时对于操作系统内核线程的虚拟，是一一映射的关系，一个goroutine最终是要放在M上执行的。p和m也一般是一一对应的
  - goroutine初始栈的大小是2k.

## sync.WaitGroup

- goroutine对应的函数执行结束的时候，goroutine就结束了。
- main函数结束了，main函数创建的goroutine就都结束了

那么怎么样协调时间呢？

### 如何优雅的等其它线程结束

- `var wg sync.WaitGroup`
- 只能有一个这种计数器，不能有多个。只有当计数器为0的时候，main函数才能退出
- 在每一个线程中: `wg.Add(1)` ， 当一个线程结束了之后，wg才会减一。`wg.Done()`，在线程结束的时候添加，推荐在线程执行的函数中第一行用`defer wg.Done()`。
- 每一个线程开始时，Add记录一下，结束时Done()删除一下。
- 函数的最后要用`wg.Wait()`

### math/rand

- ```go
  rand.Seed()
  rand.Int()
  rand.Intn(n)//生成一个[0，n)之间的整数
  ```

- 在go里面编译好了之后，种子就固定了，如果想每次生成的不一样，那么要在函数中设置种子并且传入不同的值，比如时间。（注意这里和c语言不同哦。）

- 默认生成的都是正数，如果需要负数的话`0-rand.Int()`类似于这样即可

- .Sleep接受的是duration类型  time.Sleep(time.Millisecond * time.Duration(rand.Intn(300)))这样返回的就是Duration类型，可以这么转换整数为duration.

## `runtime.GOMAXPROCS`

- 设置处理的cpu的数量，如果作为日志监控等可以占用的少一点
- `runtime.NumCPU()`返回使用的CPU的个数
- 一个线程对应多个goroutine
- 一个程序可以使用多个操作系统线程
- goroutine和os线程时多对多的关系,  m:n , 把m个goroutine分配个n个操作系统线程去执行



# Channel(通道)

### 初始化

- 共享内存进行数据交换时，可能发生问题，这个时候就应该加锁，要改成通过通信来共享内存。goroutine是go程序并发的执行体，channel就是他们之间的连接。channel是可以让一个goroutine发送到另外一个goroutine的通信机制。
- `var 变量 chan 元素类型`，channel是一种特殊的类型，本质为一个队列，遵循First in first out的规则。
- 通道的初始化   `b = make(chan int)`
- 通道必须初始化才能使用
- `var ch3 chan []int` 声明一个传递int切片的通道
- `make(chan 元素类型，[缓冲大小])` 含有带有缓冲区的通道的初始化 , 缓冲区如果不存在的话，无法向里面传递信息，因为无位置接收。
- 通道里面的类型小一点好，如果大的话，用指针会好哦。

### 发送与接收

- `ch <- 10`
- `x := <- ch `
- 可以跨越线程进行值的输入与输出
- 只有`b<-`，如果b没有缓冲区，则无法成功输入，如果`x := <-b`，b中还没有值，则程序会等待b的输入
- 通道的关闭 : `close(channel)` ， 不关也没问题的
- `for i := range ch1{}` 这个也是作用于通道
- 关闭通道后不可以往通道里面再写数据，但是可以从通道里面读数据。往线程里写完数据之后还是最好, close(线程)这样来保证线程的安全。

### 单向通道

- 只能读入或者输出，常用于一些函数，限制通道的功能(当通道只能进行某一些操作时)
- `func f1(ch1 <-chan int) || func f2(ch2 chan<- int)`

### 数据读取

- ```go
  for{
  	x , ok := <-channel
  	if !ok{
  		break;
  	}
  	else{
  		fmt.Println(x)
  	}
  }
  ```

- 这里如果channel为空，会等待，直到channel中没有值才会退出

- 什么时候ok是false , 直到通道close掉，这个才为false,所以要人为去close掉通道，才能退出！

- for , range是一样的，会等待通道内的值，只有通道关闭才会退出循环，所以在程序中要记得close嗷！

### 其它的一些操作

- `var once sync.Once`然后`once.Do(func(){...})`这样就可以使得某一段函数只执行一次，例如多个容器接收，一次关闭某个特定的通道
- 关于关闭的通道可以继续读取通道，当读取完元素之后依旧可以继续读取，读取的是对应类型的零值。
- 通道由两个返回值，一个是真正的返回值，第二个是true或者false即是否读取到了值，就算ok为false，一样可以读取到对应的返回值。

### 通道的状态

![image-20201105184844101](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201105184844101.png)

- 关闭已经关闭的channel就会panic
- 如果所有的goroutine都在等一个阻塞的channel这个时候程序会发生死锁。
- goroutine的泄露，如果后台在等一个阻塞的channel，主程序能够运行，这个时候可以正常运行，但是后台goroutine占着一块儿内存始终不会释放，这个时候造成了goroutine的泄露。

### 匿名结构体计数通道

```go
notifyCh  <-  struct{}{}
//不占任何内存，构造出来用于记录数量
	
go func(){
    for i:=0;i<5;i++{
        <-notifyCh
    }
    close(results)
}()
```

- 如果能取到5个值，说明这个通道里面目的数据量都被传满了，这个时候就可以去取真实的值了。这这个线程也会一直等着，直到目标容量被存满之后，才会关闭对应的通道。非常非常有用。

# select多路复用

- ```go
  select{
      case <-ch1:
      
      case data := <-ch2:
      
      case ch3<-data:
      
      default:
  }
  ```

- 哪个能成走哪个，若多个能走从中随便选一个嗷。

- 空的select可以用于阻塞main函数，程序会运行到这里然后一直等哦

# Channel重大发现嗷

- 当多个任务无法区分时，可以用多个线程分别执行，思考之间的关系，在主程序中去进行wait和close之类的操作，可以很好的解决通道的问题！！！





# sync包中的互斥锁

- 实现了对于公共空间的元素的访问的操作,对于公共资源的操作和访问时，就先加锁。
- `var lock sync.Mutex`and`lock.Lock()`and`lock.Unlock()`

## 读写互斥锁

- 用于读的次数远大与写的次数
- 分为读锁和写锁两个锁，读锁的时候可以继续获取，写锁的时候别的锁就要先等一等。
- `sync.RWMutex`



# sync.Once

- `.Do(func())`只做一次，once.Do()保证了某个函数仅仅执行一次，要求函数苛刻，传入的函数要求没有参数也没有返回值，因此这个时候，可能会使用到函数闭包。

`f := func(){close(channel)}`



# sync.Map

- Go内置的map不是并发安全的
- 这个时候自己可以加锁嘛，go内置了一些开箱即用的map - `sync.Map`，定义了一些Store和Load等方法

- 开箱即用意味着map不用初始化

```go
var m2 = sync.Map{}
var wg sync.Waitgroup

func main(){
    for i:=0;i<21;i++{
        wg.Add(1)
        go func(n int){
            key := strconv.Itoa(n)
            m2.Store(key, n)
            value,_ := m2.Load(key)
            wg.Done()
        }(i)
    }
}
```

- 必须使用sync.map内部自带的Store来存值，用Load来取值，用特定的方法来取值。





# atomic原子性操作

- 原子性操作自动帮你加锁，去锁。

- ```go
  func main(){
      atomic.AddInt64(&x,1)
  }
  //内置的加法函数
  ```

- 有一系列的相关操作来保证功能的实现。





# 网络编程

## 协议和层

- 物理层：把电脑连接起来的物理手段，传送0，1信号
- 数据链路层：规定解读电信号的方式，“以太网”协议占据主导，然后将数据分为帧发送。以太网规定了网卡接口有唯一的地址，就可以确定某一个网卡。
- 网络层：网络地址和MAC协议，IPv4和IPv6两个协议，最多发65535个字节
- 传输层：通过MAC地址和IP地址，可以建立通信，还有一个参数叫端口，确定是电脑哪个程序，上述三个确定了唯一一个电脑上的某个程序。TCP和UCP两种，三次握手，四次挥手等等orz , 时效性高用UCP , TCP则保证数据的稳定性
- 应用层：建立在传输层之上，不停的加上不同的协议，最后得到一个物理层的传输数据。



## socket编程

- TCP/IP都要用到socket , socket是一个抽象层，处于传输层和应用层之间。
- 一个TCP连接多个客户端，go中的并发性高
- TCP处理流程：
  - 监听端口
  - 接收客户端请求建立来凝结
  - 创建goroutine处理链接







# os.Args

- 获取命令行参数
- 可以以os.Args[num]的方式获取某个输入参数

# Flag

- 创建一个标志位参数

`name := flag.String("name","ask","b")`,这个时候的name时一个指针，指向某个分配了位置的内存地址。

- 可以通过`--help`来体现到底有哪些方法

- ```go
  name := flag.String("name","ask","Enter your name")
  flag.Parse()
  fmt.Println(*name)
  ```

- `age := flag.Int("age",18,"请输入真实姓名")`

- 有Int , Sting ,Bool ,Duration等方法，这些方法都用于构造某个标志位，可以返回一个指针，通过特定的指针访问某个用户输入的特定的属性。
- `flag.exe -name=兔子姐姐 -age=10000`这样即为用户端输入。
- `var name string` , `flag.StringVar(&name,"name","ask","Enter your name")`

这种方式也可以，并且不用执政，得到的就是纯纯的变量本身嗷。

- `flag.Args() , flag.NArg() , flag.NFlag()`
- 第一个时返回[]string参数，返回命令行后面其它参数；第二个时返回其它参数的个数，第三个时返回命令行参数的个数

# TCP粘包

- 传送数据的时候，可能会同时发送多个数据
- 如果发送的时间间隔长的话，一次就只会发送一个数据
- 可以自己定义协议来定义包头和长度
- 详情参见代码



# `net/http`

- 首先这个包写的非常好，可以直接用来写一些小型的服务端
- 和前端的知识有的融合在一起了，`HandleFunc(request,func)`可以获得对应字段进行数据处理，读取对应的页面文件和CSS文件并传输给服务端。详情参见vscode中的例子
- `ListenAndServe`函数可以用于监听某一个端口号，作为服务端等待客户端向其发送的请求
- `http-client`中，url的构建可以采用，新建一个`url.Values{}`，然后再再不断地set元素的值，最后采用`urlStr = data.Encode()`，这个时候再去get就可以使用`http.NewRequest("GET",urlStr,nil)`这个方式来获得新的url。下面为构建url对象：

```go
apiUrl := "http://127.0.0.1:9090/get"

data := url.Values{}
data.Set("name","小王子")
data.Set("age",18)
urlObj , err := url.ParseRequestURL(apiUrl)

queryStr := data.Encode()
urlObj.RawQuery = queryStr
req , _ := http.NewRequest("GET", urlObj.String(),nil)

http.DefaultClient.Do(req)
```

- 上述为个性请求的定制
- response读取完成之后记得要关闭
- 后端发请求频繁的时候，可以考虑公用一个Client，这个时候可以通过声明全局变量的形式来操作，可以通过var把关键字提前到最前面，然后剪辑出来，让其保证长连接,`DisableKeepAlives: false`。
- 拉起的频率的非常低时，就应该禁用长连接，上面那个设置为true，用完client后马上进行释放，就会非常方便。

# Context单元测试和pprof调试工具

## go语言中单元测试

- `go test`命令
- 每个函数必须导入test包，文件必须以_test.go命名，测试函数必须以Test开头，后缀就是特定的函数名，大写开头，写法如下:
- `func TestName(t *testing.T)`
- 可以看一看测试组，有'Run'这种函数可以有很多花哨的操作
- 可以通过`go test -cover`来查看覆盖率，甚至可以生成覆盖率的文件，通过go语言中的tool工具再浏览器中打开查看，画面很炫酷
- 一般情况下各个函数模块的测试覆盖率要达到100% , 整体覆盖率要达到60%

## go语言中的基准测试

- `func BenchmarkName()`

- 执行时使用`go test -bench=Split`

- 要用循环反复执行一个函数来进行操作效率分析

- 具体的例如基准测试使用的proxy的量，都可以找到对应的博客来操作对应的值。

- ```go
  func BenchmarkSplit(b *testing.B){
  	for i:=0;i<b.N;i++{
  		Split("A:B:C",":")
  	}
  }
  ```

- 上述就是一个测试的例子，可以通过结果来分析各个值，然后再次去优化代码。例如内存申请的次数等会影响到代码的效率。

### 性能比较函数

- 例如斐波那契数列
- 可以通过封装函数来实现多次跑一个函数来达到目的，传入的参数可以为`b *testing.B , int n`，第二个参数是执行特定函数的次数。
- b.N是循环的次数，不要传入函数中，是错误的用法。是一个随着函数和执行次数在该改变的变量。
- 默认情况下，每个基准测试最少运行一秒，若没有到疫苗，b.N的值会1，2，5，10，20...这样一直增加，并且函数可以继续运行。
- 可以用`-benchtime`来获得标志增加的最小基准时间，还有其它类似的用法等等。
- 还可以并行测试，指定CPU的数量之类的。

### setup和teardown等专业工具

- setup和teardown函数，一个是初始化操作，一个是结尾首位操作

## go语言中的示例函数

- 以ExampleName这种形式开头，可以自动产生文档嗷！
- 具体的在单元测试这一章的网上是有的。
- go语言可以自动拉一个文档出来，骚得很。



## pprof工具的使用(go性能调优)

- aspects: CPU profile , Memory , block , goroutine
- `runtime/pprof` , `net/http/pprof`等运行时数据进行分析
- 只有在代码测试优化时才进行调用
- `isCPUPprof , isMemPprof`是否开启标志位

![image-20201201220143773](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201201220143773.png)

- 上述往文件中写入对应的信息

- 结合flag包判断是否开启性能检测

```go
var isCPUPprof bool

flag.BoolVar(&isCPUPprof,"cpu",false,"turn cpu pprof on")
flag.Parse()

if isCPUPprof{
	file, err := os.Create("./cpu.pprof")
	if err != nil{
		fmt.Printf("create cpu failed , err: 			%v\n",err)
		return
	}
	pprof.StartCPUPprof(file)
	defer func(){
		pprof.StopCPUProfile()
		file.Close()
	}()
}
```

- 对于模型进行分析的时候，命令行:`xx.exe -cpu=true`，三十秒后就会自动生成相关的文件
- `go tool pprof xx.pprof`可以使用内置的go工具对于特定的值进行分析。进入交互界面后，输入top3或者top4就可以查看占用cpu最多的几个东东，这些东东可以帮助你了解自己函数的性能，从而进行优化！(quit可以退出交换界面嗷！)
- 还有`go-torch`等操作使得图特别选炫酷，可以看blog去看看怎么去操作那些东西。



# 数据库

- 用`MySQL`

- `DDL,DML,DCL`之类的

- 支持插件式的搜索引擎。常见的有:`MyISAM`和`InnoDB`

  - MyISAM:

  1. 查询速度快
  2. 只支持表锁
  3. 不支持事务

  - InnoDB:

  1. 整体速度快
  2. 支持表锁和行锁
  3. 支持事务

  - 事务：把多个SQL操作当成一个整体

  - ACID:

  1. 原子性：只有成功或者失败
  2. 一致性
  3. 隔离性：事务之间相互独立
  4. 持久性：事务操作结果不会丢失

- 关系型数据库：用表来存放一类的数据

- 《漫画数据库》可以康康嗷！

- 隔离的四个级别：Read uncommitted , read committed , repeatable read , serializable

- 索引的原理： B5树和B+树

- MySQL主从：

  - binlog，把某个

- 读写分离：从从库里面读，往主库里面写



# Go对于MySQL的操作

## 连接数据库：

- database/sql ，这个库并没有具体实现，不提供数据库驱动，因此要装第三方的驱动。
- <img src="C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201207163310289.png" alt="image-20201207163310289" style="zoom: 200%;" />

- ```bash
  go get -u github.com/go-sql-driver/mysql
  ```

下载依赖管理

## 读取某一行的数据：

![image-20201207172109804](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201207172109804.png)

- 必须对于`rowobj`调用scan方法，这样才能释放链接，不然就会一直占有连接资源。

- `SetMaxOpenConns`可以设置与数据库连接的最大数目
- `SetMaxIdleConn`设置连接池中的最大闲置连接数
- `db.QueryRow(sqlStr, 2).Scan(&u1.id, &u1.name, &u1.age) *//从连接池拿出一个连接去数据库查询数据*`高级写法

- 不要用:=去处理db，db这里是一个全局变量，要对于全局db进行处理。

## 增删改查：

详情看相关的go代码嗷！ 

## 数据预处理

- sql语句分开成为命令部分和数据部分，先把命令部分发送非服务端，然后数据给服务端进行替换。
- reason: 让服务器提前编译，一次白你要多次执行，时间花费少，以及避免用户输入恶意数据这种情况的发生。

## go语言实现事务

- 两个或者多个操作揉成一个整体
- `Begin(*Tx, error) Commit Rollback()`这三个来实现
- 只要有错误就回滚
- 相当于多个事务打包一起，除了问题就回退，保证表原来的状态，只有所有过程都没问题才能正常更新表嗷！！！

## sqlx的使用

- [具体使用方法](https://www.liwenzhou.com/posts/Go/sqlx/)

- 注意，这里的get方法用到了反射，取了指针，所以变量定义的时候要是大写，才能够被别的包的reflect检索到。
- 注意传递指针注意注意注意嗷！
- 不同的数据库占位符是不一样的。

## sql注入



# GO MODULE

- go mod init
- go get
- go env

# Context

- 非常重要:
  - 如何优雅控制子goroutine退出？
    - 可以和C语言中一样，去中间break一下就退出了。
    - `var exitChan chan bool`， 然后用select去检查channel中有没有值，这样来控制退出。
    - `ctx , cancel := context.WithCancel(context.Background)`
    
    完整版:
    
    ```go
    func f(ctx context.Context){
    	def wg.Done
        FORLOOP:
        for{
            fmt.Println("退出")
            time.Sleep(time.Millisecond * 500)
            select{
                case <- ctx.Done():
                	break FORLOOP
            	default:
            }
        }
    }
    
    func main(){
        ctx , cancel := context.WithCancel(context.Background())
        wg.Add(1)
        go f(ctx)
        time.Sleep(time.Second*5)
        cancel()
        wg.Wait()
    }
    ```
    
    一个根的ctx一级级往下传，可以控制嵌套的进程的暂停和退出



# 服务端Agent开发

## 项目架构设计

## Kafka and zookeeper

## tailf接受





# gin框架

- github上有源码，直接上去copy下来
- 路由初始化：
  - 1. gin.Default()获取路由
    2. 绑定路由规则，执行的函数,`r.GET("/",func(c *gin.Context))` , Context里面封装了request,response之类的东西。使得更加为一个整体。
    3. 使服务器监听端口
- gin路由：
  - 由httprouter做的
  - `Restful风格的API`:
    - 获取文章 `/blog/getXxx` => Get   blog/Xxx
    - 添加 `/blog/addXxx`=>  Post   blog/Xxx
    - 修改 `/blog/updateXxx` => Put   blog/Xxx
    - 删除 `/blog/delXxx` =>  Delete   blog/Xxx
- 剩下的例子去看代码
- httproter原理可以去github上看源码，构造前缀树啊之类的东西
- ![image-20201215152054557](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201215152054557.png)




# cookie

- HTTP是无状态的协议，为了区分是否为同一个客户端发出，才有了cookie。
- client请求服务器，服务器除了返回信息还会返回一个cookie，cookie保存在本地的浏览器上，起到身份识别的作用。当client再次请求server时，会携带cookie
- 作用：
  - 保持用户登录状态
  - 保持用户的信息(比如说京东的购物车)
- 缺点：
  - 安全性不高
  - 增加访问带宽消耗
  - 可以被禁用
  - cookie有上限的



# Redis数据库

- 应用场景:
  - 缓存系统，减轻mysql的压力
  - cache缓存
  - 热门排行榜
  - 利用LIST实现队列的功能
- Session的过期时间，可以在set的时候使用
- 我们用的库是自带连接池的库嗷！
- 《Redis实战》好书啊！

# Session

- 弥补Cookie的不足，只存session的id， session是离不开cookie的，session保存在cookie里面
- 要用到redis数据库嗷！
- ![image-20201216200209531](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201216200209531.png)

- session接口设计：
  - Set()
  - Get()
  - Del()
  - Save():  session存储

















































# 一些偷学到的小技巧:



## vscode中用户代码片段打开设置

- 在vscode中:
  - ctrl + shift + p
  - snippets
  - 选含有preferences那一项
  - 选中go
  - 然后用json格式就可以自定义代码格式
    - 前面的名字为你想定义的模板名
    - prefix为快捷键的方式
    - body为你要输出的内容
    - description为描述貌似没啥用???
    - $为最后光标停留的位置
    - 关于json怎么写之类的我收藏了的，可以直接康收藏。



## 相关代码含义查找

- 上go语言官网（已在收藏夹中）
- 找到对应的import函数
- 找到对应的用法即可





# 一些小而重要的知识点

## 文件中defer的用法

![image-20201026172410243](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201026172410243.png)

下面才是对的，由于出问题时，fileObj为nil ， 而err有值，所以调用fileObj.Close()会引发错误造成panic ， 所以直接退出程序这个时候反而是对的，因为对应文件根本没有打开，只有打开了对应文件才应该关闭。相当于固定搭配，记住就好啦！

## 使用go module导入包

- 对于需要包管理的文件夹，要先`go mod init name`这阿姨那个对于包管理初始化，这个name是起的名字。这名字对于以后的调用中都用得到，而且是从`demoname`开始调用相对路径的，注意不是从文件夹开始调用，是从`demoname`开始调用



## 一些快捷键补充

- 按住alt用鼠标选中多个位置，可以多行一起操作



## 面试题分享

- 判断链表有循环？

从头开始走，一次走一步，一次走两步，看两者有没有机会相遇

- 走台阶问题

问题归纳，使用递归可以解决





# 大作业

## 日志库

### 需求分析

- 不同的地方输出日志
- 日志的分级别:
  - debug
  - trace
  - info
  - warning
  - error
  - fatal
- 日志要支持开关控制
- 日志要有时间，行号，文件名，日志级别，日志信息
- 日志文件要切割:
  - 按照文件大小切割
    - 每次记录日志之前都判断一下文件大小
  - 按照日期切割
    - 每次记录日志之前都判断一下时间（在日志结构体中设置一个字段记录上一次切割的小时数）
    - 写日志前判断一下小时数是否和结构体中的小时数一致即可

### 日志库的异步实现:

- 在线程写入的时候，启用其它的线程来执行其它功能保证不会卡。



## worker pool(goroutine池)

- 使用goroutine来实现int64位中各位和的计算







# Tips:

- 一个用了指针，全部都用指针嗷，这样才能保证对象对应的结构体或者方法能够被修改！！！要么全是指针接收者，要么全是值接收者。
- 传递参数的时候:

```go
func add(a ...interface{}){
	body
}

var s = []interface{1,2,3}
add(s)
add(s...)

//这里s传递是一个slice,s...传递的则是切片中的每一个元素，这个很重要嗷。
```

