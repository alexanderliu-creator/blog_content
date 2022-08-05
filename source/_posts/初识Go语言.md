---
title: 初识Go语言
date: 2021-10-12 22:17:11
tags: 大三自学
---



# Go非常有意思的一门语言，这一次的学习偏向于基础知识，后面可能还会接触工程项目，总之先把基础打好！！！

<!--more-->



# 基础环境配置

## 具体步骤：

- 在Window上直接下载Golang加上Goland就好，这里比较简单我不做过多的记录，电脑崩了好几次，这已经是第二次配置Go语言了orz。
- 下面记录一下在Linux中配置环境的过程：
  - 镜像网站：
    1. 官方网站`https://golang.org/dl/`，搭梯子，访问太慢。
    2. Go官方镜像站（推荐）：`https://golang.google.cn/dl`
    3. 中文网站：`https://studygolang.com/dl`
    
  - 下载源码之后：`sudo tar -zxvf 源码包 -C /usr/local/`才能够很好的解决上述的问题嗷！！！
  
  - 解压完成之后配置环境变量：
  
    - 首先`cd / cd ~`，回到`$HOME目录下`，HOME目录下有个.bashrc文件，就是环境目录的配置文件
    - 在文件末尾加上我们的环境配置：
  
    ![image-20211013094203229](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211013094212.png)
  
    其中第一行是我们解压的go源码所在的目录。第二行GOPATH是我们写go代码的工作路径，可以自定义，这里我们将其配置在$HOME目录下的go文件夹中。第三行我们去写系统的环境变量，取并集，机上我们GOROOT的bin和GOPATH的bin就可以了嗷！！！
  
    - `source ~/.bashrc`重新加载我们的配置文件进来嗷！！！
    - `go version`以及`go help`没有问题，说明就配置成功了。
    - 使用本地的VSCode通过SSH插件连接虚拟机中对应的go的工作目录，可以成功连上，说明配置成功了！！！

# Go的序言

## 优势：

### 部署简单：

1. 可以直接编译成机器码（go build）
2. 不依赖其他库（编译生成的文件就全部整好了！！！）
3. 直接运行就可以部署！！！（./生成的可执行文件就可以完成服务器的运行嗷！！！）

### 静态类型语言：

- 编译的时候就可以查出来隐藏的大多数问题，防止跑的过程中出现问题。



### 语言层面的并发：

- 天生的基因支持并发
- 充分利用多核



### 强大的标准化：

- runtime系统调度机制：
  - 帮助垃圾回收
  - 帮助我们做一些Go的调度的平均分配
  - 很多的优化机制
- 高效的GC垃圾回收：
  - 1.8版本之后就加入了三色标记和混合写屏障，效率非常搞了
- 丰富的标准库：

![image-20211013095232293](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211013095232.png)



### 简单易学：

- 25个关键字
- C基因，内嵌C都支持？？？
- 面向对象的特征（继承，多态，封装）
- 跨平台



### 大厂领军：

- 背书猛的一批：

![image-20211013095436889](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211013095437.png)



- 编译速度很快啊！！！

![image-20211013095552886](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211013095553.png)



## 适用领域：

1. 云计算基础设施：**Docker , k8s** , etcd , consul , cloudflare CDN等等
2. 基础后端软件：tidb , influxdb
3. 微服务：go-kit , micro , monzo bank的bilibili等
4. 互联网基础设施：以太坊 ， hyperledger等



## 不足：

1. 包管理的不足
2. 没有泛化，GO计划2.0上加上（传言）
3. 所有的Exception都用Error来处理
4. 对于C的降级处理，并非无缝衔接

# 认识Go程序

## 语法：

### 简单程序入门：

```go
package main //程序的包名

//引入的包
import (
   "fmt"
   "time"
)

//main函数
func main() {
   //Golang中的";"，加上和不加上都可以哦
   fmt.Println("hello Go!!!")
   time.Sleep(1 * time.Second)
}
```

- 一个Package中只有一个Main入口哦！！！
- 函数的`{`一定是和函数名在同一行的，否则编译错误。





### 变量声明：

```go
package main

import (
   "fmt"
   "math"
)

func main() {
   //方法一：声明一个变量
   var a int
   fmt.Println("a = ",a)
   fmt.Printf("type of a = %T\n",a)

   //方法二：声明一个变量，初始化一个值
   var b int = 100
   fmt.Println("b = ",b)
   fmt.Printf("type of b = %T\n",b)

   //方法三：初始化的时候，可以省去数据类型，通过值自动会匹配当前的变量的数据类型
   var c = 200
   fmt.Println("c = ",c)
   fmt.Printf("type of c = %T\n",c)

   var d = "abcd"
   fmt.Printf("d = %s, type of d = %T\n",d,d)

   //方法四：（最常用的方法）省去var关键词，直接自动匹配
   e := math.Pi
   fmt.Printf("e = %f, type of e = %T\n",e,e)
}
```

- 上面就是声明变量的方法。
- 如果想要全局变量，把变量的声明放在方法外面就成嗷！！！

```go
var gA int = 100
var gB = 200
//gC := 300
```

上面的方法一二三都可以声明全局变量，但是方法四这种简单的是不适用的嗷！！！

- 声明多个变量

`var xx , yy int  = 100, 200`

`var kk , ll  = 100 , "Alexander"`

多行的多变量声明：

```go
var(
	vv int = 100
    jj bool = true
)
```

语法四声明变量：
```go
xx , yy := 100,200
fmt.Println(xx)
fmt.Println(yy)
```





### Const与Iota知识点注意事项

```go
package main

import "fmt"

// const可以用于枚举类型
const (
   //可以在const() 中添加一个关键字iota，每行iota都会累加1，第一行的ioat默认值为0
   BEIJING = 10*iota  //iota = 0
   SHANGHAI      //iota = 1
   SHENZHEN      //iota = 2
)

const (
   a, b = iota+1 , iota+2 // iota = 0 , a = iota + 1 , b = iota + 2 , a = 1 , b = 2
   c, d               // iota = 1 , c = iota + 1 , d = iota + 2 , c = 2 , d = 3
   e, f               // iota = 2 , e = iota + 1 , f = iota + 2 , e = 3 , f = 4

   g, h = iota*2 , iota*3 // iota = 3 , g = iota * 2 , h = iota * 3 , g = 6 , h = 9
   i,k                   // iota = 4 , i = iota * 2 , k = iota * 3 , i = 8 , k = 12
)

// summary: iota每一行都加1，第一行为0（从0开始），具体的计算公式则是根据我们第一行给出的计算公式来确定的嗷！！！
// iota只能够配合const() 一起使用哦！！！iota只有在const进行累加效果。。。

func main() {
   //常量（只读属性）
   const length int = 10

   fmt.Println("length = ",length)

   //length = 100，常量是不允许修改的
   fmt.Println("BEIJING = ",BEIJING)
   fmt.Println("SHANGHAI = ",SHANGHAI)
   fmt.Println("SHENZHEN = ",SHENZHEN)
}
```



### 函数的多返回值三种写法

