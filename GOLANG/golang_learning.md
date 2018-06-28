# golang语言特性
### 1. 垃圾回收
* a.内存自动回收，再也不需要开发人员管理内存
* b.开发人员专注业务实现，降低了心智负担
* c. 只需要new分配内存，不需要释放
### 2. 天然并发
* a.从语言层面支持并发，非常简单
* b. goroute，轻量级线程，创建成千上万个goroute成为可能
* c. 基于CSP（Communicating Sequential Process）模型实现 管道+goroute
```
func main() {
    go fmt.Println(“hello")
}
```
### 3. channel
* a.管道，类似unix/linux中的pipe
* b. 多个goroute之间通过channel进行通信
* c. 支持任何类型
* d. 管道中的数据不能直接打印，只能取出来再打印
```
func main() {
  pipe := make(chan int,3)
pipe <- 1
pipe <- 2
}
```
> make分配内存空间与new类似,chan 代表管道, 3代表容量(只能放三个int,如果超过三个会进行阻塞) pipe<-1 代表把
1放到管道里

### 4. 多返回值
* a.一个函数返回多个值
```
func calc(a int, b int)(int,int) {
    sum := a + b
    avg := (a+b)/2
    return sum, avg
}
```


# printf使用
```
package main
import "fmt" //一定不要忘了
 
type point struct{
    x,y int
}
 
func test(i , j int) int{ return i+j;}
 
func main(){
    p := point{1,2}
 
    fmt.Printf("%d\n",p)    // {1 2}
     
    fmt.Printf("%+v\n",p)   // {x:1 y:2}
     
    fmt.Printf("%#v\n",p)   // main.point{x:1, y:2}
 
    //输出类型
    fmt.Printf("%T\n",p)    // main.point
 
    //输出函数签名
    fmt.Printf("%T\n",test) //func(int ,int) int
 
    //输出bool值
    flag := true
    fmt.Printf("%t\n",flag) // true
     
    //尝试将一个字符串作为参数来输出bool值，不要尝试这样做
    fmt.Printf("%t\n","true") //%!t(string=true)  
     
    //输出十进制形式输出
    fmt.Printf("%d\n",123)      // 123
     
    //输出一个字符，参数对应ASCII码
    fmt.Printf("%c\n",98)       // b
     
    //输出一个整数的二进制形式的值
    fmt.Printf("%b\n",98)       // 1100010
 
    //输出一个字符的二进制形式的值
    fmt.Printf("%b\n",'b')      // 1100010
 
    //如果参数是数字，则以十六进制形式输出
    fmt.Printf("%x\n",456)      // 1c8
     
    //如果参数是字符串，则打印字符串的每一个字符的ASCII码
    fmt.Printf("%x\n","hex this") // 6865782074686973 
     
    //浮点数形式输出，注意小数位数为6位
    fmt.Printf("%f\n",78.53)    // 78.530000   
     
    //注意这里前后不对应，不会报错，但是不会自动转换
    fmt.Printf("%d\n",78.53)    // %!d(float64=78.53)
     
    //科学计数法的形式，注意参数要为小数，不为小数，可以乘1.0
    fmt.Printf("%e\n",123400000000.0)   //1.234000e+11 注意参数为小数
     
    //科学计数法的形式，注意参数要为小数，不为小数，可以乘1.0
    fmt.Printf("%E\n",123000000000.0)   //1.234000E+11
     
    //输出字符串
    fmt.Printf("%s\n","\"ddadjfaskdafjasfsaf")  //"ddadjfaskdafjasfsaf
     
    //保留字符串两端的双引号
    fmt.Printf("%q\n","\"dddddddd\"")   // "\"dddddddd\""
     
    //输出指针（地址）的值
    fmt.Printf("%p\n",&p) //0xc420012090
     
    //最小宽度为6，默认右对齐，不足6位时，空格补全，超过6位时，不会截断
    fmt.Printf("|%6d|%6d|\n",12,1234567) // |    12|1234567|
     
    //最小6个宽度（包含小数点)，2位小数，超过6位时，不会截断
    fmt.Printf("|%6.2f|%6.2f|\n",12,222.333) // |%!f(int=    12)|222.33|
     
    //使用 - 表示左对齐
    fmt.Printf("|%-6.2f|%-6.2f|\n",12.2,3.33) //|12.20 |3.33  |   
     
    //最小6个宽度，右对齐，不足6个时，空格补齐，超过6位时，不会截断
    fmt.Printf("|%6s|%6s|\n","foo","abcdefgg") //|   foo|abcdefgg|
     
    ////最小6个宽度，右对齐，不足6个时，空格补齐，超过6位时，不会截断
    fmt.Printf("|%-6s|%-6s|\n","foo","abcdeefgth") //|foo   |abcdeefgth|
 
    //不会输出内容，相反，会将内容以字符串的形式返回
    s:= fmt.Sprintf("a %s","string")
    fmt.Println(s)  //a string
    
    //从终端输入数值
    fmt.Scanf("%d",&n)
}
```

