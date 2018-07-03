## 习题

#### 计算两个大数相加的和，这两个大数会超过int64的表示范围.
```
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func multi(str1, str2 string) (result string) {

	if len(str1) == 0 && len(str2) == 0 {
		result = "0"
		return
	}

	var index1 = len(str1) - 1
	var index2 = len(str2) - 1
	var left int

	for index1 >= 0 && index2 >= 0 {
		c1 := str1[index1] - '0'
		c2 := str2[index2] - '0'

		sum := int(c1) + int(c2) + left
		if sum >= 10 {
			left = 1
		} else {
			left = 0
		}
		c3 := (sum % 10) + '0'
		result = fmt.Sprintf("%c%s", c3, result)
		index1--
		index2--
	}

	for index1 >= 0 {
		c1 := str1[index1] - '0'
		sum := int(c1) + left
		if sum >= 10 {
			left = 1
		} else {
			left = 0
		}
		c3 := (sum % 10) + '0'

		result = fmt.Sprintf("%c%s", c3, result)
		index1--
	}

	for index2 >= 0 {
		c1 := str2[index2] - '0'
		sum := int(c1) + left
		if sum >= 10 {
			left = 1
		} else {
			left = 0
		}
		c3 := (sum % 10) + '0'
		result = fmt.Sprintf("%c%s", c3, result)
		index2--
	}

	if left == 1 {
		result = fmt.Sprintf("1%s", result)
	}
	return
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	result, _, err := reader.ReadLine()
	if err != nil {
		fmt.Println("read from console err:", err)
		return
	}

	strSlice := strings.Split(string(result), "+")
	if len(strSlice) != 2 {
		fmt.Println("please input a+b")
		return
	}

	strNumber1 := strings.TrimSpace(strSlice[0])
	strNumber2 := strings.TrimSpace(strSlice[1])
	fmt.Println(multi(strNumber1, strNumber2))
}
```


#### 输入一行字符，分别统计出其中英文字母、空格、数字和其它字符的个数。

```
package main

import (
	"bufio"
	"fmt"
	"os"
)

func count(str string) (worldCount, spaceCount, numberCount, otherCount int) {

	t := []rune(str)
	for _, v := range t {
		switch {
		case v >= 'a' && v <= 'z':
			fallthrough
		case v >= 'A' && v <= 'Z':
			worldCount++
		case v == ' ':
			spaceCount++
		case v >= '0' && v <= '9':
			numberCount++
		default:
			otherCount++
		}
	}

	return
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	result, _, err := reader.ReadLine()
	if err != nil {
		fmt.Println("read from console err:", err)
		return
	}
	wc, sc, nc, oc := count(string(result))
	fmt.Printf("wolrd count:%d\n space count:%d\n number count:%d\n others count:%d\n", wc, sc, nc, oc)
}

```

####   输入一个字符串，判断其是否为回文。回文字符串是指从左到右读和从右到左读完全相同的字符串。
```
package main

import "fmt"

func process(str string) bool {

	t := []rune(str)
	length := len(t)
	for i, _ := range t {

		if i == length/2 {
			break
		}

		last := length - i - 1
		if t[i] != t[last] {
			return false
		}
	}

	return true
}

func main() {
	var str string
	fmt.Scanf("%sd", &str)
	if process(str) {
		fmt.Println("yes")
	} else {
		fmt.Println("no")
	}
}

```
#### 一个数如果恰好等于它的因子之和，这个数就称为"完数"。例如6=1＋2＋3.编程找出1000以内的所有完数。
```
package main

import "fmt"

func perfect(n int) bool {

	var sum int = 0
	for i := 1; i < n; i++ {
		if n%i == 0 {
			sum += i
		}
	}

	return n == sum
}

func process(n int) {
	for i := 1; i < n+1; i++ {
		if perfect(i) {
			fmt.Println(i)
		}
	}
}

func main() {
	var n int
	fmt.Scanf("%d", &n)
	process(n)
}

```


#### 编写程序，在终端输出九九乘法表。
```
package main

import "fmt"

func multi() {
	for i := 0; i < 9; i++ {
		for j := 0; j <= i; j++ {
			fmt.Printf("%d*%d=%d\t", (i + 1), j+1, (i+1)*(j+1))
		}
		fmt.Println()
	}
}

func main() {
	multi()
}

```

#### 判断 101-200 之间有多少个素数，并输出所有素数。 
```
package main

import (
	"fmt"
	"math"
)

func isPrime(n int) bool {

	for i := 2; i <= int(math.Sqrt(float64(n))); i++ {
		if n%i == 0 {
			return false
		}
	}
	return true
}

func main() {
	var n int
	var m int

	fmt.Scanf("%d%d%s", &n, &m)
	for i := n; i < m; i++ {
		if isPrime(i) == true {
			fmt.Printf("%d\n", i)
			continue
		}
	}
}

```


####  打印出100-999中所有的“水仙花数”，所谓“水仙花数”是指一个三位数，其各位数字立方和等于该数本身。例如：153 是一个“水仙花数”，因为 153=1 的三次方＋5 的三次方＋3 的三次方。

```
方法一:
package main

import "fmt"

func isNumber(n int) bool {
	var i, j, k int
	i = n % 10
	j = (n / 10) % 10
	k = (n / 100) % 10

	sum := i*i*i + j*j*j + k*k*k
	return sum == n
}

func main() {
	var n int
	var m int

	fmt.Scanf("%d,%d", &n, &m)

	for i := n; i < m; i++ {

		if isNumber(i) == true {
			fmt.Println(i)
		}
	}
}

方法二：
package main

import (
	"fmt"
	"strconv"
)

func main() {
	var str string
	fmt.Scanf("%s", &str)

	var result = 0
	for i := 0; i < len(str); i++ {
		num := int(str[i] - '0')
		result += (num * num * num)
	}

	number, err := strconv.Atoi(str)
	if err != nil {
		fmt.Printf("can not convert %s to int\n", str)
		return
	}

	if result == number {
		fmt.Printf("%d is shuixianhuashu\n", number)
	} else {
		fmt.Printf("%d is not shuixianhuashu\n", number)
	}
}


```

#### 对于一个数n，求n的阶乘之和，即： 1！ + 2！ + 3！+…n!

```
package main

import "fmt"

func sum(n int) uint64 {

	var s uint64 = 1
	var sum uint64 = 0
	for i := 1; i <= n; i++ {
		s = s * uint64(i)
		fmt.Printf("%d!=%v\n", i, s)
		sum += s
	}
	return sum
}

func main() {
	var n int

	fmt.Scanf("%d", &n)

	s := sum(n)
	fmt.Println(s)
}

```

#### 求一个数的阶乘

```
func n_factor(n int)  int{
	if n ==1 {
		return 1
	}
	return n_factor(n-1)*n
}

func main() {
    n := 5
    fmt.Println(n_factor(n))
}
```

#### 斐波那契数
 ```
递归方法
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
 ```
 非递归方法
 
 func fab2(n int) []uint64{
	 var a []uint64
	 a = make([]uint64,n)
	 a[0] = 1
	 a[1] = 1
	for i:=2;i<n ; i++ {
		a[i] = a[i-1]+a[i-2]
	}
	return a
}
 ```
 
 