```go
package main

import "fmt"

func foo1(a string, b int) int {
   fmt.Println("--- foo1 ---")
   fmt.Println("a = ",a)
   fmt.Println("b = ",b)

   c := 100

   return c
}

//返回多个匿名返回值
func foo2(a string, b string) (int,int) {
   fmt.Println("--- foo2 ---")
   fmt.Println("a = ",a)
   fmt.Println("b = ",b)

   return 666,777
}

//返回多个返回值，有形参名称的，这两值就算我的函数中不赋值，默认的返回值也为0嗷！！！
func foo3(a string, b string) (r1 int,r2 int) {
   fmt.Println("--- foo3 ---")
   fmt.Println("a = ",a)
   fmt.Println("b = ",b)

   //给有名称的返回值变量赋值
   r1 = 1000
   r2 = 2000

   return
}

//r1 , r2类型相同的时候，可以使用一个int就成嗷！！！
func foo4(a string, b string) (r1 ,r2 int) {
   fmt.Println("--- foo4 ---")
   fmt.Println("a = ",a)
   fmt.Println("b = ",b)

   //给有名称的返回值变量赋值
   r1 = 1000
   r2 = 2000

   return
}

func main() {
   c := foo1("foo1",6666)
   fmt.Println("c = ",c)


   ret1 , ret2 := foo2("foo2","Hah, this is another one!!!")
   fmt.Println(ret1)
   fmt.Println(ret2)


   ret1 , ret2 = foo3("foo3","Hah, this is the third one!!!")
   fmt.Println("ret1 = ",ret1," ret2 = ",ret2)

   ret1 , ret2 = foo4("foo4","Hah, this is the fourth one!!!")
   fmt.Println("ret1 = ",ret1," ret2 = ",ret2)
}
```

- summary：
  1. 匿名
  2. 匿名多返回值
  3. 有名称的返回值



### Import导包路径和Init方法

- 示意图：

![image-20211014194102605](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211014194110.png)

有点像Java的初始化函数，在调用某个包中的方法的时候，先会执行Init()方法进行初始化。

- 函数：

```go
package main


//大写的函数才是对外开放的函数，不然的话就只有当前包中才可以调用嗷！！！
import (
	"go2/GolangStudy/5-init/lib1"
	"go2/GolangStudy/5-init/lib2"
)

func main() {
	lib1.Lib1Test()
	lib2.Lib2Test()
}
```

文件目录结构：

![image-20211014194749257](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211014194749.png)

![image-20211014194943744](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211014194943.png)

GoLand非常强，不用自己导入包，直接输入lib1就有提示的嗷！！！

- lib1中对应的go文件的内容：

```go
package lib1

import "fmt"

func Lib1Test(){
   fmt.Println("Lib1Test()...")
}

func init(){
   fmt.Println("lib1 , init() ...")
}
```







### Import匿名及别名导包方式

- 导入的包如果不使用任何函数，那么会报错，如果我仅仅想要执行某个包中的初始化方法但是又用不到这个包中的任何方法呢？？？

1. 匿名导入包：

```go
package main


//大写的函数才是对外开放的函数，不然的话就只有当前包中才可以调用嗷！！！
import (
	_ "go2/GolangStudy/5-init/lib1"
	"go2/GolangStudy/5-init/lib2"
)

func main() {
	// lib1.Lib1Test()
	lib2.Lib2Test()
}
```

如上就可以使得lib1中的Init()方法执行嗷！！！

如果匿名导入包的话是无法使用这个包的方法的，但是可以使得对应包的初始化方法执行嗷！！！

2. 同理，可以给导入的包起别名嗷！！！

```go
package main


//大写的函数才是对外开放的函数，不然的话就只有当前包中才可以调用嗷！！！
import (
	_ "go2/GolangStudy/5-init/lib1"
	mylib2 "go2/GolangStudy/5-init/lib2"
)

func main() {
	// lib1.Lib1Test()
	mylib2.Lib2Test()
}
```

3. 全部方法的导入方法：

```go
package main


//大写的函数才是对外开放的函数，不然的话就只有当前包中才可以调用嗷！！！
import (
	_ "go2/GolangStudy/5-init/lib1"
	. "go2/GolangStudy/5-init/lib2"
)

func main() {
	// lib1.Lib1Test()
	Lib2Test()
}
```

类似于Python中的`from package import *`

点不要轻易使用，如果两个包中有同名的方法就会发生冲突嗷！！！



### Golang中的指针速通

- 和C语言中的是一样的嗷！！！简单的参数就是传值！！！如果`*a`和C语言是一样的嗷！！！就是传址操作嗷！！！

![image-20211014200036290](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211014200036.png)

- 经典示例代码：

```go
package main

import "fmt"

func swap(a, b int) {
   var temp = a
   a = b
   b = temp
}

func swap2(a, b *int) {
   var temp = *a
   *a = *b
   *b = temp
}

func main() {
   a, b := 10, 20
   swap(a,b)
   fmt.Println("a = ",a)
   fmt.Println("b = ",b)

   //swap
   a,b = 10,20
   swap2(&a,&b)
   fmt.Println("a = ",a)
   fmt.Println("b = ",b)
}
```



### defer语句

- 示例代码：

```go
package main

import "fmt"

func main() {
   //写入defer关键字
   defer fmt.Println("main end1")
   defer fmt.Println("main end2")

   fmt.Println("main::hello go 1")
   fmt.Println("main::hello go 2")
}
```

- defer后面的语句在所有流程结束，代码结束之前执行，类似于Java中的finally，经常用于释放资源。
- defer可以有多个，本质是进行了压栈，所以上面的end1在栈底，end2在栈首，执行的时候先main end2再main end1嗷！！！
- 另外一个Demo：

```go
package main

import "fmt"

func test() {
   fmt.Println("hehe, I am here !!!")
}

func main() {
   //写入defer关键字
   defer fmt.Println("main end")
   defer test()

   fmt.Println("main::hello go 1")
   fmt.Println("main::hello go 2")
}
```

输出结果：

```shell
main::hello go 1
main::hello go 2
hehe, I am here !!!
main end
```

- defer要放在return之前哈，不然defer根本执行不到哇！！！return应该放在最后嗷！！！现在探究的是return和defer的执行顺序：

![image-20211014201307142](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211014201307.png)

很好理解，当程序运行到`}`的时候，才执行deter函数嗷，这个时候return早就执行完了嗷！！！deter是在return后面执行的嗷！！！

- 另外一个小例子：

```go
package main

import "fmt"

func test() {
   fmt.Println("hehe, I am here !!!")
}

func test2() string {
   defer test()
   return "Hello this is the return function"
}

func main() {
   //写入defer关键字
   defer fmt.Println("main end")
   defer test()

   fmt.Println("main::hello go 1")
   fmt.Println("main::hello go 2")

   test2()

   fmt.Println("======================")
}
```

```shell
main::hello go 1
main::hello go 2
hehe, I am here !!!
======================
hehe, I am here !!!
main end
```

可以看到，deter是在 **本方法执行结束之后执行的嗷！！！**



### Go中的Slice

- 相应笔记在代码中哦！！

```go
package main

import (
   "fmt"
)

func printArray(myArray []int) {
   for _,val := range myArray {
      fmt.Println(val)
   }

   myArray[0] = 100
}

func main() {
   //固定长度数组
   var myArray1 [10]int
   myArray2 := [10]int{1,2,3,4}

   //第一种遍历方式
   for i := 0; i < len(myArray1); i++ {
      fmt.Println(myArray1[i])
   }
   
   fmt.Println("================")
   
   //第二种方式
   for index, value := range myArray2 {
      fmt.Println("index = ",index,",value = ",value)
   }

   fmt.Println("================")

   //查看数组的数据类型
   fmt.Printf("myArray1 type = %T\n", myArray1)
   fmt.Printf("myArray2 type = %T\n", myArray2)

   fmt.Println("================")

   //不同长度数组的类型都不一样，那咱怎么传参呢？？？不可能给每个长度的数组定制啊，这也太麻烦了！！！
   //引入我们的动态数组的概念
   //注意，如果传参是myArray [10]int这种形式，本质是传值，如果是myArray []int这种动态数组，本质是传址
   myArray := []int{1,2,3,4} //这个也是动态数组的表示方式哦！！！最大的区别，就是没有写长度嗷！！！
   fmt.Printf("myArray type = %T\n", myArray)
   printArray(myArray)
   fmt.Println("====")
   for _, value := range myArray {
      fmt.Println(value)
   }

   fmt.Println("================")
}
```

