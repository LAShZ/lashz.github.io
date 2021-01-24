# LeetCode 剑指Offer 10-I

<!--more-->
## 题目：斐波那契数列

[题目链接](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)。

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

> 示例 1：

**输入**：

n = 2

**输出**：

1

> 示例2：

**输入**:

n = 5

**输出**:

5

**限制**：

0  <= n <= 100

## 题解

本题也是很经典的基础题，可以使用递归与非递归的解法。也可以选择保存Fibonacci数列的所有值或是只保存所求值。下面给出代码：

### 递归解法

```go
var fibs [101]int	//保存Fibonacci数列中的各个元素值

func fib(n int) int {
    if fibs[n] != 0{
        return fibs[n] % (1e9+7)
    }
    if n <= 1{	//F(0) = 0, F(1) = 1
        fibs[n] = n
    }else{	//递归
        fibs[n] = fib(n-1) + fib(n-2)
    }
    return fibs[n] % (1e9+7)
}
```
执行用时: 0 ms

内存消耗: 1.9 MB (1928 KB)
### 非递归解法

```go
func fib(n int) int {
    f0, f1 := 0, 1
    for i := 0; i < n; i++ {
        f0,f1 = f1,(f0 + f1) % (1e9 + 7);
    }
    return f0
}
```
执行用时: 0 ms

内存消耗: 1.9 MB (1924 KB)

第二种解法执行用时及内存消耗均超过100%的用户。


