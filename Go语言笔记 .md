# Go笔记

声明：本笔记仅为个人学习使用，参考自教程：https://go.dev/tour/list等。

## 包

###包的导入

```go
package main //声明所属的包

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("我最喜欢的数字是 ", rand.Intn(10))
}

```

​	第一行代码`package main`定义了包名。必须在源文件中非注释的第一行指明这个文件属于哪个包，如：`package main`。`package main`表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 `main` 的包。

​	下一行`import “fmt”`告诉 Go 编译器这个程序需要使用 `fmt `包（的函数，或其他元素），`fmt `包实现了格式化 IO（输入/输出）的函数。

​	下一行`func main()`是程序开始执行的函数。`main `函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 `init()`函数则会先执行该函数）。


* 除了使用圆括号`()`分组导入外，也可以编写多个导入语句：

```go
import "fmt"
import "math"
```

### 包的导出

在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的。例如，`Pi` 就是个已导出名，它导出自 `math`包。

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.Pi)
}

```

## 变量

### 定义

参考自：https://blog.go-zh.org/gos-declaration-syntax；[CSDN博客](https://blog.csdn.net/weixin_44211968/article/details/121232016?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171965622116800227418592%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171965622116800227418592&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-121232016-null-null.142^v100^pc_search_result_base9&utm_term=go%20变量声明&spm=1018.2226.3001.4187)

* Go的变量声明不需要冒号，并且类型在变量名后面。
* 数值类型（包括complex64/128）默认为 `0`。布尔类型默认为 `false`。字符串默认为 `""`（空字符串）。其他类型默认为 `nil`

```go
var name(变量名) type(数据类型)
var name1, name2, name3 type	//多个同类型变量
var (
    name1 type1
    name2 type2
)	//批量声明
```

### 基本类型

```text
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
     // 表示一个 Unicode 码位

float32 float64

complex64 complex128
```

`int`、`uint` 和 `uintptr` 类型在 32-位系统上通常为 32-位宽，在 64-位系统上则为 64-位宽。当你需要一个整数值时应使用 `int`类型， 除非你有特殊的理由使用固定大小或无符号的整数类型

### 初始化

* 先声明，再初始化：

```go
var name type //声明
name = value  //初始化
```

* 多个变量一同初始化：

```go
var a, b int
var c string
a, b, c = 5, 7, "abc"
```

* 交换同类型变量的值：

```go
a, b = b, a
```

* 声明同时初始化：

```go
var name1 type = value
var name2 = value	//省略类型，Go编译器自动判断
```

* **函数体内声明变量**：

​	`:=`是赋值操作符，用它来声明，表明`name` 是一个变量；然后 Go 根据给 `name `初始化的值 `value `来确定 `name` 变量的类型。

​	但是它**只能被用在函数体内**，而不可以用于全局变量的声明与赋值。

```go
name := value 
a, b, c := 5, 7, "abc"
```

```go
package main

import (
    "fmt"
)
// 全局变量m
var m = 100

func main() {
    n := 10
    m := 200 // 此处声明局部变量m
    fmt.Println(m, n)
}
```

* 匿名变量：

​	匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明，用一个下划线 _ 表示：

```go
func foo() (int, string) {
    return 10, "LMQ"
}
func main() {
    x, _ := foo()
    _, y := foo()
    fmt.Println("x=", x)
    fmt.Println("y=", y)
}

```

### 类型转换

* 与 C 不同的是，Go 在不同类型的项之间赋值时需要显式转换。

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

### 注意事项

1. 声明之后，不可以再次对变量声明（==声明只需要一次==）；
2. 如果你在定义变量 a 之前使用它，则会得到编译错误 undefined: a（==应该先声明，再使用==）；
3. 如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误 a declared and not used，即==局部变量声明之后必须使用==；
4. 但是==全局变量是允许 “ 声明但不使用 ”== 的。

## 常量

常量的声明与变量类似，只不过使用 `const` 关键字。

常量可以是字符、字符串、布尔值或数值。

常量不能用 `:=` 语法声明。

```go
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}

