# LeetCode 剑指Offer 17

<!--more-->

## 题目：打印从1到最大的n位数

[题目链接](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

输入数字`n`，按顺序打印出从1到最大的n位十进制数。比如输入3，则打印出1、2、3一直到最大的3位数999。

> 示例 1：

**输入**：

n=1

**输出**：

[1,2,3,4,5,6,7,8,9]

## 题解

先通过$10^{n-1}+9$获取需要的切片大小，然后循环将数字填入即可。

```go
func printNumbers(n int) []int {
	max := 0
	for ; n > 0; n-- {
		max = max*10 + 9
	}
	res := make([]int, max)
	for i := 1; i <= max; i++ {
		res[i-1] = i
	}
	return res
}
```

> 执行用时: 8 ms
>
> 内存消耗: 7 MB

## 进阶

上述AC代码未考虑大数越界的情况，虽然测试集中没有，但是仍然是一个非常值得思考的问题。

未完待续...