- 注意上面哦！！！如果我们写死了传递的数组如：`myArray [4]int`这样的话，本质上也是值拷贝，不好修改元素，也不方便传值嗷！！！由此，动态数组非常重要嗷！！！
- 动态数组在传参上使用的类似于Java的引用传递，而且不同元素长度的动态数组，他们的形参是一致的嗷！！！





#### Slice的声明方式

```go
func main() {
   //第一种哦！！！
   slice1 := []int{1,2,3}
   fmt.Printf("len = %d, slice = %v\n",len(slice1),slice1)

   //第二种
   var slice2 []int //声明slice2是一个切片，但是没有给他分配空间嗷！！！这个时候直接slice[0] = 100是错误的，因为这个空间都不存在嗷！！！
   slice2 = make([]int , 3) // 为这个slice分配空间，默认值都是0哈！！！
   fmt.Printf("len = %d, slice = %v\n",len(slice2),slice2)

   //第三种，声明的同时为其分配空间嗷！！！
   var slice3 []int = make([]int,3)
   fmt.Printf("len = %d, slice = %v\n",len(slice3),slice3)

   //第四种，通过给遍历分配空间，通过:=推导出slice是一个切片
   slice4 := make([]int,3)
   fmt.Printf("len = %d, slice = %v\n",len(slice4),slice4)
}
```

- 常用分配空间方式

```go
if slice == nil{
    slice = make([]int,3)
}
```



#### Slice的使用方式

- 切片容量追加

```go
var numbers = make([]int , 3 , 5)
fmt.Printf("len = %d , cap = %d , slice = %v\n", len(numbers) , cap(numbers) , numbers)
```

![image-20211016155330391](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211016155338.png)ptr指向合法空间末尾，后面两个格子是非法空间，虽然无法访问，但是为位置已经开辟好了嗷！！！

- 追加两个元素：

```go
numbers = append(numbers, 1)
numbers = append(numbers, 2)
```

向numbers中追加两个元素，这个时候numbers已经满了。

- numbers已经满了，再向其中添加内容怎么办？

>go会自动帮助我们再去开辟一段空间，默认容量拓展为原来容量的两倍（切片扩容机制）
>
>![image-20211016155711564](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211016155711.png)

- 如果不指定cap，默认的cap和你分配的len是一样嗷！！！
- 切片的截取：

```go
s := []int{1,2,3}

//第一种方式，startIndex : endIndex
s1 := s[0:2]
//下面这种表示全部元素，还有类似的
s2 := s[:]
s[startIndex:]
s[:endIndex]
```

- 截取本质：

![image-20211016160146239](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211016160146.png)

默认截取指向的底层切片是同一个！！

如果我想要完成copy呢？

`s2 := make([]int , 3)`

`copy(s3, s)`

这个就类似于Java中的浅拷贝和深拷贝，上面这个意思就是将s中的内容依次拷贝到s3中嗷！！！

![image-20211016160418875](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211016160419.png)







### Go中的Map

#### Map的声明方式

```go
func main() {
   //===》 第一种方式：声明myMap1是一个map，key和value都为string，和slice一样，这样声明map底层是没有空间的嗷
   var myMap1 map[string]string
   if myMap1 == nil {
      fmt.Println("myMap1 is an empty map!!!")
   }

   myMap1 = make(map[string]string , 2)
   myMap1["one"] = "java"
   myMap1["two"] = "python"
   myMap1["three"] = "c++"

   //如果我的map赋值赋满了，底层也会自动帮助我们开辟空间，和slice是一样的嗷！！！
   fmt.Printf("%v , %d\n" , myMap1,len(myMap1))

   //===》 第二种方式：可以不指定长度哦，自动会给我们分配长度的
   myMap2 := make(map[int]string)
   myMap2[1] = "java"

   //===》 第三种方式
   myMap3 := map[string]string{
      "one": "php",
      "two": "python",
   }
   fmt.Println(myMap3)
}
```

和Java很像，声明了一个这种变量，一定要初始化（在Go中叫做分配空间），才能够使用嗷！！！



#### Map的使用方式

- 增删改查：

```go
cityMap := make(map[string]string)

//添加
cityMap["china"] = "BeiJing"
cityMap["Japan"] = "Tokyo"
cityMap["USA"] = "NewYork"

//删除
delete(cityMap , "Japan")

//修改
cityMap["USA"] = "DC"

//遍历
for key, value := range cityMap{
    fmt.Println("key = ",key)
    fmt.Println("value = ",value)
}
```

- 定义打印Map的函数

```go
func printMap(cityMap map[string]string){
    for key, value := range cityMap{
        fmt.Println("key = ",key)
        fmt.Println("value = ",value)
    }
}

func changeValue(cityMap map[string]string){
    cityMap["England"] = "London"
}

//调用
printMap(cityMap)
```

这里要注意，调用这个函数的时候，本质上是一个引用传递，实质上传递的是cityMap的指针嗷！！！指向的是同一块空间嗷！！！



### struct的基本定义和使用

- 结构体基本使用，函数定义（传值或传址）

```go
package main

import "fmt"

type myint int

type Book struct {
	title string
	auth string
}

func changeBook(book Book){
	//传递一个Book的副本
	book.auth = "tuzijiejie"
}

func changeBook2(book *Book){
	//传递一个Book的指针
	book.auth = "tuzijiejie"
}

func main() {
	var a myint = 10
	fmt.Println("a = ",a)

	var book1 Book
	book1.title = "Golang"
	book1.auth = "tuzi"

	fmt.Printf("%v\n",book1)

	changeBook(book1)
	fmt.Printf("%v\n",book1)

	changeBook2(&book1)
	fmt.Printf("%v\n",book1)
}
```

#### 类的表示：

- 将结构体和方法分开，此外要注意大小写，大写才是能够被外部访问到的嗷！！！

下面这一个struct搭配上下面专属的方法，构成了一个完整的类嗷！！！

```go
type Hero struct{
    Name string
    Ad int
    Level int
}

//(this Hero)这种是传值嗷！！！打咩，只有*才是传地址，才可以改变属性嗷！！！
func (this *Hero) GetName(){
    fmt.Println("Name = ", this.Name)
}

func (this *Hero) SetName(newName string){
    this.name = newName
}

func (this *Hero) Show(){
    fmt.Println("Name = ",this.Name)
    fmt.Println("Ad = ",this.Ad)
    fmt.Println("Level = ",this.Level)
}
```
this Hero指的是和Hero这个struct绑定，调用这个属性的是Hero对象，实际调用的对象可以用this来指代

- 创建对象：

```go
hero := Hero{Name: "tuzi", Ad: 100 , Level: 1}
hero.Show()

hero.SetName("lisi")
hero.Show()
```

虽然语法没有任何错误，但是所有的方法传入的对象（this Hero），都是该对象的一个副本（拷贝）

因此要改为 **指针** ！！！（this *Hero），这样写，当对象调用的时候，才默认是传递引用嗷！！！

- 大写与小写：

![image-20211016165448638](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211016165449.png)

例如Level -> level，这个只有类中可以访问嗷！！！如果方法是大写，其他的包中是能够访问的。但是如果是小写，只有本包之内能访问这个方法。





#### 类的继承：