# 包的概念
*  和python一样，把相同功能的代码放到一个目录，称之为包
* 包可以被其他包引用
* main包是用来生成可执行文件，每个程序只有一个main包
* 包的主要用途是提高代码的可复用性


# go程序的基本结构
```
package main
import “fmt”
func main() {
    fmt.Println(“hello, world”)
}
```

1. 任何一个代码文件隶属于一个包
2. import 关键字，引用其他包：
    ```
    import(“fmt”)
    或者
    import (
          “fmt”
           “os”
    )
     ```
3. golang可执行程序，package main，并且有且只有一个main入口函数
4. 包中函数调用：
    * a. 同一个包中函数，直接调用
    * b. 不同包中函数，通过包名+点+函数名进行调用
5. 包访问控制规则：
    * 大写意味着这个函数/变量是可导出的
    * 小写意味着这个函数/变量是私有的，包外部不能访问

> 注意:   如果:=则是在执行的时候进行赋值，如果是一开始定义好的，则编译的时候已经赋值

### 包别名的应用

```
package main

import(
     a “add”
)

func main () {

fmt.Println(“result:”, a.Name)
fmt.Println(“result:”, a.age)
}

```


### 包的只初始化，不引用
```
package main

import(
     _ “add”
)

func main () {

fmt.Println(“add not refer:”)
}

```



### 常量
1. 常量使用const 修饰，代表永远是只读的，不能修改。
2. const 只能修饰boolean，number（int相关类型、浮点类型、complex）和string。
3. 语法：const identifier [type] = value，其中type可以省略。
```
举例:
const b string = "hello world" 
const b = "hello world" 
const Pi = 3.1414926
const a = 9/3
此为错误写法：
const c = getValue() 

比较优雅的写法：
const (
    a = 0
    b = 1
    c = 2
)
更加专业的写法：
const (
    a = iota
    b    //1
    c    //2
)

```
### 变量
语法：var identifier type
```
var a int
var b string
var c  bool 
var d int = 8
var e string = "hello world"


var (
    a int         //默认为0
    b string      //默认为””
    c bool        //默认为false
    d = 8
    e string =  "hello world"
    )

```


##### 时间
```
time.Sleep(1000 * time.Millisecond)  //一秒钟执行一次  time.Millisecond为一毫秒
time.Now().Unix() //获取当前时间
```

### 值类型和引用类型
1. 值类型：变量直接存储值，内存通常在栈中分配。值类型：基本数据类型int、float、bool、string以及数组和struct。
```
var i = 5    ====》   i----> 5
```
2. 引用类型：变量存储的是一个地址(指针)，这个地址存储最终的值。内存通常在堆上分配。通过GC回收。在32位系统中指针是4字节，64系统中指针是8字节。引用类型：指针、slice、map、chan等都是引用类型。

```
refer  -----> 地址 -----> 值
```

### 堆和栈

```

栈： 先进后出
使用栈完成函数的调用，栈空间的内存非常的高效，函数调用结束自动就弹出来，弹出来就释放了
栈内存的限制 C语言不超过1M  GO语言更小 基本上是几K
程序执行的时候栈一般都是公用的，调用一个函数，比如原本是调用a函数，再去调用b函数的话，还是使用同一个栈，只是a的现场会使用寄存器保护好，退栈之后，再把原来的现场恢复。




堆:  先进先出
物理内存基本上都是在堆里面, 堆内存需要申请，申请使用完之后会释放。
堆内存基本上大家都可以使用，多线程模型，每个线程都可以在堆里面分配，堆分配需要加锁，堆内存分配性能低于栈
通过GC回收

```

