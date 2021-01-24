# Go语言string和[]byte互相转换

<!--more-->

Go语言中有两种可以储存一串字符的类型，分别是字符串`string`和byte类型数组`[]byte`，但是他们直接并不能直接使用等于号赋值，也不能单纯的转换，而是要通过切片来进行转换。

## string转换成[]byte

可以直接使用`[]byte(str)`强制转换。

```go
package main

import "fmt"

func main() {
	var str string = "test"
	var data []byte = []byte(str)
    fmt.Println("string: ", str)	// string: test
    fmt.Println("[]byte: ", data)	// []byte: [116 101 115 116]
}
```

可以看到[]byte输出的结果正是string的各个字符对应的ASCII码。

## []byte转换成string

要将byte数组转换成string不能直接转换，而需要将[]byte的切片转换。即使用`string(数组名[:])`进行转换。

```go
package main

import "fmt"

func main() {
	var data [5]byte
	data[0] = 'T'
	data[1] = 'E'
	data[2] = 'S'
	data[3] = 'T'
	var str string = string(data[:])
	fmt.Println("[]byte: ", data)	// []byte: [84 69 83 84 0]
	fmt.Println("string: ", str)	// string: TEST
}
```

## 实践环节

将[][]byte类型的二维数组board初始化为[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]

```go
board := [][]byte{[]byte("ABCE"),[]byte("SFCS"),[]byte("ADEE")}
```

由于Go语言需要所有变量（包括中间结果）都有确定的值，所以在声明过程中，需要先将(board[0])[]，(board[1])[]，(board[2])[]都赋予确定的值才能生成board，所以需要在定义内部就进行转换。