```



## 函数

[更详细的内容参考](https://blog.csdn.net/weixin_44211968/article/details/121295530?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171972582816800182750880%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171972582816800182750880&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-121295530-null-null.142^v100^pc_search_result_base9&utm_term=go%20函数&spm=1018.2226.3001.4187)

### 定义

```text
func name( [parameter list] ) [return_types] {
   函数体
}
```

* 函数可接受零个或多个参数。
* 注意类型在变量名的**后面**。

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

* 同类型参数简写

```go
func add(x, y int) int {
	return x + y
}
```

* 多返回值

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

* **裸返回值**

```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

```

### 值传递和引用传递

* ==值传递==是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中==如果对参数进行修改，将不会影响到实际参数==。
* ==引用传递==是指在调用函数时将实际参数的地址传递到函数中，那么在函数中==对参数所进行的修改，将影响到实际参数==。

* 默认情况下，Go使用的是值传递：

```go
package main

import "fmt"

/* 定义相互交换值的函数 */
func swap(x, y int) int {
	var temp int

	temp = x /* 保存 x 的值 */
	x = y    /* 将 y 值赋给 x */
	y = temp /* 将 temp 值赋给 y*/

	return temp
}

func main() {
	/* 定义局部变量 */
	var a int = 100
	var b int = 200

	fmt.Printf("交换前 a 的值为 : %d\n", a)
	fmt.Printf("交换前 b 的值为 : %d\n", b)

	/* 通过调用函数来交换值 */
	swap(a, b)

	fmt.Printf("交换后 a 的值 : %d\n", a)
	fmt.Printf("交换后 b 的值 : %d\n", b)
}

```

这段代码输出：

```texe
交换前 a 的值为 : 100
交换前 b 的值为 : 200
交换后 a 的值 : 100
交换后 b 的值 : 200
```

* 引用传递：引用传递将**指针**参数传递到函数内

```go
package main

import "fmt"

func swap(x *int, y *int) {
	var temp int
	temp = *x /* 保存 x 地址上的值 */
	*x = *y   /* 将 y 值赋给 x */
	*y = temp /* 将 temp 值赋给 y */
}

func main() {
	/* 定义局部变量 */
	var a int = 100
	var b int = 200

	fmt.Printf("交换前，a 的值 : %d\n", a)
	fmt.Printf("交换前，b 的值 : %d\n", b)

	swap(&a, &b)

	fmt.Printf("交换后，a 的值 : %d\n", a)
	fmt.Printf("交换后，b 的值 : %d\n", b)
}

```

这段代码输出：

```texe
交换前 a 的值为 : 100
交换前 b 的值为 : 200
交换后 a 的值 : 200
交换后 b 的值 : 100
```

### 匿名函数

* 匿名函数是指不需要定义函数名的一种函数实现方式。

* 在Go里面，函数可以像普通变量一样被传递或使用，Go语言支持随时在代码里定义匿名函数。

* 匿名函数由一个不带函数名的函数声明和函数体组成。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    getSqrt := func(a float64) float64 {
        return math.Sqrt(a)
    }
    fmt.Println(getSqrt(4))
}

```

输出为`2`。

### 函数用法

#### 函数作为实参

* 函数是第一类对象，可作为参数传递。

```go
package main

import "fmt"

// 声明一个函数类型
type cb func(int) int

func main() {
	testCallBack(1, callBack)
	testCallBack(2, func(x int) int {
		fmt.Printf("我是回调，x：%d\n", x)
		return x
	})
}

func testCallBack(x int, f cb) {
	f(x)
}

func callBack(x int) int {
	fmt.Printf("我是回调，x：%d\n", x)
	return x
}

```

这段代码输出：
```text
我是回调，x：1
我是回调，x：2
```

建议将复杂签名定义为函数类型，以便阅读，如：

```go
type cb func(int) int
ype FormatFunc func(s string, x, y int) string
```

```go
package main

import "fmt"

func test(fn func() int) int {
	return fn()
}

// 定义函数类型。
type FormatFunc func(s string, x, y int) string

func format(fn FormatFunc, s string, x, y int) string {
	return fn(s, x, y)
}

func main() {
	s1 := test(func() int { return 100 }) // 直接将匿名函数当参数。

	s2 := format(func(s string, x, y int) string {
		return fmt.Sprintf(s, x, y)
	}, "%d, %d", 10, 20)

	println(s1, s2)
}

```

