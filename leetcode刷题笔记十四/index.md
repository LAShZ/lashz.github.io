# LeetCode 剑指Offer 16

<!--more-->

## 题目：数值的整数次方

[题目链接](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

> 示例 1：

**输入**：

2.00000, 10

**输出**：

1024.00000

> 示例2：

**输入**:

2.10000, 3

**输出**:

9.26100

> 示例3：

**输入**:

2.00000, -2

**输出**:

31

**限制**：

- -100.0 < x < 100.0
- n是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。

## 题解

使用**快速幂乘法**，通过以下公式不断递归：

$$
x^n = \begin{cases}
	{(x^2)^{\frac{n}{2}}} &{n为偶数} \\cr  %这里要加cr不然在博客中无法显示，但是加了会导致typora中显示多一个cr
	{x*(x^2)^{\frac{n}{2}}} &{n为奇数} 
\end{cases}
$$
同时使用位操作，用`n>>1`代替`n/2`，用`n&0x1`代替`n%2`，得到最后代码如下：

```go

func myPow(x float64, n int) float64 {
    if n == 0{
        return 1
    }
    if n == 1{
        return x
    }
    if n<0{
        x = 1/x
        n = -n
    }
    temp := myPow(x , n>>2)
    if n&0x1 == 0{
        return temp*temp
    }
    return x*temp*temp
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.1 MB