```go
package main

import "fmt"

//父类
type Human struct {
   name string
   sex string
}

func (this *Human) Eat(){
   fmt.Println("Human eat ......")
}

func (this *Human) Walk() {
   fmt.Println("Human walk ......")
}

//=====================================================

//子类
type SuperMan struct {
   Human // 这么写就表示继承了Human这个类

   level int
}

//重定义父类方法
func (this *SuperMan) Eat() {
   fmt.Println("SuperHuman eat ......")
}

//子类新方法
func (this *SuperMan) Fly()  {
   fmt.Println("SuperHuman fly ......")
}

func (this *SuperMan) Print()  {
   fmt.Println("========================")
   fmt.Println("name = ",this.name)
   fmt.Println("gender = ",this.sex)
   fmt.Println("level = ",this.level)
}



func main() {
   //定义父类对象
   h := Human{"zhang3","male"}

   h.Eat()
   h.Walk()

   fmt.Println("======================")

   //定义子类对象
   s := SuperMan{Human{"Clerk","male"},100}
   s.Eat() //子类重写的方法
   s.Walk() //父类定义的方法
   s.Fly() //子类新增的方法

   //子类定义对象的另外一种方式
   var s2 SuperMan
   s2.name = "Alexander Liu"
   s2.sex = "male"
   s2.level = 88
   s2.Print()
}
```



### Interface的基本定义和使用

#### 多态的实现：

```go
package main

import "fmt"

//本质上是一个指针
type Animal interface {
   Sleep()
   GetColor() string //获取动物的颜色
   GetType() string //获得动物的种类
}

//具体的类1
type Cat struct {
// 继承接口甚至都不用写继承，直接把接口中的方法全部实现就行了嗷！！！
   color string
}

func (this *Cat) Sleep() {
   fmt.Println("Cat is sleeping")
}

func (this *Cat) GetColor() string {
   return this.color
}

func (this *Cat) GetType() string {
   return "Cat"
}

//具体的类2
type Dog struct {
   // 继承接口甚至都不用写继承，直接把接口中的方法全部实现就行了嗷！！！
   color string
}

func (this *Dog) Sleep() {
   fmt.Println("Dog is sleeping")
}

func (this *Dog) GetColor() string {
   return this.color
}

func (this *Dog) GetType() string {
   return "Dog"
}

//定义接口的方法
func showAnimal(animal Animal)  {
   animal.Sleep()
   fmt.Println("kind = ",animal.GetType())
   fmt.Println("color = ",animal.GetColor())
}

//完全实现了所有的方法，接口的指针才能够具体的实现类
//什么是多态：接口指针指向具体的实现类，由于实现类的不同，导致状态也不一样嗷！！！

func main() {
   var animal Animal

   animal = &Cat{"White"}

   showAnimal(animal)
   fmt.Println("=============")

   animal = &Dog{"Black"}

   showAnimal(animal)
}
```

- 多态的基本要素：
  1. 有一个父类（有接口）。
  2. 有子类（实现了父类的全部接口方法）。
  3. 父类的类型的变量（指针）指向（引用）子类的具体数据变量。





#### 通用万能类型（空接口）：

- interface {}
- 所有的int , string , float64 , struct ......都实现了interface{} ------> 可以使用interface {} 类型引用任意的数据类型。
- 本质上空接口没有方法，也就是所有类型都实现了空接口，因此空接口可以调用任何类型的数据。

```go
package main

import "fmt"

//interface是万能数据类型
func myFunc(arg interface{})  {
   fmt.Println("MyFunc is called ... ")
   fmt.Println(arg)

   //interface{} 如何区分引用的底层数据类型是什么

   //底层有“类型断言机制”
   value , ok := arg.(string)
   if !ok {
      fmt.Println("arg is not string type")
   }else {
      fmt.Println("arg is string , value = ",value)
      fmt.Printf("value type is %T\n",arg)
   }
}

func main() {
   myFunc(0)
   fmt.Println("=============")
   myFunc("0")
}
```

- 类型断言为什么会成功？

底层变量的构造：

![image-20211017102801380](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211017102801.png)

```go
var a string
a = "abc"
//pair<static type:string , value:"abc">

var allType interface{}
allType = a
//pair<type: string , value:"abc">

str , _ := allType.(string)
//这个断言会去查找上面的pair，判断这个type和你的判断类型是否相等嗷！！！
```

具体类型：

![image-20211017103354620](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211017103355.png)

- 另外一个小例子：

![image-20211017103856558](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211017103856.png)

虽然拿着它的Interface一直在变，但是在记录的pair中的数据，始终如一嗷！！！



### Reflect反射机制

- 这里的反射机制要承接上面的pair <type , value>使用哦

- 允许我们把变量的原型给他造出来

常用方式：

![image-20211018192044873](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211018192045.png)

![image-20211018192059582](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211018192059.png)



下面这两个其实就是获得了传入的interface的pair来获得了对应的类型信息。

- 使用代码：

```go
package main

import (
	"fmt"
	"reflect"
)


type User struct {
	Id int
	Name string
	Age int
}

func (this *User) Call() {
	fmt.Println("user is called ... ")
	fmt.Printf("%v\n",this)
}


func reflectNum(arg interface{}){
	fmt.Println("type : ",reflect.TypeOf(arg))
	fmt.Println("value : ",reflect.ValueOf(arg))
}

func DoFiledAndMethod(input interface{}){
	//获取input的type
	inputType := reflect.TypeOf(input)
	fmt.Println("input type is : ",inputType.Name())

	//获取input的value
	inputValue := reflect.ValueOf(inputType).Elem()
	fmt.Println("input value is : ",inputValue)

	//通过type获取里面的字段
	//1. 获取interface的reflect.type，通过type得到NumField，进行遍历
	//2. 得到每个field，数据类型
	//3. 通过field又一个Interface{}方法得到对应的value
	//for i := 0; i < inputType.NumField(); i++ {
	//	field := inputType.Field(i)
	//	value := inputValue.Field(i).Elem()
	//
	//	fmt.Printf("%s : %v = %v\n",field.Name,field.Type,value)
	//}

	//通过type获取里面的方法
	for i := 0; i < inputType.NumMethod(); i++ {
		m := inputType.Method(i)
		fmt.Printf("%s: %v\n",m.Name,m.Type)
	}
}

func main() {
	num := 3.1415926525

	reflectNum(num)

	fmt.Println("===================")

	user := User{
		Id:   22,
		Name: "Alexander liu",
		Age:  19,
	}

	DoFiledAndMethod(user)
}
```



#### 反射解析结构体标签tag

```go
package main

import (
   "fmt"
   "reflect"
)

type resume struct {
   Name string `info:"name" doc:"我的名字"`
   Sex string `info:"sex"`
}

func findTag(str interface{}){
   t := reflect.TypeOf(str).Elem()

   for i := 0; i < t.NumField(); i++ {
      tagString := t.Field(i).Tag.Get("info")
      tagdoc := t.Field(i).Tag.Get("doc")

      fmt.Println("info: ", tagString , " doc: ",tagdoc)
   }
}

func main() {
   var re resume

   findTag(&re)
}
```



#### 结构体标签在JSON中的使用

```go
package main

import (
   "encoding/json"
   "fmt"
)

type Movie struct {
   //后面的json后面就是json中对应的标签
   Title string `json:"title"`
   Year int `json:"year"`
   Price int  `json:"rmb"`
   Actors []string `json:"actors"`
}

func main() {
   movie := Movie{
      Title:  "Avengers",
      Year:   2019,
      Price:  142,
      Actors: []string{"Iron Man","Thor","Hulk","Vision","Captain America"},
   }

   //编码的过程   结构体--->json
   jsonStr, err := json.Marshal(movie)
   if err!=nil {
      fmt.Println("json marshal error",err)
      return
   }

   fmt.Printf("jsonStr = %s\n",jsonStr)
    
    //解码的过程 jsonStr ---> 结构体
    my_movie := Movie{}
    err = json.Unmarshal(jsonStr,&myMovie)
    if err != nil{
        fmt.Println("json unmarshal error", err)
        return
    }
    
    fmt.Printf("%v\n",myMovie)
}
```