这段代码输出：

```text
100 10, 20
```

#### 闭包

[更详细的内容参考](https://blog.csdn.net/weixin_44211968/article/details/121295530?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171972582816800182750880%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171972582816800182750880&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-121295530-null-null.142^v100^pc_search_result_base9&utm_term=go%20函数&spm=1018.2226.3001.4187)

## 流程控制语句

### for

```go
for 变量初值; 循环条件; 变量自增减 {
  语句1
  语句2
}
```

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

* 初始化语句和后置语句是可选的。

```go
for ; 循环条件; {
  语句1
  语句2
}
```

```go
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```

* 省略循环条件，该循环就不会结束

```go
package main

func main() {
	for {
	}
}

```

### "while"

for 循环去掉`;`就是Go中的`while`

```go
for 循环条件 {
  语句1
  语句2
}
```

```go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

### if

```go
if 条件 {
  语句1
  语句2
}
```

```go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}

```

* 和 `for` 一样，`if` 语句可以在条件表达式前执行一个简短语句。该语句声明的变量作用域仅在 `if` 之内。

```go
if 变量赋值; 循环条件 {
  语句1
  语句2
}
```

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

### if else

```go
if 变量赋值; 循环条件 {
  语句1
  语句2
} else {
  语句1
  语句2
}
```

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

### switch

​	Go 的 `switch` 语句类似于 C、C++、Java、JavaScript 和 PHP 中的，不过 Go 只会运行第一个 `case` 值等于条件表达式的子句，而非之后所有的 `case`。 在效果上，==Go 的做法相当于这些语言中为每个 `case` 后面自动添加了所需的 `break` 语句==。在 Go 中，除非以 `fallthrough` 语句结束，否则分支会自动终止。 Go 的另一点重要的不同在于 `switch` 的 `case` 无需为常量，且取值不限于整数。

```go
swith 变量定义; 变量 {
  case 值1:
  	语句1
  case 值2:
  	语句2
  default:
  	语句3
}
```

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go 运行的系统环境：")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("macOS.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

### 无条件switch

```go
swith  {
  case 条件1:
  	语句1
  case 条件2:
  	语句2
  default:
  	语句3
}
```

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("早上好！")
	case t.Hour() < 17:
		fmt.Println("下午好！")
	default:
		fmt.Println("晚上好！")
	}
}
```

### defer 推迟（栈）

​	defer 语句会将函数推迟到外层函数返回之后执行。推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

这段代码输出：

```text
hello
world
```

* 推迟调用的函数调用会被压入一个栈中。 当外层函数返回时，被推迟的调用会按照后进先出的顺序调用

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

这段代码输出：

```text
counting
done
9
8
7
6
5
4
3
2
1
0
```

## 指针

* 指针保存了值的内存地址。

* 与 C 不同，Go 没有指针运算。

```go 
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // 指向 i
	fmt.Println(*p) // 通过指针读取 i 的值
	*p = 21         // 通过指针设置 i 的值
	fmt.Println(i)  // 查看 i 的值

	p = &j         // 指向 j
	*p = *p / 37   // 通过指针对 j 进行除法运算
	fmt.Println(j) // 查看 j 的值
}
```

## 结构体

