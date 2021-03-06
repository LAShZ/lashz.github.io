# LeetCode 剑指Offer 15

<!--more-->

## 题目：二进制中1的个数

[题目链接](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof)

请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

> 示例 1：

**输入**：

00000000000000000000000000001011

**输出**：

3

**解释**:

输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。

> 示例2：

**输入**:

00000000000000000000000010000000

**输出**:

1

**解释**:

输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。

> 示例3：

**输入**:

11111111111111111111111111111101

**输出**:

31

**解释**:

输入的二进制串11111111111111111111111111111101中，共有32位为 '1'。

**限制**：

输入必须是长度为 `32` 的 **二进制串** 

## 题解

直接按位判断即可。

```go
func hammingWeight(num uint32) int {
    var sum uint32
    for num != 0 {
        sum += (num & 0x1)
        num = num >> 1
    }
    return int(sum)
}
```

> 执行用时: 0 ms
>
> 内存消耗: 1.9 MB

看到一个牛逼的[解法](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/solution/mian-shi-ti-15-er-jin-zhi-zhong-1de-ge-shu-wei-yun/)：

(n−1) 解析： 二进制数字n最右边的1变成0，此1右边的0都变成1。

n \& (n - 1)解析： 二进制数字n最右边的1变成0，其余不变。

故只需要:

1. 初始化数量统计变量 res。
2. 循环消去最右边的1：当 n = 0 时跳出。
   1. res += 1 ： 统计变量加1；
   2. n &= n - 1 ： 消去数字n最右边的1 。
3. 返回统计数量 res 。

```go
func hammingWeight(num uint32) int {
    sum := 0
    for num != 0 {
        sum++
        num &= num-1
    }
    return sum
}
```

> 执行用时: 0 ms
>
> 内存消耗: 1.9 MB

该解法时间复杂度要优于第一种解法。