- 可以看一下JSON的结果哦！！！

`jsonStr = {"title":"Avengers","year":2019,"rmb":142,"actors":["Iron Man","Thor","Hulk","Vision","Captain America"]}`

可以看到这里的标签就是我们上面的tag中的标签哦！！！和Java相比，多了一种方式嗷！！！











# GoRoutine

## 设计策略：

1. 复用内核级线程

- 多线程，多进程解决了阻塞的问题，但是还是有问题！！！

![image-20211020204519579](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020204527.png)

![image-20211020205213128](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020205213.png)

![image-20211020205234515](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020205235.png)

- go-routine的引入：

![image-20211020205350253](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020205350.png)

![image-20211020205405129](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020205405.png)

M:N关系：

![image-20211020205456336](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020205456.png)

GMP：

G -> goroutine协程

P -> processor处理器

M -> thread线程（内核态线程）

整体结构：

![image-20211020210016275](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020210016.png)



- Work Stealing机制：

![image-20211020210234665](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020210235.png)自己的没了，从旁边的队列偷一个过来执行，偷取机制！！！

- Hand Off机制：

![image-20211020210435926](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020210436.png)

分离机制，一个携程卡死了，CPU里面进行M的切换，队列也发生改变，让阻塞的阻塞去吧，我继续干活了嗷！！！

2. 利用并行

GOMAXPROCS限定P的个数 = CPU核数 / 2

3. 抢占：

![image-20211020210712773](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020210713.png)

4. 全局G队列

![image-20211020210745336](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211020210745.png)



## 创建GoRoutine：

```go
package main

import (
   "fmt"
   "time"
)

func newTask(){
   i := 0
   for {
      i++
      fmt.Printf("new Gouroutine : i = %d\n",i)
      time.Sleep(1 * time.Second)
   }
}

func main() {
   //创建一个go程
   //第一种方式，定义方法，用go去执行
   go newTask()

   //所有的子goroutine空间都是从main给的
   //main退出了，其他的子goroutine都会退出嗷！！！！

   //第二种方法，匿名函数跑跑跑！！！
   go func() {
      defer fmt.Println("B.defer")
      //退出当前GoRoutine，不影响其他Goroutine
      //runtime.Goexit()
      //直接让整个Go程序退出
      //return
      
      fmt.Println("B")
   }()

   //第二种方法（二），匿名+参数
   //这里的返回值是拿不到的嗷！！！
   //本来就是并行的，我都往下走了，我还回头把值返回给你吗？？？？？？
   go func(a int, b int)  {
      fmt.Println(a + b)
   }(10,20)

   i := 0
   for i != 5{
      i ++
      fmt.Printf("             main goroutine : i = %d\n",i)
      time.Sleep(1 * time.Second)
   }
}
```

