---
title: Go语言自学
date: 2021-02-20 16:05:21
tags: 
	后端


---



# Go是大二比较系统的学习的后端语言，没有学习完，但是对于redis数据库等之类的后端内容有了相关的了解，后面还会更新完善

---



<!--more-->


- 
- - 包，变量和函数:

  - 1. 程序都要从main包开始运行，所以一直都要package main

    ```go
    package main
    
    import(
    	"fmt"
    	"math/rand"
    )
    ```

    包名与导入路径的最后一个元素一致。例如，`"math/rand"` 包中的源码均以 `package rand` 语句开始。

    2. 程度的导入:

    两种形式，如上一种，两句import导入两个语句也可以。

    3. 导出名:

    如果一个名字以大写字母开头，那么他是已经导出的:

    例如: `path.pi`这个东西就不存在，`math.Pi`就是包内存在可以导出的东西，导出名一定要是大写的，记住嗷呜!

    4. 函数可以没有参数或者接受多个参数，类型在变量名之后

    ```go
    package main
    
    import "fmt"
    
    func add(x int, y int) int {
    	return x + y
    }
    
    func main() {
    	fmt.Println(add(42, 13))
    }
    
    ```

    5. 当参数类型相同时，可以只写一个type,`func add(x,y int) int`
    6. 和python一样，go可以同时返回多个函数值，但是在定义的时候就得有所作为嗷！

    ```go
    package main
    
    import "fmt"
    
    func swap(a,b string) (string,string){
        return b,a
    }
    
    func main(){
        a, b := swap("hello","world")
        fmt.Println(a,b)
    }
    ```

    7. go的返回值也可以被命名:

    ```go
    func split(sum int) (x, y int) {
    	x = sum * 4 / 9
    	y = sum - x
    	return
    }
    ```

    直接返回函数一般只用于这样的小函数，命名了返回为x,y

    就可以在函数中使用x,y。但是一般不用于长函数中，不然容易影响代码可读性

    8. var变量用于声明一个变量列表，和函数的参数列表一样，type在最后

    ```go
    var c, python, java bool
    
    func main() {
    	var i int
    	fmt.Println(i, c, python, java)
    }
    
    ```

    单个元素的声明也得用 `var + name + type`这种形式来声明。

    9. 变量初始化:

    ```go
    var i,j int = 1,2
    ```

    如果初始化类型值已经存在，则会省略类型，直接从初始值获得初值。

    10. 在函数中，简洁赋值语句 `:=` 可在类型明确的地方代替 `var` 声明。

    直接 k:=3 即可成功赋值。

    11. 没有初始化的元素系统会自动赋予它们零值，对于字符串为空串，和java中类似。
    12. 类型转换：

    ```go
    i := 42
    f := float64(i)
    u := uint(f)
    ```

    13. 当应用上述赋值方法时，左边的类型只能由右边推导出来。
    14. 常量比较特殊，声明的时候要用const来修饰，而且常量声明时不能用:=声明, 顺带一提，可以不加类型去声明一个常量，常量的const相当于一般变量的var,常量的=也可以直接赋值使用。

    tips: 经过在线的实现发现，对于变量进行赋值时，可以不声明它的类型 ， 它的type由等号右边的值来决定。但是如果不进行赋值，就得声明type，不然系统自动的初始化没法进行。



- 流程控制语句:

  - 1. 循环:

    - 1. go只有for循环这一种循环结构哦

        ```go
        package main
        
        import "fmt"
        
        func main() {
        	sum := 0
        	for i := 0; i < 10; i++ {
        		sum += i
        	}
        	fmt.Println(sum)
        }
        
        ```

        更加深入的理解：这个i有大问题啊，i := 0实际上说明了两件事儿，以见时i是一个变量，另一个是i的初始化值为1， go里面的循环没有小括号，但是for中循环的代码块任然要有大括号。

        2. for语句中，初始化语句和后置语句是可选的，经过测试知，实际上用法和其他语言中的循环用法没啥差别嗷！
        3. 甚至可以去掉分号，这个时候的while就等同于go中的for

        ```go
        for sum < 1000 {
        		sum += sum
        	}
        ```

        4. 同样有无限循环的概念哦

  - 2. if和switch语句:

    - 1. 和for语句一样，无需小括号但是大括号是必须的

      2. 同 `for` 一样， `if` 语句可以在条件表达式前执行一个简单的语句。

         该语句声明的变量作用域仅在 `if` 之内。

      ```go
      func pow(x, n, lim float64) float64 {
      	if v := math.Pow(x, n); v < lim {
      		return v
      	}
      	return lim
      }
      ```

      总结，这里在if的条件中引入了变量v有利于辅助if判断，这个变量的作用域在if之中，可以像for一样声明一个变量。

      3. 这里if中声明的变量在else中也能使用哦，出了if-else的判断就不能使用了。
      4. go中的switch语句和其余的语言差不多，不同之处在于go已经帮忙编写好了每一个case后面对应的break语句，如果不是用fallthrough语句，分支会自动终止，同时case不用为常量，且取值也不一定为整数。
      5. switch求值顺序:

      ```go
      package main
      
      import (
      	"fmt"
      	"time"
      )
      
      func main() {
      	fmt.Println("When's Saturday?")
      	today := time.Now().Weekday()
      	switch time.Saturday {
      	case today + 0:
      		fmt.Println("Today.")
      	case today + 1:
      		fmt.Println("Tomorrow.")
      	case today + 2:
      		fmt.Println("In two days.")
      	default:
      		fmt.Println("Too far away.")
      	}
      }
      
      ```

      相当于用变量作为了衡量的标准去和一个常量相互匹配，这样使得整个程序的意义更加明显，如果case中含有函数的话，当程序没有执行到某个case时，这个函数是不会被程序执行的。

      6. go中的switch感觉有点东西，可以起到类似于if-then-else这样的东西的作用，用于多段的匹配很适合 switch可以不加任何的参数，变量的判定可以统一放在case里面添加。

      ```go
      func main() {
      	t := time.Now()
      	switch {
      	case t.Hour() < 12:
      		fmt.Println("Good morning!")
      	case t.Hour() < 17:
      		fmt.Println("Good afternoon.")
      	default:
      		fmt.Println("Good evening.")
      	}
      }
      
      ```

  - 3. defer语句等:

    - 1. defer语句会将函数推迟到外层函数返回之后执行
