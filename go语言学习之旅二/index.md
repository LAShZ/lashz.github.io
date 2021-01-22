# Go语言学习之旅(二)

<!--more-->
# Go语言学习之旅

> *Go语言是21世纪的C语言*

本文是用于记录作者在Go语言的学习过程中经历的种种，方便日后复习，也为他人提供些许的经验参考。

本文开始于作者大三寒假时期，对于数据结构、算法、计算机网络等知识都有一定的了解，也有过一定的项目经验，故一些较为基础的知识不再予以记录，如若读者遇到了难以理解的部分，可以上网自行寻找相关的资料，若仍然无法理解，请联系作者加以解释。也欢迎各位对本文中出现的错误以及不足之处予以指正。

转载请注明出处。

## Go语言语法

### 函数

#### 函数定义

```go
func g() {
}
```

需要注意的是，左花括号必须与函数名在同一行，否则无法通过编译。

#### 函数参数与返回值

函数可以接收多个参数，并返回多个值，且可以对返回值命名。 

```go
func getX2AndX3_2(input int) (x2 int, x3 int) {
    x2 = 2 * input
    x3 = 3 * input
    // return x2, x3
    return
}
```

在函数有多个返回值时，只要有一个返回值有命名，其他的也必须命名，如以下代码片段就会报错，因为error类型的返回值没有命名。

```go
func funcMui(x,y int)(sum int,error){
	return x+y,nil
}
```

如果有多个返回值必须加上括号()；如果只有一个返回值且命名也必须加上括号()。

如果函数的最后一个参数使用`...type`的形式，则可以传入变长的参数。如果参数被储存在一个slice类型的变量`slice`中，则可以通过`slice...`的形式传递参数，调用变参函数。

```go
package main

import "fmt"

func main() {
	x := min(1, 3, 2, 0)
	fmt.Printf("The minimum is: %d\n", x)
	slice := []int{7,9,3,5,1}
	x = min(slice...)
	fmt.Printf("The minimum in the slice is: %d", x)
}

func min(s ...int) int {
	if len(s)==0 {
		return 0
	}
	min := s[0]
	for _, v := range s {
		if v < min {
			min = v
		}
	}
	return min
}

/* Output:
The minimum is: 0
The minimum in the slice is: 1
*/
```

#### defer

defer是Go中一个特殊的关键字，用于将某函数或是语句的执行顺序推迟到函数返回之前，使得函数在返回时进行一些操作，当有多个defer语句时，其执行顺序为**先进后出**。通过defer语句可以进行进程中止前的收尾，代码追踪，记录函数参数及返回值等功能，如以下的例子：

```go
package main

import (
	"io"
	"log"
)

func func1(s string) (n int, err error) {
	defer func() {
		log.Printf("func1(%q) = %d, %v", s, n, err)
	}()
	return 7, io.EOF
}

func main() {
	func1("Go")
}

/* Output:
2021/1/2 22:46:11 func1("Go") = 7, EOF
*/
```

#### 内置函数

Go 语言拥有一些不需要进行导入操作就可以使用的内置函数，可以理解为某种类型的关键字，但是是以函数的方式实现的。以下给出一个简单的列表，之后会进行深入讨论：

| 名称               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| close              | 用于管道通信                                                 |
| len、cap           | len 用于返回某个类型的长度或数量（字符串、数组、切片、map 和管道）；cap 是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map） |
| new、make          | new 和 make 均是用于分配内存：new 用于值类型和用户定义的类型，如自定义结构，make 用于内置引用类型（切片、map 和管道）。它们的用法就像是函数，但是将类型作为参数：new(type)、make(type)。new(T) 分配类型 T 的零值并返回其地址，也就是指向类型 T 的指针。它也可以被用于基本类型：`v := new(int)`。make(T) 返回类型 T 的初始化之后的值，因此它比 new 进行更多的工作。**new() 是一个函数，不要忘记它的括号** |
| copy、append       | 用于复制和连接切片                                           |
| panic、recover     | 两者均用于错误处理机制                                       |
| print、println     | 底层打印函数，在部署环境中建议使用 fmt 包                    |
| complex、real imag | 用于创建和操作复数                                           |

> *未完待续*...
