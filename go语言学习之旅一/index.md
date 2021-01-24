# Go语言学习之旅(一)

<!--more-->
> *Go语言是21世纪的C语言*

本文是用于记录作者在Go语言的学习过程中经历的种种，方便日后复习，也为他人提供些许的经验参考。

本文开始于作者大三寒假时期，对于数据结构、算法、计算机网络等知识都有一定的了解，也有过一定的项目经验，故一些较为基础的知识不再予以记录，如若读者遇到了难以理解的部分，可以上网自行寻找相关的资料，若仍然无法理解，请联系作者加以解释。也欢迎各位对本文中出现的错误以及不足之处予以指正。

转载请注明出处。

## GO语言的安装

### *Windows下的安装*

由于本文作者主要使用的操作系统仍是Windows（没钱买Mac），故首先尝试的是在Windows下安装GO。

进入[GO语言官方下载地址](https://golang.org/dl/)，点击Microsoft Windows下方的下载链接自动下载最新的GO语言稳定版安装器。下载完成后运行选择Install即可完成安装。

若安装过Windows下的包管理器如*Scoop*、*Chocolatey*等，也可以以使用指令：

```shell
scoop install go
```

或是

```shell
choco install go
```

进行安装。

### Linux下的安装

本文作者目前使用的Linux发行版为Debian系下的Ubuntu 20.04和Deepin 20，故使用的指令均为Debian系下的操作。

直接打开终端，输入

```shell
sudo apt install golang
```

即可完成安装。

## GO语言的IDE

一个好的IDE可以使代码的编写事半功倍。本文作者使用的IDE为JetBrains推出的GoLand，平时对于小型代码的编写也会使用Visual Studio Code配上Go语言插件直接进行编写。

两个软件的下载安装配置工作请读者自行学习，本文不再赘述。

## GO语言的语法

Go语言作为21世纪的C语言，许多语法特性与C语言类似，如需要对每个变量的类型都进行声明等，本文不对Go语言的语法基础细节做过多的说明，对于一些基本的语法使用只需参照官方文档或是任意一本参考书即可，本文只对作者学习过程中遇到的较为难以理解或是复杂的概念做一个记录说明。

### 变量与常量声明

Go的常量声明使用`const`关键字：`const identifier [type] = value`

Go的变量声明使用`var`关键字：`var identifier type`

可以注意到的是，Go语言的变量声明与其他语言最为不同的一点是，**类型放在变量名之后**，这点在之后的各个地方都会被使用到，也是新手学习Go语言要跨过的第一个障碍。下面给出几个Go语言声明变量的例子：

```Go
var a int
var b,c *int
var d bool
// 使用因式分解关键字写法，一般用于全局变量
var (
	str string
	array []int
)
```

可以看出这样声明可以使得变量的类型定义清晰，每一行声明的变量都一定是同一个类型的。

Go语言的 变量命名需要遵循骆驼命名法，如`myArray`和`startDate`。

Go语言的变量在声明的时候会被赋予该类型的初值：int为0，float为0.0，bool为false，string为空字符串，指针为nil。Go中所有的变量都是经过**初始化**的。Go语言也支持变量在声明的时候进行初始化。同时，若在初始化时不指定类型，Go语言会根据初始化的值自动判断类型，但是没有初始化值且没有类型的变量声明是不被允许的。

由于自动判断特性，Go语言在声明时支持简短声明语法`:=`，如`a := 1`，此时a自动被初始化成int型的变量，且值为1。（该种用法只能在一个代码块内对同一个变量名使用一次）

Go语言也支持对多个变量同时声明或是同时赋值

```go
a, b, c := 5, 7, "abc"	//变量之前未被声明
d, e, f = 1, 3, "def"	//变量之前已被声明
a, b = b, a	//交换两个变量的值
_, b = 5, 7	//使用只写变量"_"抛弃值
```

最后，在每个源文件中都支持包含一个init函数，支持对全局变量的初始化。这是一类非常特殊的函数，它不能够被人为调用，而是在每个包完成初始化后自动执行，并且执行优先级比 main 函数高。如下面的例子中便将Pi初始化为了3.1415

```go
var Pi float64

func init() {
   Pi = 3.1415
}
```

### 字符串

与Python字符串类似，支持`+`拼接字符串，支持len(str)获取字符串长度，也支持str[]获取内容。若是导入了`strings`包或是`strconv`包则支持更多的特性。相关的函数接口功能参照该[链接](https://github.com/unknwon/the-way-to-go_ZH_CN/blob/master/eBook/04.7.md)，做了较为详细的说明。

### 指针

Go语言与C语言类似也有指针，但是Go语言的指针不允许进行指针算法，如`pointer+2`或是`pointer++`。

Go的指针会进行自动反向引用，如

```go
v := 1
var p *int
p = &v
fmt.printLn("v: %d p: %d p: %p\n",v, p, p)	//Output: V: 1 p: 1 p: 0x24f0820
```

### if-else结构

标准结构如下

```go
if condition1 {
	// do something	
} else if condition2 {
	// do something else	
} else {
	// catch-all or default
}
```

### switch结构

Go语言中的switch结构接收任意形式的表达式，甚至可以不接收表达式，直接在case中使用语句进行判断（而非C语言一样之能接收整型数据），且不需要使用break语句来结束，当需要继续执行后面的代码时，可以使用`fallthrough`关键字来达到目的。

```go
switch a, b := x[i], y[j]; {
	case a < b: t = -1
	case a == b: t = 0
	case a > b: fallthrough
    case default: t = 1
}
```

在上面这个代码片段中，变量 a 和 b 被平行初始化，然后作为判断条件。

### for结构

基本用法与C语言类似，只是不需要小括号。

for是Go语言提供的**唯一一种**循环结构，Go语言**没有**`while`或是`do-while`循环，因为都可以以for循环的形式实现。

Go也支持一种特殊的迭代结构**for-range**，一般形式为：`for ix, val := range coll {}`。其中`val`是集合中对应值的**拷贝**，对其的修改不会改变原集合中的值。

> *未完待续*...