### 函数传值
传递到函数内部时，都会拷贝一份作为副本，无论是传值还是传地址
1. 当传的是地址时，副本指向的是同一个地址
2. 当是传值的时候，拷贝的副本指向不同的地址

### 变量的作用域
1. 在函数内部声明的变量叫做局部变量，生命周期仅限于函数内部。
2. 在函数外部声明的变量叫做全局变量，生命周期作用于整个包，如果是大写的，
则作用于整个程序。
3. 语句块里面定义的变量的生命周期只在该语句块里面
4. 在全局区只能声明变量，不能执行代码
```
全局区中:
var a int = 100   //其实是一条语句,声明变量a为100

不能使用 
a := 100
因为此语句相当于
var a int
a = 100  //此为赋值语句
在全局区域中 不能执行代码，所以不能执行a:=100
```

### 数据类型和操作符
1. bool类型，只能存true和false
```
var a bool
var a bool = true
var a = true

```

2. 相关操作符， ！、&&、||
```
var a bool = true
var b bool

请问!a 、!b、a && b、a || b的值分别是多少？

false true false true
```

3.数字类型，主要有int、int8、int16、int32、int64、uint8、uint16、uint32、uint64、float32、float64

4. 类型转换，type(variable），比如：var a int=8;  var b int32=int32(a)

```
package main
func main() {
    var a int
    var b int32
    a = 15
    b = a + a // compiler error
    b = b + 5 // ok: 5 is a constant
} 

```


5. 逻辑操作符： == 、!=、<、<=、>和 >=
6. 数学操作符：+、-、*、/等等
7. 字符类型：var a byte 
> byte 大小：占一个字节  8位
```
var a byte = 'c'   // 字符必须为单引号
```
> '0' 单引号0代表ASCII码48

> t:= []rune(str) 可用于统计中文
8. 字符串类型： var str string
9. 字符串表示两种方式： 1）双引号    2）``（反引号）
> 反引号 是一个原生的字符串，里面可以换行，不会转义




####  strings和strconv使用


```
strings
1. strings.HasPrefix(s string, prefix string) bool：判断字符串s是否以prefix开头 。
2. strings.HasSuffix(s string, suffix string) bool：判断字符串s是否以suffix结尾。
3. strings.Index(s string, str string) int：判断str在s中首次出现的位置，如果没有出现，则返回-1
4. strings.LastIndex(s string, str string) int：判断str在s中最后出现的位置，如果没有出现，则返回-1
5. strings.Replace(str string, old string, new string, n int)：字符串替换
6. strings.Count(str string, substr string)int：字符串计数
7. strings.Repeat(str string, count int)string：重复count次str
8. strings.ToLower(str string)string：转为小写
9. strings.ToUpper(str string)string：转为大写
10. strings.TrimSpace(str string)：去掉字符串首尾空白字符
11.strings.Trim(str string, cut string)：去掉字符串首尾cut字符
12.strings.TrimLeft(str string, cut string)：去掉字符串首cut字符
13.strings.TrimRight(str string, cut string)：去掉字符串首cut字符
14.strings.Field(str string)：返回str空格分隔的所有子串的slice
15.strings.Split(str string, split string)：返回str split分隔的所有子串的slice
16.strings.Join(s1 []string, sep string)：用sep把s1中的所有元素链接起来
17.strconv.Itoa(i int)：把一个整数i转成字符串
18.strconv.Atoi(str string)(int, error)：把一个字符串转成整数

```


#### 时间和日期类型
```
1. time包
2. time.Time类型，用来表示时间
3. 获取当前时间， now := time.Now()
4. time.Now().Day()，time.Now().Minute()，time.Now().Month()，time.Now().Year()
5. 格式化，fmt.Printf(“%02d/%02d%02d %02d:%02d:%02d”, now.Year()…)
6. time.Duration用来表示纳秒
7. 一些常量：
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)
8. 格式化：
now := time.Now()
fmt.Println(now.Format(“02/1/2006 15:04”))
fmt.Println(now.Format(“2006/1/02 15:04”))
fmt.Println(now.Format(“2006/1/02”))


```


#### 指针类型
1. 普通类型，变量存的就是值，也叫值类型
2. 获取变量的地址，用&，比如： var a int, 获取a的地址：&a
3. 指针类型，变量存的是一个地址，这个地址存的才是值
4. 获取指针类型所指向的值，使用：*，比如：var *p int, 使用 *p获取p指向的值
![](https://note.youdao.com/yws/public/resource/5978c86577f2382879467cc51562a46c/xmlnote/F7E2EAFF501440B5B06CC35EAE273FC2/5441)
```
获取一个变量的地址
    var a int = 10
	fmt.Println(&a)
	
	var p *int
	p = &a
	fmt.Println(*p)
```

### 流程控制
```
1. If / else分支判断

语法:
if condition1 {
}

if condition1 {
    
} else {

}

if condition1 {
    
} else if condition2 {

} else if condition3 {

} else {

}

写法:
    package main
    import "fmt"
    func main() {
        bool1 := true
        if bool1 {
            fmt.Printf("The value is true\n")
        } else {
            fmt.Printf("The value is false\n")
        }
    }
```

```
2. switch case语句  不需要break
语法1:
switch var {
    case var1:
    case var2:
    case var3:
    default:
}
写法1:
    var i = 0
    switch i {
        case 0:
        case 1:
            fmt.Println("1")
        case 2:
            fmt.Println("2")
        default:
            fmt.Println("def")
    }
写法2:
    var i = 0
    switch i {
    case 0, 1:
          fmt.Println("1")
    case 2:
    fmt.Println("2")
    default:
         fmt.Println("def")
    }
```    

```
switch 语法3:
    var i = 0
    switch {
        condition1:
              fmt.Println("i > 0 and i < 10")
        condition2:
              fmt.Println("i > 10 and i < 20")
        default:
              fmt.Println("def")
    }
 
写法3：   
    var i = 0
        switch {
            case i > 0 && i < 10:
                 fmt.Println("i > 0 and i < 10")
            case i > 10 && i < 20:
                 fmt.Println("i > 10 and i < 20")
            default:
                 fmt.Println("def")
        }
写法3.1：
        switch i := 0; {
            case i > 0 && i < 10:
                    fmt.Println("i > 0 and i < 10")
            case i > 10 && i < 20:
                    fmt.Println("i > 10 and i < 20")
            default:
                    fmt.Println("def")
        }
        
        
写法4： fallthrough  

Go里面switch默认相当于每个case最后带有break，匹配成功后不会自动向下执行其他case，

而是跳出整个switch, 但是可以使用fallthrough强制执行后面的case代码。

```

```
3. for 语句   条件不能写括号

语法1：
    for 初始化语句; 条件判断; 变量修改 {
    
    }
写法1：
    for i := 0 ; i < 100; i++ {
        fmt.Printf("i=%d\n", i)
    }
    
    
    
语法2：    
    for  条件 {
    
    }
写法2.1：
    for i > 0 {
          fmt.Println("i > 0")
    }
写法2.2：
    for true {
          fmt.Println("i > 0")
    }
写法2.3：
    for {
          fmt.Println("i > 0")
    }
    
```

```
3. for range 语句

str := "hello world,中国"
for i, v := range str {
     fmt.Printf("index[%d] val[%c] len[%d]\n", i, v, len(string(v)))
}
```

```
3. break continue语句
str := "hello world,中国"
for i, v := range str {
       if i > 2 {
             continue
       }
  if (i > 3) {
         break  }
     fmt.Printf("index[%d] val[%c] len[%d]\n", i, v, len(string(v)))
}

```

```
4. goto 和 label 语句
    package main
    import "fmt"
    func main() {
    LABEL1:
    	for i := 0; i <= 5; i++ {
    		for j := 0; j <= 5; j++ {
    			if j == 4 {
    				continue LABEL1
    			}
    			fmt.Printf("i is: %d, and j is: %d\n", i, j)
    		}
    	}
    }
    
    
    package main
    func main() {
    	i := 0
    HERE:
    	print(i)
    	i++
    	if i == 5 {
    		return
    	}
    	goto HERE
    }


```


### init函数
> 每个源文件都可以包含一个init函数，这个init函数自动被go运行框架调用。执行顺序,先调用全局变量,再调用init函数

```
package main

import(
     _ “add”
)

func init () {

fmt.Println("initialized")
}

```


### 函数声明： 

```
func   函数名字 (参数列表) (返回值列表）{}
举例:
1.
func add() {
} 
2.
func add(a int , b int) int {
} 
3.
func add(a int , b int) (int, int) {
} 
3.1
func add(a, b int) (int, int)  {
}

```

##### 函数特点
* 不支持重载，一个包不能有两个名字一样的函数
* 函数是一等公民，函数也是一种类型，一个函数可以赋值给变量
* 匿名函数
* 多返回值

```
func add(a, b int) int {
	return a + b
}

func main(){
    c := add
	fmt.Println(c)

	sum := c(10, 20)
	fmt.Println(sum)
}

```

```
package main

import "fmt"

type add_func func(int, int) int

func add(a, b int) int {
	return a + b
}

func operator(op add_func, a int, b int) int {
	return op(a, b)
}

func main() {
	c := add
	fmt.Println(c)
	sum := operator(c, 100, 200)
	fmt.Println(sum)
}

```


#####  函数参数传递方式：
* 值传递
* 引用传递

```
注意1：

无论是值传递，还是引用传递，传递给函数的都是变量的副本，

不过，值传递是值的拷贝。引用传递是地址的拷贝，一般来说，地址

拷贝更为高效。而值拷贝取决于拷贝的对象大小，对象越大，则性能越低。



注意2：
map、slice、chan、指针、interface默认以引用的方式传递
基本的数据类型，int、type、数组等都是值传递
```

##### 命名返回值的名字
```
func add(a, b int) (c int) {
        c = a + b
        return
    
}
        
func calc(a, b int) (sum int, avg int) {
        sum = a + b
        avg = (a +b)/2
        return
    
}
```

##### 可变参数：

```
0个或多个参数

func add(arg…int) int {
}



1个或多个参数
func add(a int, arg…int) int {
}



2个或多个参数
func add(a int, b int, arg…int) int {
}


注意：其中arg是一个slice，我们可以通过arg[index]依次访问所有参数,通过len(arg)来判断传递参数的个数,
arg可以自定义名字



例子:
func add_multi(a int , arg ... int) int {
	var sum int = a
	for i := 0; i < len(arg); i++{
		sum += arg[i]
	}
	return sum
}

func main (){
    sum := add_multi(10,1,2,3)
	fmt.Println(sum)
}

```


#####  defer：
```
1. 当函数返回时，执行defer语句。因此，可以用来做资源清理
2. 多个defer语句，按先进后出的方式执行
3. defer语句中的变量，在defer声明时就决定了。

例子:

func defer_test(){
	var i int = 0
	defer fmt.Println(i)
	defer fmt.Println("second")

	i = 10
	fmt.Println(i)
}

输出为
10
second
0
```

#####  defer用途：
```
1. 关闭文件句柄

    func read() {
        file := open(filename)
        defer file.Close()
        
        //文件操作
    }
    
2. 锁资源释放
    func read() {
        mc.Lock()
        defer mc.Unlock()
        //其他操作
    }
    
3. 数据库连接释放
    func read() {
        conn := openDatabase()
        defer conn.Close()
        //其他操作
    }


```

##### 匿名函数

```
1.
    var (
    	result = func(a1 int , b1 int ) int {
    		return a1+b1
    	}
    )
2.
    func Anonymous(a1 int ,b1 int) int  {
    	result := func (a1 int , b1 int) int {
    		return a1+b1
    	}(a1,b1)
    	return result
    }
```

##### 读取终端的输入
```
    reader := bufio.NewReader(os.Stdin)
    result,_,err := reader.ReadLine()
    if err != nil{
        fmt.Println("read from console err:",err)
        return
    }
```

### 内置函数
* close：主要用来关闭channel
* len：用来求长度，比如string、array、slice、map、channel
* new：用来分配内存，主要用来分配值类型，比如int、struct。返回的是指针
* make：用来分配内存，主要用来分配引用类型，比如chan、map、slice
* append：用来追加元素到数组、slice中
* panic和recover：用来做错误处理

##### new 例子:
```
func self_function(){
	var i int
	fmt.Println(i)
	j:= new(int)
	*j = 100
	fmt.Println(j) //打印地址
	fmt.Println(*j) //打印值
}

输出：
0
0xc420014088
100
```
```
    var a [5]int  //代表一个数组长度为5
    var b []int  //代表一个切片
```

##### 捕获错误
```
例子一：
func recover_test(){

	defer func() {
		if err := recover();err!=nil{
			fmt.Println(err)
		}
	}()
	a:= 0
	b:= 100/a
	fmt.Println(b)

}
```
```
例子二：
func initConfig() (err error)  {
	return errors.New("init config failed")
}
func panic_test()  {
	err :=initConfig()
	if err != nil {
		panic(err)
	}
	return
}
```

#### new和make的区别

![](https://note.youdao.com/yws/public/resource/5978c86577f2382879467cc51562a46c/xmlnote/F5AD099831CD4F82AA9E3F18D2BE31AC/5674)


```
func new_make_test(){

	s1:= new ([]int)
	fmt.Println(s1)

	s2:= make([]int,10)
	fmt.Println(s2)
}

输出：
&[]
[0 0 0 0 0 0 0 0 0 0]
```

### 递归函数
 一个函数调用自己，就叫做递归。
 
 递归的设计原则
* 一个大的问题能够分解成相似的小问题
* 定义好出口条件

 ```
 例子：求n的阶乘
 package main

import (
	"fmt"
)

func calc(n int) int {
	if n == 1 {
		return 1
	}

	return calc(n-1) * n
}

func main() {
	n := calc(5)
	fmt.Println(n)
}

 ```
 ```
 斐波那契数
 package main

import "fmt"

func fab(n int) int {
	if n <= 1 {
		return 1
	}

	return fab(n-1) + fab(n-2)
}

func main() {
	for i := 0; i < 10; i++ {
		n := fab(i)
		fmt.Println(n)
	}
}

 ```
 
 ### 闭包
 闭包：一个函数和与其相关的引用环境组合而成的实体
 
 ```
 package main

import “fmt”

func main() {
     var fun = Adder()
	fmt.Println(fun(1))     //1
	fmt.Println(fun(100))   //101
	fmt.Println(fun(1000))  //1101
} 

func Adder() func (int) int {
	var x int
	return func(i int) int {
		x +=i
		return x
	}
}

 ```
 
 ```
    package main

    import (
    	"fmt"
    	"strings"
    )
    func makeSuffixFunc(suffix string) func(string) string {
    	return func(name string) string {
    		if !strings.HasSuffix(name, suffix) {
    			return name + suffix
    		}
    		return name
    	}
    }
    func main() {
    	func1 := makeSuffixFunc(".bmp")
    	func2 := makeSuffixFunc(".jpg")
    	fmt.Println(func1("test"))
    	fmt.Println(func2("test"))
    }

 ```
 
 ### 数组
 * 1. 数组：是同一种数据类型的固定长度的序列。
 * 2. 数组定义：var a [len]int，比如：var a[5]int 一旦定义，长度不能变
 * 3. 长度是数组类型的一部分，因此，var a[5] int和var a[10]int是不同的类型
 * 4. 数组可以通过下标进行访问，下标是从0开始，最后一个元素下标是：len-1
 ```
 for i := 0; i < len(a); i++ {
 }
 
 for key, value := range a {
 }
 ```
 * 5. 访问越界，如果下标在数组合法范围之外，则触发访问越界，会panic
 * 6. 数组是值类型，因此改变副本的值，不会改变本身的值
 
##### 数组初始化
* a. var age0 [5]int = [5]int{1,2,3}
* b. var age1 = [5]int{1,2,3,4,5}
* c. var age2 = […]int{1,2,3,4,5,6}
* d. var str = [5]string{3:”hello world”, 4:”tom”}


##### 多维数组
* a. var age [5][3]int
* b. var f [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

```
func arr(){
	var arr[2] int
	arr2 := arr
	arr2[1] = 100
	fmt.Println(arr)
	fmt.Println(arr2)
}
输出：
[0 0]
[0 100]
```

```
使用指针,改变数组的值
package main

import (
	"fmt"
)

func modify(arr *[5]int) {
	(*arr)[0] = 100
	return
}

func main() {
	var a [5]int

	modify(&a)
	for i := 0; i < len(a); i++ {
		fmt.Println(a[i])
	}
}


```
```
多维数组遍历
package main

import (
	"fmt"
)

func main() {

	var f [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

	for k1, v1 := range f {
		for k2, v2 := range v1 {
			fmt.Printf("(%d,%d)=%d ", k1, k2, v2)
		}
		fmt.Println()
	}
}

```

### 切片
* 1. 切片：切片是数组的一个引用，因此切片是引用类型
* 2. 切片的长度可以改变，因此，切片是一个可变的数组
* 3. 切片遍历方式和数组一样，可以用len()求长度
* 4. cap可以求出slice最大的容量，0 <= len(slice) <= cap(array)，其中array是slice引用的数组，容量是整个的大小,而长度是实际的长度
* 5. 切片的定义：var 变量名 []类型，比如 var str []string  var arr []int
##### 切片初始化
1. 切片初始化：var slice []int = arr[start:end] 包含start到end之间的元素，但不包含end 
2.  Var slice []int = arr[0:end]可以简写为 var slice []int=arr[:end]
3. Var slice []int = arr[start:len(arr)] 可以简写为 var slice[]int = arr[start:]
4. Var slice []int = arr[0, len(arr)] 可以简写为 var slice[]int = arr[:]
5. 如果要切片最后一个元素去掉，可以这么写： 
```
Slice = slice[:len(slice)-1]

```
##### 切片的内存结构
![](https://note.youdao.com/yws/public/resource/5978c86577f2382879467cc51562a46c/xmlnote/6E370D93220E4D139CA83B9E3C8B0542/5769)

##### 通过make来创建切片
不需要自定义数组，但是底层操作的还是一个数组，只是我们无法看见

```
var slice []type = make([]type, len)
slice  := make([]type, len)
slice  := make([]type, len, cap)
```
![](https://note.youdao.com/yws/public/resource/5978c86577f2382879467cc51562a46c/xmlnote/75EE2148BADA4863BBC252EFFF7C6B09/5774)

##### 用append内置函数操作切片

```

slice = append(slice, 10)
var a = []int{1,2,3}
var b = []int{4,5,6}
a = append(a, b…)

```

> 注意: 如果slice通过append等函数进行扩容，此时会开辟另一块内存空间，操作另一个副本的数组，与原始指针指向的数组地址不一致


#####  For range 遍历切片
```
for index, val := range slice {
    
}

```

#####   切片resize
```
var a = []int {1,3,4,5}
b := a[1:2]
b = b[0:3]

```

#####  切片拷贝
```
s1 := []int{1,2,3,4,5}

s2 := make([]int, 10)

copy(s2, s1)   //s2 ===> [1 2 3 4 5 0 0 0 0 0]

s3 := []int{1,2,3}

s3 = append(s3, s2…)

s3 = append(s3, 4,5,6)   // s3 ===>  [1 2 3 1 2 3 4 5 0 0 0 0 0 4 5 6]

```


#####  string与slice

> string底层就是一个byte的数组，因此，也可以进行切片操作

```
str := "hello world"
s1 := str[0:5]
fmt.Println(s1)

s2 := str[5:]
fmt.Println(s2)
```

 ![](https://note.youdao.com/yws/public/resource/5978c86577f2382879467cc51562a46c/xmlnote/75EE2148BADA4863BBC252EFFF7C6B09/5774)
 
 
 #####  如何改变string中的字符值？
> string本身是不可变的，因此要改变string中字符，需要如下操作：

```
str := "hello world"
s := []byte(str)
s[0] = 'a'
str = string(s)

```
> 如果要改中文

```
str := "你好 hello world "
s := []rune(str)
s[0] = 'a'
str = string(s)
fmt.Println(str)
```

##### 排序和查找操作
排序操作主要都在 sort包中，导入就可以使用了

import("sort")

sort.Ints对整数进行排序， sort.Strings对字符串进行排序, sort.Float64s对浮点数进行排序.

sort.SearchInts(a []int, b int) 从数组a中查找b，前提是a必须有序  返回排序后的位置

sort.SearchFloats(a []float64, b float64) 从数组a中查找b，前提是a必须有序  返回排序后的位置

sort.SearchStrings(a []string, b string) 从数组a中查找b，前提是a必须有序  返回排序后的位置

```
sort.Ints例子：
func testSortInt(){
	var a = [...]int{1,8,32,4,16,64,128}
	sort.Ints(a[:])
	fmt.Println(a)
}


sort.Strings例子:
func testSortString()  {
	var a = [...]string{"abc","efg","b","A","eee"}
	sort.Strings(a[:])
	fmt.Println(a)
}

sort.Float64s例子：
func testSortFloat()  {
	var a = [...]float64{2,3,0.8,28.2,1321,0.6,0.1}
	sort.Float64s(a[:])
	fmt.Println(a)
}

sort.SearchInts例子：
func testSearchInt(){
	var a = [...]int{1,8,32,4,16,64,128}
	sort.Ints(a[:])
	index := sort.SearchInts(a[:],4)
	fmt.Println(index)
}
```

### map数据结构
##### 1. map简介
key-value的数据结构，又叫字典或关联数组

声明：
```
var map1 map[keytype]valuetype
var a map[string]string
var a map[string]int
var a map[int]string
var a map[string]map[string]string

```
> 注意: 声明是不会分配内存的，初始化需要make

```
func  testMap(){
	//var a map[string]string = map[string]string
	a := make(map[string]string)  
	a["abc"] ="efg"
	fmt.Println(a)
}
```
##### 2. map相关操作

```
func testMap2()  {
	a := make(map[string]map[string]string)
	a["key1"] = make(map[string]string)
	a["key1"]["key2"] = "value2"  //插入和更新
	a["key1"]["key3"] = "value3"  //插入和更新
	fmt.Println(len(a))			  //长度 1
	val,ok:=a["key1"]["key3"]	  //查找
	fmt.Println(val,ok)     	  // value3 true
	for _,v :=range a{
		for _,v1 :=range v{
			fmt.Println(v1)      //value2
								 //value3
		}
	}
	delete(a,"key1")
	fmt.Println(len(a))			 //长度0
}
```

##### 3.map是引用类型
```
func modify(a map[string]int) {
    a[“one”] = 134
}

```

##### 4. slice of map

```
func testMapSlice(){
	var a []map[int]int
	a = make([]map[int]int,5)   	//因为a是Slice不初始化的话也会panic
	if a[0] == nil{
		a[0] = make(map[int]int)	// map也需要初始化
	}
	a[0][0] = 10 
	fmt.Println(a)					// [map[0:10] map[] map[] map[] map[]]
}
```

```
Items := make([]map[int][int], 5)
For I := 0; I < 5; i++ {
        items[i] = make(map[int][int])
}

```


##### 5. map排序
a. 先获取所有key，把key进行排序

b. 按照排序好的key，进行遍历

```
func testMap3(){
	var a map[int]int
	a = make(map[int]int,5)
	a[1] = 10
	a[2] = 20
	a[3] = 30
	a[4] = 40
	a[5] = 50
	var keys []int
	for k,v :=range a{
		keys = append(keys,k)
		fmt.Println(k,v)
	}
	sort.Ints(keys)
	fmt.Println(keys)
	for _,v :=range keys{
		fmt.Println(v,a[v])
	}
}
```

##### 6. Map反转
a. 初始化另外一个map，把key、value互换即可

### 包
##### 1. golang中的包

    a. golang目前有150个标准的包，覆盖了几乎所有的基础库

    b. golang.org有所有包的文档

##### 2. 线程同步
    a. import(“sync”)

    b. 互斥锁, var mu sync.Mutex
    
    c. 读写锁, var mu sync.RWMutex
    
    如果业务读多写少,使用读写锁。读少写多用互斥锁
```
互斥锁 例子：
func test_lock(){
	var a map[int]int
	a = make(map[int]int,5)
	a[1] = 10
	a[2] = 20
	a[3] = 30
	a[4] = 40
	a[5] = 50
	var lock sync.Mutex
	for i:=0;i<2;i++{
		go func(b map[int]int) {
			lock.Lock()
			b[1] = rand.Intn(100)
			lock.Unlock()
		}(a)
	}
	lock.Lock()
	fmt.Println(a)
	lock.Unlock()
	time.Sleep(time.Second)
}
```

##### 3.原子操作与读写锁
> 原子操作为串行操作，不需要锁

```

func test_RWlock(){

	var a map[int]int
	a = make(map[int]int, 5)
	var count int32
	a[8] = 10
	a[3] = 10
	a[2] = 10
	a[1] = 10
	a[18] = 10
	var rwLock sync.RWMutex		//读写锁
	for i := 0; i < 2; i++ {
		go func(b map[int]int) {
			rwLock.Lock()
			b[8] = rand.Intn(100)
			time.Sleep(10 * time.Millisecond)
			rwLock.Unlock()
		}(a)
	}

	for i := 0; i < 100; i++ {
		go func(b map[int]int) {
			for {
				rwLock.RLock()
				time.Sleep(time.Millisecond)
				rwLock.RUnlock()
				atomic.AddInt32(&count, 1)   //原子性写操作
			}
		}(a)
	}
	time.Sleep(time.Second * 3)
	fmt.Println(atomic.LoadInt32(&count))		  //原子性读操作
}

```

####




























 
 
















































































