2. defer本质上会按照顺序将要执行的语句压栈，等到其余语句执行完之后，在从栈中以此pop出要执行的语句执行。
      3. 类似于在python中，如果打开了一个文件并且没有关闭，可能会导致后续对于文件，可以通过关闭文件来避免这个问题。但是同时，如果函数非常多的话，可能就会出现问题，不好处理。在python中，我们有类似于with open(filepath) as filename的方法来确定文件的关闭，在go中，我们也有类似的方法可以解决这样的问题，要用到go语言中的defer语句。通过在打开每个文件的时候，添加一个defer src.Close()文件，确保文件最后会被关闭。
4. defer语句的用途和应用都挺多的，可以以后再去补充这些内容嗷！



- 更多类型: struct，slice 和映射

  - 指针:

    - go语言内也拥有pointer ， 指针保存了值的内存地址
    - *T是指向T类型的执政，其零值为nil ` var p *int`
    - & 操作符会产生一份指向其操作数的指针。

    ```go
    var p *int
    i = 42
    p = &i
    ```

    - *p也和c一样是间接应用，但是go里面没有指针运算。
    
  - 结构体:

    - ```go
      type Vertex struct {
      	X int
      	Y int
      }
      ```

    - 和变量的声明有点类似，先指明类型为一个type类型，然后指明为struct结构，其余和c没啥区别。

    - 对于一个struct的初始化:

    ```go
    v := Vertex{1,2}
    ```

    - 对于struct中某个field的访问: `v.X | v.Y`可以访问和修改特殊的元素，和c中一样。
    - 访问结构体指针对应的对象，可以写成(*p).X这种形式，同时也允许我们使用隐式简介应用，写成p.X就可以啦。
    - 一些对应的操作:

    ```go
    var (
    	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
    	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予
    	v3 = Vertex{}      // X:0 Y:0
    	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
    )
    ```

  - 数组:

    - 初始化一个数组: `var a [10]int`

    ```go
    func main() {
    	var a [2]string
    	a[0] = "Hello"
    	a[1] = "World"
    	fmt.Println(a[0], a[1])
    	fmt.Println(a)
    
    	primes := [6]int{2, 3, 5, 7, 11, 13}
    	fmt.Println(primes)
    }
    ```

    - 数组的切片，和py没啥差别:

    ```
    func main() {
    	primes := [6]int{2, 3, 5, 7, 11, 13}
    
    	var s []int = primes[1:4]
    	fmt.Println(s)
    }
    ```

    这里用到了数组并没有给具体的长度，长度由后面决定。

    - 与py不同，go中的切片是对于底层数组的描述，与其他元素共享，一旦修改改的就是底层元素，所有相关的切片和原数组都会观测到这些修改
    - 切片文法:

    切片文法类似于没有长度的数组文法。

    这是一个数组文法：

    ```
    [3]bool{true, true, false}
    ```

    下面这样则会创建一个和上面相同的数组，然后构建一个引用了它的切片：

    ```
    []bool{true, true, false}
    ```

    - 切片默认下限是0，上限时切片长度，用法和py一样
    - 切片的长度与容量，len(s)和cap(s)来获取

    长度：所包含的元素个数

    容量：从第一个元素开始数，到底层数组元素末尾的个数

    ```go
    package main
    
    import "fmt"
    
    func main() {
    	s := []int{2, 3, 5, 7, 11, 13}
    	printSlice(s)
    
    	// 截取切片使其长度为 0
    	s = s[:0]
    	printSlice(s)
    
    	// 拓展其长度
    	s = s[:4]
    	printSlice(s)
    
    	// 舍弃前两个值
    	s = s[2:]
    	printSlice(s)
    }
    
    func printSlice(s []int) {
    	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
    }
    
    
    len=6 cap=6 [2 3 5 7 11 13]
    len=0 cap=6 []
    len=4 cap=6 [2 3 5 7]
    len=2 cap=4 [5 7]
    ```

    - 切片的零值是 `nil`，nil 切片的长度和容量为 0 且没有底层数组。
    - 切片可以用内建函数make来创建，这也是你创建动态数组的方式:

    ```go
    a := make([]int, 5)  // len(a)=5
    b := make([]int, 0, 5) // len(b)=0, cap(b)=5
    
    b = b[:cap(b)] // len(b)=5, cap(b)=5
    b = b[1:]      // len(b)=4, cap(b)=4
    ```

    - 切片有点像list,其中也可以包含其他的切片
    - 向切片追加元素: append函数，和py没啥区别，结果是一个新的切片，换言之，切片实现了数组的动态增长。

    
    
  - Range:

    - 1. `for` 循环的 `range` 形式可遍历切片或映射。

         当使用 `for` 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

         ```go
         var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
         
         func main() {
         	for i, v := range pow {
         		fmt.Printf("2**%d = %d\n", i, v)
         	}
         }
         ```

         下标从0开始，注意第二个值为一个副本，不影响原来的值。

      2. 注意我们可以使用下标来忽略它，例如:

      ```go
      for i, _ := range pow
      for _, value := range pow
      ```

      如果你只需要索引，甚至可以`for i := range pow`

      直接忽略第二个变量。

    
    
  - 映射: (map)

    - 映射的零值为nil

    - make函数会返回给定类型的映射，并将其初始化备用。

    - 有点像py中字典的感觉

    - 具体操作及用法:

      - 1. ```go
           var m map[string]Vertex
           
           func main() {
           	m = make(map[string]Vertex)
           	m["Bell Labs"] = Vertex{
           		40.68433, -74.39967,
           	}
           	fmt.Println(m["Bell Labs"])
           }
           ```

      用make关键字来返回给定类型的映射

      - 映射的文法与结构体相似，不过必须有键名

      ```go
      type Vertex struct {
      	Lat, Long float64
      }
      
      var m = map[string]Vertex{
      	"Bell Labs": Vertex{
      		40.68433, -74.39967,
      	},
      	"Google": Vertex{
      		37.42202, -122.08408,
      	},
      }
      
      func main() {
      	fmt.Println(m)
      }
      ```

      - 若顶级类型中只有一个类型名，你可以在文法的元素中省略它。
      - 修改映射:

      1.  初始化过程中用make    如果你事先声明了变量就可以直接用`=`,如果你没有实现声明变量，就要用`:=`
      2. 构造了`m := make(map[string]int)`之后，就可以类似于字典访问元素，给元素赋值
      3. 删除元素要delete(m, key)
      4. 双赋值检测某个键是否存在: elem, ok = m[key]

      如果key在，那ok为ture; 如果key不在，那么ok为false, elem即为映射元素类型的零值

      5. **注** ：若 `elem` 或 `ok` 还未声明，你可以使用短变量声明：

         ```go
         elem, ok := m[key]
         ```

  - 函数值: 

    - 函数也是值，可以像其他值一样传递。
    - 可以返回，可以传递，也可以赋值嗷。

    ```go
    func compute(fn func(float64, float64) float64) float64 {
    	return fn(3, 4)
    }
    ```

    - go中把函数的定义和函数赋予变量两个工作分开了，使得整体更加有效率。
    - 函数的闭包：
      - go函数可以是一个闭包，闭包是一个函数值，可以应用函数体之外的变量，可以访问并且赋予其引用变量的值。函数被这些变量“绑定”在一起。

    ```go
    func adder() func(int) int {
    sum := 0
    return func(x int) int {
    	sum += x
    	return sum
    }
    }
    func main() {
  pos, neg := adder(), adder()
  for i := 0; i < 10; i++ {
  	fmt.Println(
  		pos(i),
  		neg(-2*i),
  		)
  	}
  }
    
    ```
  ```
    
  - 关于闭包还有一些知识在课后习题中嗷！

  
  
  ```