- [Goroutine中exit函数的使用！！！](https://blog.csdn.net/inthat/article/details/118531656)



## 通信机制Channel：

- 本身就是一种数据类型，具有同步两个goroutine的能力嗷！！！如果队列中没有数据读取数据会阻塞！！！当其中有数据的时候，会通知刚刚阻塞的线程嗷！！！

- 创建channel：

`make(chan Type, capacity)`

```go
func main() {
   //定义一个channel

   c := make(chan int)

   go func() {
      defer fmt.Println("goroutine结束")
      time.Sleep(time.Second*5)
      fmt.Println("goroutine正在执行")
      //发送数据给channel c
      c <- 666
   }()

   //接收c并且赋值给变量num
   //这里会阻塞嗷！！！！因为上面sleep了，channel中啥都没有，主线程就阻塞在这里了嗷！！！
   num := <- c
   fmt.Println("num = ",num)
   fmt.Println("main goroutine 结束...")
}
```



### 无缓冲Channel：

示意图：

![image-20211021190714968](C:\Users\Alexander\AppData\Roaming\Typora\typora-user-images\image-20211021190714968.png)

上面的代码其实就是无缓冲的channel嗷



### 有缓冲Channel：

![image-20211021190750932](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021190751.png)

- channel满了塞数据就会阻塞，channel空了取数据就会阻塞。

```go
func main() {
   c := make(chan int , 3)//带有缓冲的channel

   fmt.Println("len(c) = ",len(c) , " cap(c) = ",cap(c))

   go func() {
      defer fmt.Println("子go程结束")

      for i := 0; i < 4; i++ {
         c <- i
         fmt.Println("子go正在执行，发送的元素是：",i)
      }
   }()

   time.Sleep(2*time.Second)
   for i := 0; i < 3; i++ {
      num := <-c //从c中接受数据
      fmt.Println("num = ",num)
   }

   fmt.Println("main结束了嗷！！！")
}
```

### 关闭Channel：

1. 如果不关闭Channel：

```go
func main() {
   c := make(chan int)

   go func() {
      for i := 0; i < 5; i++ {
         c <- i
      }

      //close可以关闭一个channel
      //close(c)
      
   }()

   for {
      //ok为true说明channel没有关闭，false的话channel已经关闭了嗷！！！
      if data, ok := <- c;ok {
         fmt.Println(data)
      }else {
         break
      }
   }

   fmt.Println("Main Finished ... ")
}
```

![image-20211021191845813](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021191846.png)

因为子goroutine退出了，channel没有关闭，main在取数据的时候阻塞，又没有人叫它起来，gg了。

如果关闭了话！！！

![image-20211021191958941](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021191959.png)

- Channel不像文件一样需要经常去关闭，只有当我们没有任何数据要发送了，才关闭channel
- **关闭channel后再发数据会引发panic，并且接收立即返回零值**。
- **关闭channel后，可以继续从channel接收数据**
- 对于nil channel，无论收发都会被阻塞嗷！！！（比如声明了一个chan但是并没有make，就是nil咯orz）



### Channel和range：

- 迭代操作Channel的时候我们使用range嗷！！！

```go
for data := range c{
   fmt.Println(data)
}
```

非常好用嗷！！！有的话我就取，没有我就等，关了我就收！！！

### Channel和select：

- 单流程下一个go只能监控一个channel的状态，select可以监控多个channel的状态！！！
- select一般和for搭配一起使用嗷！！！

```go
func fibonacci(c , quit chan int){
   x , y := 1,1
   for  {
      select {
      case c <- x:
         //如果c可以写进去，则该case就会进来
         x = y
         y = x + y
      case <- quit:
         //如果quit可以读，则该case就会进来
         fmt.Println("quit")
         return
      }
   }
}

func main() {
   c := make(chan int)
   quit := make(chan int)

   go func() {
      for i := 0; i < 10; i++ {
         fmt.Println( <- c)
      }

      quit <- 0
   }()

   //main go
   fibonacci(c,quit)
}
```

多路Channel的一个监控功能嗷！！！





# GOMODULES

## 工作模式弊端

- Go Modules的出现淘汰了GoPath，例如之前GOPATH要求项目全写在$GOPATH下，很恼火，很不自由嗷！！！

![image-20211021194902854](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021194903.png)

- cmd中输入go env可以查看go的环境变量
- GOPAHT的弊端：
  1. 没有版本控制的概念哈！！！无法指定拉取自己需要的版本的库之类的
  2. 无法同步一致第三方版本号
  3. 无法指定当前项目引用的第三方版本号



## GoModules模式

- `go mod help`可以得到go mod的全部指令的帮助文档。

![image-20211021195641167](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021195641.png)

- go mod环境变量：

![image-20211021195727200](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021195727.png)

- 环境变量讲解：

1. GO111MODULE

![image-20211021195819381](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021195819.png)

![image-20211022114233438](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022114233.png)

2. GOPROXY

![image-20211021200003916](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021200004.png)

这个本质上就是配置镜像源嗷！！！

注意这里七牛云后面还有个`,direct`，或者表示如果在前面的仓库中找不到，就会回源到模块版本的源地址去找（比如Github，如果再找不到的话，就会报错了嗷！！！）

3. GOSUMDB

![image-20211021200730404](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021200730.png)

校验拉取的第三方库是否完整。设置了GOPROXY，我们就不用担心这个原来的设置是外网的库拉，这个GOPROXY就会帮助我们自动校验嗷！！！

4. GONOPROXY/GONOSUMDB/GOPRIVATE

![image-20211021200943334](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021200943.png)

通过设置一个GOPRIVATE即可嗷！！！

![image-20211021201542414](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021201542.png)

- 小总结：

![image-20211021201056360](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211021201056.png)

环境变量设置：

`go env -w 环境变量名=相应参数`



## Go Modules初始化项目

- 步骤：
  1. 创建对应的文件夹
  2. 进入文件夹中执行`go mod init 当前模块名称`

![image-20211022112500525](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022112500.png)

后面导包的时候，项目的整体名称就是这个了嗷！！！

进入初始化好的go.mod文件看一眼

![image-20211022112604894](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022112605.png)

可以看到这个项目的module名称还有go的版本号

- 导入github的项目康康效果？

![image-20211022112734815](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022112735.png)

- 然后我们可以导入对应的包：

1. 导入单个子模块：

![image-20211022112936281](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022112936.png)

直接找到对应的模块go get就可以拉下来了嗷！！！

- 其实可以不用手动down的，你直接跑项目的时候go mod就会帮助我们自动down下来我们需要的依赖了嗷！！！

2. 下载整个模块：

`go get github.com/aceld/zinx`

这样就把对应的项目的包全部都拉下来了嗷！！！

跑一把：

![image-20211022113121334](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022113121.png)

- 导完之后发现当前目录下多了一个go.sum文件嗷！！！
- 而且当我们访问go mod的时候：

![image-20211022113316814](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022113317.png)

间接依赖，这里意味着，我只以来了这个包的子模块，不是整体依赖这个包嗷！！！

- 我们再看看go.sum文件中的内容：

![image-20211022113450155](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022113450.png)

第一个h1 + hash，第二个是gomod + hash。表达一共就只有这两种方式哈！！！

h1 + hash：对于整个包所有文件做哈希，这是为了保证整个包全部文件的完整性。

gomod + hash：表示对于mod文件进行的Hash，确保hash没有问题。

- 刚刚下载的库再哪里呢？

`$GOPATH/pkg`这个包中，有一个mod模块，里面记录了我们的所有的不同的module的依赖包，打开对应的module就能够看到我们在对应的module中下载的依赖包了嗷！！！

- 项目可以在任意的目录中，因为下载位置等都配置好了嗷！！！

- summary：

![image-20211022114629646](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022114630.png)

![image-20211022114810294](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022114810.png)



## 改变模块依赖关系

- 比如确定要使用某个版本的依赖，这个版本才有对应的依赖和资源。
- 第一：可以更改go mod中的版本依赖，改回某个老版本。
- 第二：使用指令进行版本替换：

![image-20211022145950537](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022145950.png)

![image-20211022150021521](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022150021.png)

![image-20211022150124165](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022150124.png)

你需要找到$GOPATH下对应的下载的包和对应的版本，然后进行replace替换操作嗷！！！





# 即时通信系统小项目

- 技术结构：

![image-20211022150258190](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211022150258.png)

OnlineMap：记录在线用户

OnlineMap旁边的channel：进行广播channel



## 版本一：构建基础Server

- 新建一个文件夹

1. 建立server.go文件，写Server这个类，这个类包含三个大方法和一个重要的结构：
   - Server类型的定义
   - 创建一个Server对象方法
   - 启动一个Server服务方法
   - 处理一个连接方法

- server.go：

```go
package main

import (
   "fmt"
   "net"
)

// Server 这个Server就是用于定义对应的结构体和方法的嗷！！！
type Server struct {
   Ip string
   Port int
}

// NewServer 创建一个server的接口
func NewServer(ip string, port int) *Server {
   server := &Server{
      Ip: ip,
      Port: port,
   }

   return server
}

func (this *Server) Handler(conn net.Conn)  {
   //...用于对接当前链接的业务
   fmt.Println("连接建立成功")
}


// Start 启动服务器的端口
func (this *Server) Start() {
   // socket listen
   listener , err := net.Listen("tcp",fmt.Sprintf("%s:%d",this.Ip,this.Port))
   if err != nil {
      fmt.Println("net.Listen error : ",err)
      return
   }

   //close listen socket
   defer listener.Close()

   for {
      //accept
      conn , err := listener.Accept()
      if err != nil {
         fmt.Println("listener accept error : ",err)
         continue
      }

      //do handler
      go this.Handler(conn)
   }
}
```

- main.go:

```go
package main

func main() {
   server := NewServer("127.0.0.1",8888)
   server.Start()
}
```







## 版本二：用户上线功能

还需要补充OnlineMap和Message这两个属性：
- OnlineMap：记录哪些用户在线。每一个用户实际上都绑定一个channel，这个channel是用于给client发消息的嗷，用户在监听channel，如果有数据会立刻把数据给client！！！
- Message：本质上是server的一个channel，把消息广播嗷！！！

- 流程图：

![image-20211023094951712](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211023094952.png)

- 重点在于广播功能的实现：

新增User.go：

```go
package main

import "net"

type User struct {
   Name string
   Addr string
   C chan string
   conn net.Conn
}

// NewUser 创建一个用户的API
func NewUser(conn net.Conn) *User{
   userAddr := conn.RemoteAddr().String()

   user := &User{
      Name: userAddr,
      Addr: userAddr,
      C: make(chan string),
      conn: conn,
   }

   //启动监听当前user channel消息的goroutine
   go user.ListenMessage()

   return user
}

// ListenMessage 监听当前User channel的方法，一旦有消息就直接发送给client
func (this *User) ListenMessage() {
   for {
      msg := <- this.C

      this.conn.Write([]byte(msg+"\n"))
   }
}
```

修改server.go

```go
package main

import (
   "fmt"
   "net"
   "sync"
)

// Server 这个Server就是用于定义对应的结构体和方法的嗷！！！
type Server struct {
   Ip string
   Port int

   //子啊先用户的列表
   OnlineMap map[string]*User
   mapLock sync.RWMutex

   //消息广播的channel
   Message chan string
}

// NewServer 创建一个server的接口
func NewServer(ip string, port int) *Server {
   server := &Server{
      Ip: ip,
      Port: port,
      OnlineMap: make(map[string]*User),
      Message: make(chan string),
   }

   return server
}

// BroadCast 广播消息方法
func (this *Server) BroadCast(user *User, msg string)  {
   sendMsg := "[" + user.Addr + "]" + user.Name + ":" + msg
   this.Message <- sendMsg
}

// ListenMessage 监听Message广播消息channel的goroutine,一旦有消息就发送给全部的在线User
func (this *Server) ListenMessage() {
   for {
      msg := <- this.Message

      //将message发送给全部的在线User
      this.mapLock.Lock()

      for _, cli := range this.OnlineMap{
         cli.C <- msg
      }

      this.mapLock.Unlock()
   }
}


// Handler 处理连接的方法
func (this *Server) Handler(conn net.Conn)  {
   //...用于对接当前链接的业务
   //fmt.Println("连接建立成功")

   user := NewUser(conn)

   //用户上线，把用户加入到onlineMap中，并且广播消息
   this.mapLock.Lock()
   this.OnlineMap[user.Name] = user
   this.mapLock.Unlock()

   //广播当前用户上线消息
   this.BroadCast(user,"已经上线")

   //当前handler阻塞，为了防止这个死了之后，go程就挂了
   select {
   }
}


// Start 启动服务器的端口
func (this *Server) Start() {
   // socket listen
   listener , err := net.Listen("tcp",fmt.Sprintf("%s:%d",this.Ip,this.Port))
   if err != nil {
      fmt.Println("net.Listen error : ",err)
      return
   }

   //close listen socket
   defer listener.Close()

   //启动监听Message的goroutine
   go this.ListenMessage()

   for {
      //accept
      conn , err := listener.Accept()
      if err != nil {
         fmt.Println("listener accept error : ",err)
         continue
      }

      //do handler
      go this.Handler(conn)
   }
}
```





## 版本三：用户消息广播机制

- 其实上面已经完成了相关的广播功能的编写了
- 我们接下来需要的就是编写对应的函数，把相应的用户消息放入我们的消息队列中即可



## 版本四：用户业务层封装

- 把代码进行合并，整理成函数嗷，是谁负责的，例如用户的登录和退出，就封装在哪个类的方法中嗷！！！！同时通过数据结构中的指针，把用户和Service串起来，方便我们的操作嗷！！！

```go
//用户的上线业务
func (this *User) Online() {

   //用户上线,将用户加入到onlineMap中
   this.server.mapLock.Lock()
   this.server.OnlineMap[this.Name] = this
   this.server.mapLock.Unlock()

   //广播当前用户上线消息
   this.server.BroadCast(this, "已上线")
}

//用户的下线业务
func (this *User) Offline() {

   //用户下线,将用户从onlineMap中删除
   this.server.mapLock.Lock()
   delete(this.server.OnlineMap, this.Name)
   this.server.mapLock.Unlock()

   //广播当前用户上线消息
   this.server.BroadCast(this, "下线")

}

//用户处理消息的业务
func (this *User) DoMessage(msg string) {
   this.server.BroadCast(this, msg)
}
```

上面就是重构后提取出来的用户的业务嗷！！！

```go
func (this *Server) Handler(conn net.Conn) {
   //...当前链接的业务
   //fmt.Println("链接建立成功")

   user := NewUser(conn, this)

   user.Online()

   //接受客户端发送的消息
   go func() {
      buf := make([]byte, 4096)
      for {
         n, err := conn.Read(buf)
         if n == 0 {
            user.Offline()
            return
         }

         if err != nil && err != io.EOF {
            fmt.Println("Conn Read err:", err)
            return
         }

         //提取用户的消息(去除'\n')
         msg := string(buf[:n-1])

         //用户针对msg进行消息处理
         user.DoMessage(msg)
      }
   }()

   //当前handler阻塞
   select {}
}
```

上面就是重构后提取出来的service的业务嗷！！！

以及把User和Server（正向一对一，反向一对多），分别使用指针和Map的方式将两者穿起来，方便我们后续的操作嗷！！！



## 版本五：在线用户查询

- 添加在用户的DoMessage方法中，对于用户输入的消息进行判断，如果为"who"，就返回所有用户信息即可嗷！！！

```go
//专门用于给当前用户发消息的嗷！！！
func (this *User) SendMsg(msg string) {
   this.conn.Write([]byte(msg))
}

//用户处理消息的业务
func (this *User) DoMessage(msg string) {
   if msg == "who"{
      //查询在线的用户都有哪些
      this.server.mapLock.Lock()
      for _, user := range this.server.OnlineMap{
         onlineMsg := "[" + user.Addr + "]" + user.Name + ":" + "在线...\n"
         this.SendMsg(onlineMsg)
      }
      this.server.mapLock.Unlock()

   } else {
      this.server.BroadCast(this, msg)
   }
}
```



## 版本六：修改用户名

- 定义一下：例如：`rename|兔子`就是当前用户名重命名为兔子！！！
- 和上面一样，都要使用DoMessage来判断处理

```go
//专门用于给当前用户发消息的嗷！！！
func (this *User) SendMsg(msg string) {
   this.conn.Write([]byte(msg))
}

//用户处理消息的业务
func (this *User) DoMessage(msg string) {
   if msg == "who"{
      //查询在线的用户都有哪些
      this.server.mapLock.Lock()
      for _, user := range this.server.OnlineMap{
         onlineMsg := "[" + user.Addr + "]" + user.Name + ":" + "在线...\n"
         this.SendMsg(onlineMsg)
      }
      this.server.mapLock.Unlock()

   }else if len(msg) > 7 && msg[:7] == "rename|" {
      //消息格式：rename|张山
      newName := strings.Split(msg,"|")[1]

      //判断name是否存在
      _, ok := this.server.OnlineMap[newName]
      if ok {
         this.SendMsg("当前用户名被使用了！！！\n")
      }else {
         //修改服务器端的名字
         this.server.mapLock.Lock()
         delete(this.server.OnlineMap,this.Name)
         this.server.OnlineMap[newName] = this
         this.server.mapLock.Unlock()

         //修改用户端的名字
         this.Name = newName
         this.SendMsg("您已经成功更新用户名为：" + this.Name + "\n")
      }
   } else {
      this.server.BroadCast(this, msg)
   }
}
```





## 版本七：超时强踢功能

- 首先应该记录一个用户的在线时间嗷！！！
- 用到了select语法之类的 ，用到了两个channel嗷！！！

```go
func (this *Server) Handler(conn net.Conn) {
    //...当前链接的业务
    //fmt.Println("链接建立成功")
 
    user := NewUser(conn, this)
 
    user.Online()
 
    //监听用户是否活跃的channel
    isLive := make(chan bool)
 
    //接受客户端发送的消息
    go func() {
        buf := make([]byte, 4096)
        for {
            n, err := conn.Read(buf)
            if n == 0 {
                user.Offline()
                return
            }
 
            if err != nil && err != io.EOF {
                fmt.Println("Conn Read err:", err)
                return
            }
 
            //提取用户的消息(去除'\n')
            msg := string(buf[:n-1])
 
            //用户针对msg进行消息处理
            user.DoMessage(msg)
 
            //用户的任意消息，代表当前用户是一个活跃的
            isLive <- true
        }
    }()
 
    //当前handler阻塞
    for {
        select {
        case <-isLive:
            //当前用户是活跃的，应该重置定时器
            //不做任何事情，为了激活select，更新下面的定时器
 
        case <-time.After(time.Second * 10):
            //已经超时
            //将当前的User强制的关闭
 
            user.SendMsg("你被踢了")
 
            //销毁用的资源
            close(user.C)
 
            //关闭连接
            conn.Close()
 
            //退出当前Handler
            return //runtime.Goexit()
        }
    }
}
```





## 版本八：私聊功能

- 和之前一样，修改DOMESSAGE，类似于自己定义通信协议嗷！！！
- 例如：`to|用户名|信息`这种方式，进行判断即可嗷！！！从全局的用户列表中，找到对应的用户，把他的Channel拿出来，把我要发的信息塞进去即可嗷！！！

```go
//用户处理消息的业务
func (this *User) DoMessage(msg string) {
    if msg == "who" {
        //查询当前在线用户都有哪些
 
        this.server.mapLock.Lock()
        for _, user := range this.server.OnlineMap {
            onlineMsg := "[" + user.Addr + "]" + user.Name + ":" + "在线...\n"
            this.SendMsg(onlineMsg)
        }
        this.server.mapLock.Unlock()
 
    } else if len(msg) > 7 && msg[:7] == "rename|" {
        //消息格式: rename|张三
        newName := strings.Split(msg, "|")[1]
 
        //判断name是否存在
        _, ok := this.server.OnlineMap[newName]
        if ok {
            this.SendMsg("当前用户名被使用\n")
        } else {
            this.server.mapLock.Lock()
            delete(this.server.OnlineMap, this.Name)
            this.server.OnlineMap[newName] = this
            this.server.mapLock.Unlock()
 
            this.Name = newName
            this.SendMsg("您已经更新用户名:" + this.Name + "\n")
        }
 
    } else if len(msg) > 4 && msg[:3] == "to|" {
        //消息格式:  to|张三|消息内容
 
        //1 获取对方的用户名
        remoteName := strings.Split(msg, "|")[1]
        if remoteName == "" {
            this.SendMsg("消息格式不正确，请使用 \"to|张三|你好啊\"格式。\n")
            return
        }
 
        //2 根据用户名 得到对方User对象
        remoteUser, ok := this.server.OnlineMap[remoteName]
        if !ok {
            this.SendMsg("该用户名不不存在\n")
            return
        }
 
        //3 获取消息内容，通过对方的User对象将消息内容发送过去
        content := strings.Split(msg, "|")[2]
        if content == "" {
            this.SendMsg("无消息内容，请重发\n")
            return
        }
        remoteUser.SendMsg(this.Name + "对您说:" + content)
 
    } else {
        this.server.BroadCast(this, msg)
    }
}
```





## 版本九：客户端实现

- 新建Client.go嗷！！！

```go
package main
 
import (
    "flag"
    "fmt"
    "io"
    "net"
    "os"
)
 
type Client struct {
    ServerIp   string
    ServerPort int
    Name       string
    conn       net.Conn
    flag       int //当前client的模式
}
 
func NewClient(serverIp string, serverPort int) *Client {
    //创建客户端对象
    client := &Client{
        ServerIp:   serverIp,
        ServerPort: serverPort,
        flag:       999,
    }
 
    //链接server
    conn, err := net.Dial("tcp", fmt.Sprintf("%s:%d", serverIp, serverPort))
    if err != nil {
        fmt.Println("net.Dial error:", err)
        return nil
    }
 
    client.conn = conn
 
    //返回对象
    return client
}
 
//处理server回应的消息， 直接显示到标准输出即可
func (client *Client) DealResponse() {
    //一旦client.conn有数据，就直接copy到stdout标准输出上, 永久阻塞监听
    io.Copy(os.Stdout, client.conn)
}
 
func (client *Client) menu() bool {
    var flag int
 
    fmt.Println("1.公聊模式")
    fmt.Println("2.私聊模式")
    fmt.Println("3.更新用户名")
    fmt.Println("0.退出")
 
    fmt.Scanln(&flag)
 
    if flag >= 0 && flag <= 3 {
        client.flag = flag
        return true
    } else {
        fmt.Println(">>>>请输入合法范围内的数字<<<<")
        return false
    }
 
}
 
//查询在线用户
func (client *Client) SelectUsers() {
    sendMsg := "who\n"
    _, err := client.conn.Write([]byte(sendMsg))
    if err != nil {
        fmt.Println("conn Write err:", err)
        return
    }
}
 
//私聊模式
func (client *Client) PrivateChat() {
    var remoteName string
    var chatMsg string
 
    client.SelectUsers()
    fmt.Println(">>>>请输入聊天对象[用户名], exit退出:")
    fmt.Scanln(&remoteName)
 
    for remoteName != "exit" {
        fmt.Println(">>>>请输入消息内容, exit退出:")
        fmt.Scanln(&chatMsg)
 
        for chatMsg != "exit" {
            //消息不为空则发送
            if len(chatMsg) != 0 {
                sendMsg := "to|" + remoteName + "|" + chatMsg + "\n\n"
                _, err := client.conn.Write([]byte(sendMsg))
                if err != nil {
                    fmt.Println("conn Write err:", err)
                    break
                }
            }
 
            chatMsg = ""
            fmt.Println(">>>>请输入消息内容, exit退出:")
            fmt.Scanln(&chatMsg)
        }
 
        client.SelectUsers()
        fmt.Println(">>>>请输入聊天对象[用户名], exit退出:")
        fmt.Scanln(&remoteName)
    }
}
 
func (client *Client) PublicChat() {
    //提示用户输入消息
    var chatMsg string
 
    fmt.Println(">>>>请输入聊天内容，exit退出.")
    fmt.Scanln(&chatMsg)
 
    for chatMsg != "exit" {
        //发给服务器
 
        //消息不为空则发送
        if len(chatMsg) != 0 {
            sendMsg := chatMsg + "\n"
            _, err := client.conn.Write([]byte(sendMsg))
            if err != nil {
                fmt.Println("conn Write err:", err)
                break
            }
        }
 
        chatMsg = ""
        fmt.Println(">>>>请输入聊天内容，exit退出.")
        fmt.Scanln(&chatMsg)
    }
 
}
 
func (client *Client) UpdateName() bool {
 
    fmt.Println(">>>>请输入用户名:")
    fmt.Scanln(&client.Name)
 
    sendMsg := "rename|" + client.Name + "\n"
    _, err := client.conn.Write([]byte(sendMsg))
    if err != nil {
        fmt.Println("conn.Write err:", err)
        return false
    }
 
    return true
}
 
func (client *Client) Run() {
    for client.flag != 0 {
        for client.menu() != true {
        }
 
        //根据不同的模式处理不同的业务
        switch client.flag {
        case 1:
            //公聊模式
            client.PublicChat()
            break
        case 2:
            //私聊模式
            client.PrivateChat()
            break
        case 3:
            //更新用户名
            client.UpdateName()
            break
        }
    }
}
 
var serverIp string
var serverPort int
 
//./client -ip 127.0.0.1 -port 8888
func init() {
    flag.StringVar(&serverIp, "ip", "127.0.0.1", "设置服务器IP地址(默认是127.0.0.1)")
    flag.IntVar(&serverPort, "port", 8888, "设置服务器端口(默认是8888)")
}
 
func main() {
    //命令行解析
    flag.Parse()
 
    client := NewClient(serverIp, serverPort)
    if client == nil {
        fmt.Println(">>>>> 链接服务器失败...")
        return
    }
 
    //单独开启一个goroutine去处理server的回执消息
    go client.DealResponse()
 
    fmt.Println(">>>>>链接服务器成功...")
 
    //启动客户端的业务
    client.Run()
}
```

- Init的使用，Init是一个非常有意思的东西，可以指定某些参数的值为定值，同时init()会在main之前执行嗷！！！

![image-20211025154843964](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211025154852.png)





# Golang生态拓展介绍

1. Web框架：
   - beego
   - gin
   - echo
   - lris
2. 微服务框架：
   - Go kit
   - Istio
3. 容器编排：
   - K8S
   - swarm
4. 服务发现：
   - Consul
5. 存储引擎：
   - etcd
   - tidb
6. 静态建站：
   - hugo
7. 中间键：
   - nsg
   - zinx
   - Leaf
   - gRPC
   - codis
8. 爬虫框架：
   - go query