[更详细的内容参考](https://blog.csdn.net/Suk_god/article/details/133877517?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171972396616800211526620%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171972396616800211526620&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-133877517-null-null.142^v100^pc_search_result_base9&utm_term=go%20结构体&spm=1018.2226.3001.4187)

####type 用法

在Go语言中有一些基本的数据类型，可以使用`type`关键字来定义自定义类型
自定义类型是定义了一个全新的类型，我们可以基于内置的基本类型定义，也可以通过`struct`定义

```go
//类型定义
type NewInt int

//类型别名
type MyInt = int

func main() {
	var a NewInt
	var b MyInt
	
	fmt.Printf("type of a: %T\n", a) 
	fmt.Printf("type of b: %T\n", b)
}

```

这段代码输出：

```text
type of a: main.NewInt
type of b: int
```

####结构体定义

* 结构体就是一组字段。

```text
type 结构体名称 struct {
	变量名称1 变量类型
	变量名称2 变量类型
}
```

```go
type Vertex struct {
	X int
	Y int
}
```

* 结构体匿名字段：
  结构体允许其成员字段在申明的时候没有字段名只有字段类型，这种没有名字的字段就称为匿名字段。

```go
type Person struct {
	string
	int
}
```

#### 嵌套结构体

* 一个结构体中可以嵌套包含另一个结构体或结构体指针。

```go
type Address struct {
	Province string
	City     string
}

type Person struct {
	Name    string
	Age     int8
	Address Address
}

func main() {
	person := Person{
		Name: "LMQ",
		Age:  18,
		Address: Address{
			Province: "jiangxi",
			City:     "jiujiang",
		},
	}

	fmt.Printf("%#v\n", person)
}
                    
```

* 当然，也支持**嵌套匿名字段**，将上述的Person结构体中的Address字段设置为匿名字段：

```go
type Person struct {
	Name    string
	Age     int8
	Address //匿名字段
}
```

####字段的访问

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}

```

* 通过指针访问

​	如果我们有一个指向结构体的指针 `p` 那么可以通过 `(*p).X` 来访问其字段 `X`。 语言也允许我们使用隐式解引用，直接写 `p.X` 就可以。

```go 
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}
```

####结构体字面量

使用 `Name:` 语法可以仅列出部分字段（字段名的顺序无关）。

特殊的前缀 `&` 返回一个指向结构体的指针。

```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予零值
	v3 = Vertex{}      // X:0 Y:0
	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```

这段代码输出：

```text
{1 2} &{1 2} {1 0} {0 0}
```

## 数组

==数组下标是从0开始计数的==

### 数组定义

* 类型 `[n]T` 表示一个数组，它拥有 `n` 个类型为 `T` 的值。

```go
var a [10]int //将变量 `a` 声明为拥有 10 个整数的数组。
```

数组的长度是其类型的一部分，因此数组不能改变大小。 这看起来是个限制，不过没关系，Go 拥有更加方便的使用数组的方式。

```go
package main

import "fmt"

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

这段代码输出：

```text
Hello World
[Hello World]
[2 3 5 7 11 13]
```

* 多维数组

```go
board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}
```

### 切片

​	每个数组的大小都是固定的。而切片则为数组元素提供了动态大小的、灵活的视角。 在实践中，切片比数组更常用。

* 类型 `[]T` 表示一个元素类型为 `T` 的切片。

```
a[low : high]
```

它会选出一个半闭半开区间，==包括第一个元素，但排除最后一个元素==。

以下表达式创建了一个切片，它包含 `a` 中下标从 1 到 3 的元素：

```
a[1:4]
```

```go
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}

```

这段代码输出：

```text
[3 5 7]
```

* 更改切片的元素会修改其底层数组中对应的元素。

* 和它共享底层数组的切片都会观测到这些修改。

* 在进行切片时，你可以利用它的默认行为来忽略上下界。

  切片下界的默认值为 0，上界则是该切片的长度。

  对于数组

  ```
  var a [10]int
  ```

  来说，以下切片表达式和它是等价的：

  ```
  a[0:10]
  a[:10]
  a[0:]
  a[:]
  ```

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)

	s = s[:2]
	fmt.Println(s)

	s = s[1:]
	fmt.Println(s)
}

```

这段代码输出：

```text
[3 5 7]
[3 5]
[5]
```

####切片字面量

* 切片字面量类似于没有长度的数组字面量。

这是一个数组字面量：

```
a [3]bool{true, true, false}
```

也可以像下面这样声明。这样会创建一个和上面相同的数组，然后再构建一个引用了它的切片：

```
a []bool{true, true, false}
```

####长度和容量

* 切片的长度就是它所包含的元素个数。

* 切片的容量是从==它的==第一个元素开始数，到==其底层数组==元素末尾的个数。

* 切片 `s` 的长度和容量可通过表达式 `len(s)` 和 `cap(s)` 来获取。

* 可以通过重新切片来扩展一个切片，给它提供足够的容量。

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)
	
	a := s[:1]
	printSlice(a)
	
	a = s[2:6]
	printSlice(a)
	
	a = s[:5]
	printSlice(a)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

```