- 方法和接口:

  - Go没有类，不过可以为结构体类型定义方法，方法接收者在它自己的参数列表内，位于func关键字和方法名之间

    ```go
    type Vertex struct {
    	X, Y float64
    }
    
    func (v Vertex) Abs() float64 {
    	return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }
    
    func main() {
    	v := Vertex{3, 4}
    	fmt.Println(v.Abs())
    }
    ```

    有类那味儿了啊，这个东西你考虑一下子，本质上就是传入了一个struct进行操作，然后这个struct就可以调用定义的对应的方法。

  - remember！！！！！: 方法只是个带接收者参数的函数

  同一个函数既可以写成正常的函数，也可以写成一种类型的方法之类的东西，同时调用的方法也会有所区别。

  - 方法不一定是结构体特有的，把`v Vertex`这个地方换成别的类型的东东也可以成为别的type的方法。你只能为同一包内定义的type定义接收这声明方法，而不能为其他包内定义的类型，即接受者的类型定义和方法声明必须在同一包内。

  - 指针和函数：

    - 1. 带指针参数的函数必须接受一个指针
      2. 而如果是定义的指针的方法，对于对象和指针两者都能被接受哦，为方便起见，Go 会将语句 `v.Scale(5)` 解释为 `(&v).Scale(5)`。
    - 其实上述两点说明了方法的好处

  - 方法和指针的重定向：

    - 接受一个值作为参数的函数必须接受一个指定类型的值
    - 而以值为接受者的方法被调用时，接收者技能为值又能为指针
    - 总之，定义为方法条件更加宽松灵活，调用 `p.Abs()` 会被解释为 `(*p).Abs()`。

  - 选择值或者指针作为接收者，原因有两个:

    - 首先，方法能改变接收者指向的值
    - 其次，避免复制该值，当值为大型结构体时，会更加高效，可以直接操作该值
    - 传值和传址再次出现，注意嗷呜！

  - 接口：**接口类型** 是由一组方法签名定义的集合。

    接口类型的变量可以保存任何实现了这些方法的值。

    - 接口和java里面类似，都是方法签名定义的集合。

    类似于：具体如何实现，可以定义好实现的函数, a = func来实现，具体往后看嗷

    - 接口在go内无需专门显式声明，隐式接口从接口的实现中解耦了定义，这样接口的实现可以出现在任何包中，无需提前准备。因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。

    ```go
    type I interface {
    	M()
    }
    
    type T struct {
    	S string
    }
    
    // 此方法表示类型 T 实现了接口 I，但我们无需显式声明此事。
    func (t T) M() {
    	fmt.Println(t.S)
    }
    
    func main() {
    	var i I = T{"hello"}
    	i.M()
    }
    ```

    - 接口值：接口也是值。它们可以像其它值一样传递。

      接口值可以用作函数的参数或返回值。

      在内部，接口值可以看做包含值和具体类型的元组：

      ```
      (value, type)
      ```

      接口值保存了一个具体底层类型的具体值。

      接口值调用方法时会执行其底层类型的同名方法。

      输出的类型不同

    `i = &T{"hello"}`这个就是给接口通过T赋予了初始值"hello"

    `i = var t *T`则默认i初始为nil

    ```go
    func (t *T) M() {
    	if t == nil {
    		fmt.Println("<nil>")
    		return
    	}
    	fmt.Println(t.S)
    }
    ```

    可以通过这种方法来对于这种可能的bug情况进行处理。

  - 如果不对接口进行正确处理的话，则value和type都对应时nil

  - 空接口：定义了0个方法就被称作空接口，可以储存未知类型的值嗷,.空接口被用来处理未知类型的值。例如，`fmt.Print` 可接受类型为 `interface{}` 的任意数量的参数。

  ```go
  var i interface{}
  	i = 42
  ```

  

  - 类型断言:

    - **类型断言** 提供了访问接口值底层具体值的方式。

      ```
      t := i.(T)
      ```

      该语句断言接口值 `i` 保存了具体类型 `T`，并将其底层类型为 `T` 的值赋予变量 `t`。

    - 为了 **判断** 一个接口值是否保存了一个特定的类型，类型断言可返回两个值：其底层值以及一个报告断言是否成功的布尔值。

      ```
      t, ok := i.(T)
      ```

    - 类型选择:

    switch语句的应用:

    ```go
    switch v := i.(type) {
    case T:
        // v 的类型为 T
    case S:
        // v 的类型为 S
    default:
        // 没有匹配，v 与 i 的类型相同
    }
    ```

    类型选择与一般的 switch 语句相似，不过类型选择中的 case 为类型（而非值）， 它们针对给定接口值所存储的值的类型进行比较。

    ```go
    	func do(i interface{}) {
    		switch v := i.(type) {
    		case int:
    			fmt.Printf("Twice %v is %v\n", v, v*2)
    		case string:
    			fmt.Printf("%q is %v bytes long\n", v, len(v))
    		default:
    			fmt.Printf("I don't know about type %T!\n", v)
    		}
    	}
    
    	func main() {
    		do(21)
    		do("hello")
    		do(true)
    	}
    ```

    这里do函数后面传入的数实际上被interface所接收，因为接收前不知道i是什么类型的，所以只能用Interface来进行接收。
  
  - Stringer:

    - [`fmt`](https://go-zh.org/pkg/fmt/) 包中定义的 [`Stringer`](https://go-zh.org/pkg/fmt/#Stringer) 是最普遍的接口之一。
  
      ```
      type Stringer interface {
          String() string
      }
      ```
  
      `Stringer` 是一个可以用字符串描述自己的类型。`fmt` 包（还有很多包）都通过此接口来打印值。可以通过调整此接口函数来实现打印方法的不同。
  
    - Sprintf函用于多个参数的格式化输出
  
- 错误：


  - 与 `fmt.Stringer` 类似，`error` 类型是一个内建接口：

    ```go
    type error interface {
        Error() string
    }
    ```

  - fmt在打印时也会满足error

  - 通常函数会返回一个 `error` 值，调用的它的代码应当判断这个错误是否等于 `nil` 来进行错误处理。

    ```
    i, err := strconv.Atoi("42")
    if err != nil {
        fmt.Printf("couldn't convert number: %v\n", err)
        return
    }
    fmt.Println("Converted integer:", i)
    ```

    `error` 为 nil 时表示成功；非 nil 的 `error` 表示失败。

- reader:


  - io.Reader接口表示从数据流的末尾进行读取
  - 这个接口中有一个Read方法：

  ```go
  func (T) Read(b []byte) (n int, err error)
  ```

  - `Read` 用数据填充给定的字节切片并返回填充的字节数和错误值。在遇到数据流的结尾时，它会返回一个 `io.EOF` 错误。

    示例代码创建了一个 [`strings.Reader`](https://go-zh.org/pkg/strings/#Reader) 并以每次 8 字节的速度读取它的输出。

    ```go
    package main
    
    import (
    	"fmt"
    	"io"
    	"strings"
    )
    
    func main() {
    	r := strings.NewReader("Hello, Reader!")
    
    	b := make([]byte, 8)
    	for {
    		n, err := r.Read(b)
    		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
    		fmt.Printf("b[:n] = %q\n", b[:n])
    		if err == io.EOF {
    			break
    		}
    	}
    }
    ```

    ```go
    n = 8 err = <nil> b = [72 101 108 108 111 44 32 82]
    b[:n] = "Hello, R"
    n = 6 err = <nil> b = [101 97 100 101 114 33 32 82]
    b[:n] = "eader!"
    n = 0 err = EOF b = [101 97 100 101 114 33 32 82]
    b[:n] = ""
    ```

    一个非常有意思的例子和结果。
    
- Image图像:

  - [image][https://go-zh.org/pkg/image/#Image]相关的文档在这里嗷

  - ```go
  package image
    
    ```
  
  type Image interface {
        ColorModel() color.Model
      Bounds() Rectangle
        At(x, y int) color.Color
}
    ```

    image定义了很多接口，别内置的一些类实现

  - 如过想要调用`pic.showImage`来展示出来图像

  ​```go
  type Image interface {
      // ColorModel returns the Image's color model.
    ColorModel() color.Model
      // Bounds returns the domain for which At can return non-zero color.
    // The bounds do not necessarily contain the point (0, 0).
      Bounds() Rectangle
    // At returns the color of the pixel at (x, y).
      // At(Bounds().Min.X, Bounds().Min.Y) returns the upper-left pixel of the grid.
  // At(Bounds().Max.X-1, Bounds().Max.Y-1) returns the lower-right one.
      At(x, y int) color.Color
  }
  Image is a finite rectangular grid of color.Color values taken from a color model.
    ```
  

只有实现了上面三个接口，`pic`函数对应的方法才能起作用。



- 并发：

  - `goroutine`:

  `fo f(x,y,z)`即为语法，启动一个新的`goroutine`并执行。

  - 信道:

  类似于linux里面一样，通过`<-`来接收或者返回值。

  ```go
  ch <- v
  v := <-ch
  ```

  - 信道使用规则:

    - 使用前必须创建: `ch := make(chan int)`
    - 默认情况下会保持信道的阻塞状态，使得go在没有锁的情况下，可以进行同步。

    例子:

    ```go
    func sum(s []int, c chan int) {
    	sum := 0
    	for _, v := range s {
    		sum += v
    	}
    	c <- sum // 将和送入 c
    }
    
    func main() {
    	s := []int{7, 2, 8, -9, 4, 0}
    
    	c := make(chan int)
    	go sum(s[:len(s)/2], c)
    	go sum(s[len(s)/2:], c)
    	x, y := <-c, <-c // 从 c 中接收
    
    	fmt.Println(x, y, x+y)
    }
    ```

    - 这里设置了两个`goroutine`各自含有一个c，当两个都结束后，x,y的值才会被赋好，最终才能得出结果。

    - 带缓冲的信道: `ch := make(chan int,100)`这里的100是缓冲数值，缓冲区为空，接收方会受阻，缓冲区为满，发送方会受阻。个人总结：有点像是队列的感觉，第二个参数规定了队列中同时能够存在的元素的个数。如果队列没有元素就输出或者队列是满的还向队列中输入，就会引发异常`fatal error: all goroutines are asleep - deadlock!`

    - range和close:

      - 可以通过close来关闭一个信道表示没有需要发送的值了。
      - 接收表达式可以为: `v, ok = <-ch`，如果没有值可以接收且信道被关闭，那么ok 会被设置为false
      - for i := range c 会不断从信道接受值，直到它被关闭。
      - panic是一个不可恢复的错误。
      - mention: 只有发送者才能关闭信道，接收者不能关闭信道，一个已经关闭的信道会引发panic, 信道一般情况下无需关闭，只有在必须告诉接收者不再有值传递的时候才有必要关闭嗷！

      ```go
      func fibonacci(n int, c chan int) {
      	x, y := 0, 1
      	for i := 0; i < n; i++ {
      		c <- x
      		x, y = y, x+y
      	}
      	close(c)
      }
      
      func main() {
      	c := make(chan int, 10)
      	go fibonacci(cap(c), c)
      	for i := range c {
      		fmt.Println(i)
      	}
      }
      ```

      cap函数是用来返回分配的空间大小嗷！

    - select语句：

      - select使得`goroutine`可以等待多个通信操作，select会阻塞到某个分支可以继续执行为止，这是就会选择该分支，多个分支都准备好随时会随机选择一个执行。

    - `sync.Mutex：`为了一次只想让一个go线程能访问一个共享的变量，防止变量的改变出问题，这个概念叫互斥，有两个方法: Lock和Unlock

    ```go
    package main
    
    import (
    	"fmt"
    	"sync"
    	"time"
    )
    
    // SafeCounter 的并发使用是安全的。
    type SafeCounter struct {
    	v   map[string]int
    	mux sync.Mutex
    }
    
    // Inc 增加给定 key 的计数器的值。
    func (c *SafeCounter) Inc(key string) {
    	c.mux.Lock()
    	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
    	c.v[key]++
    	c.mux.Unlock()
    }
    
    // Value 返回给定 key 的计数器的当前值。
    func (c *SafeCounter) Value(key string) int {
    	c.mux.Lock()
    	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
    	defer c.mux.Unlock()
    	return c.v[key]
    }
    
    func main() {
    	c := SafeCounter{v: make(map[string]int)}
    	for i := 0; i < 1000; i++ {
    		go c.Inc("somekey")
    	}
    
    	time.Sleep(time.Second)
    	fmt.Println(c.Value("somekey"))
    }
    
    ```

    











  

  

  

  

  

  

  

  

  

  

  

  

  

  

  















  

  


- 练习以及答案:
  
  - 题目一：为了练习函数与循环，我们来实现一个平方根函数：用牛顿法实现平方根函数。
  
     计算机通常使用循环来计算 x 的平方根。从某个猜测的值 z 开始，我们可以根据 z² 与 x 的近似度来调整 z，产生一个更好的猜测：
  
```
    z -= (z*z - x) / (2*z)
```

     重复调整的过程，猜测的结果会越来越精确，得到的答案也会尽可能接近实际的平方根。
      
     在提供的 func Sqrt 中实现它。无论输入是什么，对 z 的一个恰当的猜测为 1。 要开始，请重复计算 10 次并随之打印每次的 z 值。观察对于不同的值 x（1、2、3 …）， 你得到的答案是如何逼近结果的，猜测提升的速度有多快。
      
     提示：用类型转换或浮点数语法来声明并初始化一个浮点数值：
      
    ```
    z := 1.0
    z := float64(1)
    12
    ```
      
     然后，修改循环条件，使得当值停止改变（或改变非常小）的时候退出循环。观察迭代次数大于还是小于 10。 尝试改变 z 的初始猜测，如 x 或 x/2。你的函数结果与标准库中的 math.Sqrt 接近吗？

  ```go
  package main
  
  import (
  	"fmt"
  	"math"
  )
  
  func Sqrt(x float64) float64 {
  	z := float64(1)
  	for {
  		res := z - (z*z-x)/(2*z)
  		if math.Abs(res-z) < 1e-10 {
  			return res
  		}
  		z = res
  	}
  	return z
  }
  
  func main() {
  	fmt.Println(Sqrt(2))
  	fmt.Println(math.Sqrt(2))
  }
  ```

  - 题目二：实现 `Pic` 。它应当返回一个长度为 `dy` 的切片，其中每个元素是一个长度为 `dx` ，元素类型为 `uint8` 的切片。当你运行此程序时，它会将每个整数解释为灰度值（好吧，其实是蓝度值）并显示它所对应的图像。
  
     图像的选择由你来定。几个有趣的函数包括 `(x+y)/2` 、`x*y` 、 `x^y` 、 `x*log(y)` 和 `x%(y+1)` 。
  
     （提示：需要使用循环来分配 `[][]uint8` 中的每个 `[]uint8` ； 请使用 `uint8(intValue)` 在类型之间转换；你可能会用到 `math` 包中的函数。）

  ```go
  func Pic(dx, dy int) [][]uint8 {
  	a := make([][]uint8,dy)
  	for i := 0;i<dy;i++{
  		a[i] = make([]uint8,dx)
  		for j:=0;j<dx;j++{
  			a[i][j] = uint8(i%(j+1))
  		}
  	}
  	return a
  }
  
  func main() {
  	pic.Show(Pic)
  }
  ```

  解题思路类似于java中的多维数组，先创建一个维度。然后对于每个一维数组中的元素，让其指向另一个数组，实现二维数组的赋值和定义。这里注意一下，切片常常用make来进行初始化嗷！切片常常用[]type这种类型来表示。

  - 题目三: 字符的count功能, 实现 `WordCount`。它应当返回一个映射，其中包含字符串 `s` 中每个“单词”的个数。函数 `wc.Test` 会对此函数执行一系列测试用例，并输出成功还是失败。
  
    你会发现 [strings.Fields](https://go-zh.org/pkg/strings/#Fields) 很有帮助。
  
    ```
    package main
    
    import (
    	"golang.org/x/tour/wc"
    	"strings"
    )
    
    func WordCount(s string) map[string]int {
    	ret := make(map[string]int)
    	target := strings.Fields(s)
    	for _,v := range target{
    		ret[v]++
    	}
    	return ret
    }
    
    func main() {
    	wc.Test(WordCount)
    }
    ```
  
    summary:  
  
    1. 首先，strings.Fields(s)函数可以把一个字符串按照空格分隔并得到一个字符串数组
    2. map的初始化你要知道用make函数，切片也是make哦
    3. 关于数组的遍历要用 for 下标, 值 := range target这种形式
    4. 对于没有指定value的key,访问时默认为0或者是不存在
  
  - 题目四：斐波纳契闭包
  
    让我们用函数做些好玩的事情。
  
    实现一个 `fibonacci` 函数，它返回一个函数（闭包），该闭包返回一个[斐波纳契数列](https://zh.wikipedia.org/wiki/斐波那契数列) `(0, 1, 1, 2, 3, 5, ...)`。
  
    ```go
    // 返回一个“返回int的函数”
    func fibonacci() func() int {
    	res1 := 0
    	res2 := 1
    	return func() int {
    		tmp := res1
    		res1, res2 = res2, (res1 + res2)
    		return tmp
    	}
    }
    
    func main() {
    	f := fibonacci()
    	for i := 0; i < 10; i++ {
    		fmt.Println(f())
    	}
    }
    ```
  
    这里首先，那个闭包函数，感觉就是，每次运行完之后，由于函数闭包，里面还存了一些东西，比如res1 res2 在接下来的调用中仍会使用，func()中则是外界可以传入的参数，也可以用于改变闭包函数内的参量，tmp即为此次运行，函数对于外界返回的值。
    
  - 题目五：练习：Stringer
  
    通过让 `IPAddr` 类型实现 `fmt.Stringer` 来打印点号分隔的地址。
  
    例如，`IPAddr{1, 2, 3, 4}` 应当打印为 `"1.2.3.4"`。

  ```go
  package main
  
  import "fmt"
  
  type IPAddr [4]byte
  
  // TODO: 给 IPAddr 添加一个 "String() string" 方法
  
  func (i IPAddr) String() string{
  	return fmt.Sprintf("%d.%d.%d.%d",i[0],i[1],i[2],i[3])
  }
  
  func main() {
  	hosts := map[string]IPAddr{
  		"loopback":  {127, 0, 0, 1},
  		"googleDNS": {8, 8, 8, 8},
  	}
  	for name, ip := range hosts {
  		fmt.Printf("%v: %v\n", name, ip)
  	}
  }
  ```

  - 题目六：错误
  
    从[之前的练习](https://tour.go-zh.org/flowcontrol/8)中复制 `Sqrt` 函数，修改它使其返回 `error` 值。
  
    `Sqrt` 接受到一个负数时，应当返回一个非 nil 的错误值。复数同样也不被支持。
  
    创建一个新的类型
  
    ```
    type ErrNegativeSqrt float64
    ```
  
    并为其实现
  
    ```
    func (e ErrNegativeSqrt) Error() string
    ```
  
    方法使其拥有 `error` 值，通过 `ErrNegativeSqrt(-2).Error()` 调用该方法应返回 `"cannot Sqrt negative number: -2"`。
  
    **注意:** 在 `Error` 方法内调用 `fmt.Sprint(e)` 会让程序陷入死循环。可以通过先转换 `e` 来避免这个问题：`fmt.Sprint(float64(e))`。这是为什么呢？
  
    修改 `Sqrt` 函数，使其接受一个负数时，返回 `ErrNegativeSqrt` 值。

  ```go
  package main
  
  import (
  	"fmt"
  	"math"
  )
  
  type ErrNegativeSqrt float64
  
  func (e ErrNegativeSqrt) Error() string{
  	return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
  }
  
  func Sqrt(x float64) (float64, error) {
  	if x < 0 {
  		return 0, ErrNegativeSqrt(x)
  	}
  	return math.Sqrt(x), nil
  }
  
  func main() {
  	fmt.Println(Sqrt(2))
  	fmt.Println(Sqrt(-2))
  }
  ```

  - 练习七：实现一个 `Reader` 类型，它产生一个 ASCII 字符 `'A'` 的无限流。

  ```go
  package main
  
  import "golang.org/x/tour/reader"
  
  type MyReader struct{}
  
  // TODO: Add a Read([]byte) (int, error) method to MyReader.
  func (r MyReader) Read(b []byte) (int, error) {
      // 赋值并返回
      b[0] = 'A'
      return 1, nil
  }
  
  func main() {
  	reader.Validate(MyReader{})
  }
  ```

  - 练习八：练习：rot13Reader
  
    有种常见的模式是一个 [`io.Reader`](https://go-zh.org/pkg/io/#Reader) 包装另一个 `io.Reader`，然后通过某种方式修改其数据流。
  
    例如，[`gzip.NewReader`](https://go-zh.org/pkg/compress/gzip/#NewReader) 函数接受一个 `io.Reader`（已压缩的数据流）并返回一个同样实现了 `io.Reader` 的 `*gzip.Reader`（解压后的数据流）。
  
    编写一个实现了 `io.Reader` 并从另一个 `io.Reader` 中读取数据的 `rot13Reader`，通过应用 [rot13](http://en.wikipedia.org/wiki/ROT13) 代换密码对数据流进行修改。
  
    `rot13Reader` 类型已经提供。实现 `Read` 方法以满足 `io.Reader`。

  ```go
  package main
  
  import (
  	"io"
  	"os"
  	"strings"
  )
  
  type rot13Reader struct {
  	r io.Reader
  }
  
  func rot13(b byte) byte {
      switch {
      case 'A' <= b && b <= 'M':
          b = b + 13
      case 'M' < b && b <= 'Z':
          b = b - 13
      case 'a' <= b && b <= 'm':
          b = b + 13
      case 'm' < b && b <= 'z':
          b = b - 13
      }
      return b
  }
  
  func (mr rot13Reader) Read(b []byte) (int, error) {
      n, e := mr.r.Read(b)
      for i := 0; i < n; i++ {
          b[i] = rot13(b[i])
      }
      return n, e
  }
  
  func main() {
  	s := strings.NewReader("Lbh penpxrq gur pbqr!")
  	r := rot13Reader{s}
  	io.Copy(os.Stdout, &r)
  }
  ```

  - 练习九：还记得之前编写的[图片生成器](https://tour.go-zh.org/moretypes/18) 吗？我们再来编写另外一个，不过这次它将会返回一个 `image.Image` 的实现而非一个数据切片。
  
    定义你自己的 `Image` 类型，实现[必要的方法](https://go-zh.org/pkg/image/#Image)并调用 `pic.ShowImage`。
  
    `Bounds` 应当返回一个 `image.Rectangle` ，例如 `image.Rect(0, 0, w, h)`。
  
    `ColorModel` 应当返回 `color.RGBAModel`。
  
    `At` 应当返回一个颜色。上一个图片生成器的值 `v` 对应于此次的 `color.RGBA{v, v, 255, 255}`。

  ```go
  package main
  
  import (
  	"image"
  	"image/color"
  
  	"golang.org/x/tour/pic"
  )
  
  type Image struct {
  	W int
  	H int
  }
  
  func (i Image) Bounds() image.Rectangle {
  	return image.Rect(0, 0, i.W, i.H)
  }
  
  func (i Image) ColorModel() color.Model {
  	return color.RGBAModel
  }
  
  func (self Image) At(x, y int) color.Color {
  	return color.RGBA{uint8(x), uint8(y), 255, 255}
  }
  
  func main() {
  	m := Image{200, 200}
  	pic.ShowImage(m)
  }
  ```

- 练习十：

```
1）题目
​ 不同二叉树的叶节点上可以保存相同的值序列。例如，以下两个二叉树都保存了序列 1，1，2，3，5，8，13 。

2_tree

​ 在大多数语言中，检查两个二叉树是否保存了相同序列的函数都相当复杂。 我们将使用 Go 的并发和信道来编写一个简单的解法。

​ 本例使用了 tree 包，它定义了类型：

type Tree struct {
    Left  *Tree
    Value int
    Right *Tree
}
1
2
3
4
5
​ 1. 实现 Walk 函数。

​ 2. 测试 Walk 函数。

​ 函数 tree.New(k) 用于构造一个随机结构的已排序二叉查找树，它保存了值 k 、 2k 、 3k … 10k 。

​ 创建一个新的信道 ch 并且对其进行步进：

go Walk(tree.New(1), ch)
1
​ 然后从信道中读取并打印 10 个值。应当是数字 1, 2, 3, ..., 10 。

​ 3. 用 Walk 实现 Same 函数来检测 t1 和 t2 是否存储了相同的值。

​ 4. 测试 Same 函数。

​ Same(tree.New(1), tree.New(1)) 应当返回 true ，而 Same(tree.New(1), tree.New(2)) 应当返回 false 。

​ Tree 的文档可在这里找到。
```

答案：

```go
package main

import (
	"fmt"

	"golang.org/x/tour/tree"
)

// Walk 步进 tree t 将所有的值从 tree 发送到 channel ch。
func Walk(t *tree.Tree, ch chan int) {
	if t == nil {
		return
	}
	Walk(t.Left, ch)
	ch <- t.Value
	Walk(t.Right, ch)
}

// Same 检测树 t1 和 t2 是否含有相同的值。
func Same(t1, t2 *tree.Tree) bool {
	ch1 := make(chan int)
	ch2 := make(chan int)
	go Walk(t1, ch1)
	go Walk(t2, ch2)
	for i := 0; i < 10; i++ {
		x, y := <-ch1, <-ch2
		fmt.Println(x, y)
		if x != y {
			return false
		}
	}
	return true
}

func main() {
	// fmt.Println(Same(tree.New(1), tree.New(2)))
	fmt.Println(Same(tree.New(1), tree.New(1)))
}
```

注意到这里，tree是一个包，tree.Tree是里面的一个类型，*tree.Tree是指向这个类型的一个指针嗷，其余的用法和C里面没啥区别，这个chan感觉就像是C语言里面的数组，主要是用来临时保存数据用的嗷！

- 练习十一:

在这个练习中，我们将会使用 Go 的并发特性来并行化一个 Web 爬虫。

修改 `Crawl` 函数来并行地抓取 URL，并且保证不重复。

*提示*：你可以用一个 map 来缓存已经获取的 URL，但是要注意 map 本身并不是并发安全的！

```go
package main

import (
	"fmt"
)

type Fetcher interface {
	// Fetch 返回 URL 的 body 内容，并且将在这个页面上找到的 URL 放到一个 slice 中。
	Fetch(url string) (body string, urls []string, err error)
}

// Crawl 使用 fetcher 从某个 URL 开始递归的爬取页面，直到达到最大深度。
func Crawl(url string, depth int, fetcher Fetcher) {
	// TODO: 并行的抓取 URL。
	// TODO: 不重复抓取页面。
        // 下面并没有实现上面两种情况：
	if depth <= 0 {
		return
	}
	body, urls, err := fetcher.Fetch(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Printf("found: %s %q\n", url, body)
	for _, u := range urls {
		Crawl(u, depth-1, fetcher)
	}
	return
}

func main() {
	Crawl("https://golang.org/", 4, fetcher)
}

// fakeFetcher 是返回若干结果的 Fetcher。
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher 是填充后的 fakeFetcher。
var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}

```

答案:

```go
package main

import (
	"fmt"
	"sync"
)

type Fetcher interface {
	Fetch(url string) (body string, urls []string, err error)
}

func Crawl(url string, depth int, fetcher Fetcher, wg *sync.WaitGroup) {

	defer wg.Done()

	if depth <= 0 {
		return
	}

	if cache.has(url) {
		// fmt.Println("already exist.")
		return
	}

	body, urls, err := fetcher.Fetch(url)
	if err != nil {
		fmt.Println(err)
		return
	}

	fmt.Printf("found: %s %q\n", url, body)

	for _, u := range urls {
		wg.Add(1)
		go Crawl(u, depth-1, fetcher, wg)
	}

	return
}

type Cache struct {
	cache map[string]bool
	mutex sync.Mutex
}

func (cache *Cache) add(url string) {
	cache.mutex.Lock()
	cache.cache[url] = true
	cache.mutex.Unlock()
}

func (cache *Cache) has(url string) bool {
	cache.mutex.Lock()
	defer cache.mutex.Unlock()
	_, ok := cache.cache[url]
	if !ok {
		cache.cache[url] = true
	}
	return ok
}

var cache Cache = Cache{
	cache: make(map[string]bool),
}

func main() {
	var wg sync.WaitGroup

	wg.Add(1)
	go Crawl("https://golang.org/", 4, fetcher, &wg)

	wg.Wait()
}

type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
			"https://golang.org/pkg/os1/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}
```



​    

​    