这段代码输出：

```text
len=6 cap=6 [2 3 5 7 11 13]
len=1 cap=6 [2]
len=4 cap=4 [5 7 11 13]
len=5 cap=6 [2 3 5 7 11]
```

* 切片的零值是 `nil`。

  nil 切片的长度和容量为 0 且没有底层数组

####make创建切片（动态数组）

`make` 函数会分配一个元素为零值的数组并返回一个引用了它的切片：

```go
a := make([]int, 5)  // len(a)=5
```

要指定它的容量，需向 `make` 传入第三个参数：

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

```go
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)

	b := make([]int, 0, 5)
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

这段代码输出：
```text
a len=5 cap=5 [0 0 0 0 0]
b len=0 cap=5 []
c len=2 cap=5 [0 0]
d len=3 cap=3 [0 0 0]
```

### append 函数

```
func append(s []T, vs ...T) []T
```

* `append` 的第一个参数 `s` 是一个元素类型为 `T` 的切片，其余类型为 `T` 的值将会追加到该切片的末尾。

* `append` 的结果是一个包含原切片所有元素加上新添加元素的切片。

* 当 `s` 的底层数组太小，不足以容纳所有给定的值时，它就会分配一个更大的数组。 返回的切片会指向这个新分配的数组。

了解关于切片的更多内容，请阅读文章 [Go 切片：用法和本质](https://tour.go-zh.org/blog/go-slices-usage-and-internals)。

```go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// 可在空切片上追加
	s = append(s, 0)
	printSlice(s)

	// 这个切片会按需增长
	s = append(s, 1)
	printSlice(s)

	// 可以一次性添加多个元素
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

这段代码输出：
```text
len=0 cap=0 []
len=1 cap=1 [0]
len=2 cap=2 [0 1]
len=5 cap=6 [0 1 2 3 4]
```

### range 函数

* 当使用 `for` 循环遍历切片时，每次迭代都会返回两个值。 第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

这段代码输出：

```text
2**0 = 1
2**1 = 2
2**2 = 4
2**3 = 8
2**4 = 16
2**5 = 32
2**6 = 64
2**7 = 128
```

* 可以将下标或值赋予 `_` 来忽略它。

```
for i, _ := range pow
for _, value := range pow
```

* 若你只需要索引，忽略第二个变量即可。

```
for i := range pow
```

## map 映射

###定义

* `map` 映射将键映射到值。

* 映射的零值为 `nil` 。`nil` 映射既没有键，也不能添加键。

* `make` 函数会返回给定类型的映射，并将其初始化备用。

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

```

### 映射字面量

* 映射的字面量和结构体类似，只不过必须有键名

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
```

* 若顶层类型只是一个类型名，那么可以在字面量的元素中省略它：

```go
type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

### 修改映射

* 在映射 `m` 中插入或修改元素：

```
m[key] = elem
```

* 获取元素：

```
elem = m[key]
```

* 删除元素：

```
delete(m, key)
```

* 通过双赋值检测某个键是否存在：

```
elem, ok = m[key]
```

若 `key` 在 `m` 中，`ok` 为 `true` ；否则，`ok` 为 `false`。

若 `key` 不在映射中，则 `elem` 是该映射元素类型的零值。

==**注**==：若 `elem` 或 `ok` 还未声明，你可以使用短变量声明：

```
elem, ok := m[key]
```

```go
package main

import "fmt"

func main() {
	m := make(map[string]int)

	m["答案"] = 42
	fmt.Println("值：", m["答案"])

	m["答案"] = 48
	fmt.Println("值：", m["答案"])

	delete(m, "答案")
	fmt.Println("值：", m["答案"])

	v, ok := m["答案"]
	fmt.Println("值：", v, "是否存在？", ok)
}
```

这段代码输出：

```text
值： 42
值： 48
值： 0
值： 0 是否存在？ false
```

