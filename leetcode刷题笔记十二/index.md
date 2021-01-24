# LeetCode 剑指Offer 14

<!--more-->

## 题目：剪绳子I && 剪绳子II

题目链接:

[剪绳子I](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

[剪绳子II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

给你一根长度为`n`的绳子，请把绳子剪成整数长度的`m`段（m、n都是整数，n>1并且m>1），每段绳子的长度记为`k[0],k[1]...k[m - 1]`。请问`k[0]*k[1]*...*k[m - 1]`可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

剪绳子II的答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

> 示例 1：

**输入**：

2

**输出**：

1

> 示例2：

**输入**:

10

**输出**:

36  (解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36)

**限制**：

2 <= n <= 1000

## 题解

本题其实为一道数学题。

将长度为n的绳子切成a段：
$$
n = n_1+n_2+...+n_a
$$
等价于求:
$$
max(n_1\*n_2\*...\*n_a)
$$

下面进行推导证明：**当全部切成长度为3的绳段时乘积最大**

### 推导

由均值不等式：
$$
\frac{n_1+n_2+...+n_a}{a} \geqslant \sqrt[{a}]{n_1n_2...n_a}
$$
可以知道当$ n_1=n_2=...=n_a $时乘积最大，此时每一段绳子的长度都为$ \frac{n}{a} $，乘积为$ x^a $。且由：
$$
x^{{a}}=x^{\frac{{n}}{{x}}}=(x^{\frac{1}{{x}}})^{{n}}
$$
可知只需求出$ x^{\frac{1}{x}}  $的最大值即可。通过求导可知当$ x=\textit{e} $时取得最大值，又由于$x$必须为整数，故取得$x=3$时得到的结果最大。又由于n不一定能被整除，故当余数为1时需要将3+1的情况替换为乘积更大的2+2。

对于剪绳子II，因为得到的数可能极大，故需要考虑越界问题，对每一步中间结果取余，且对于得到的指数也需要判断是否超范围。

### AC代码

剪绳子I:

```go
func cuttingRope(n int) int {
	if n < 4 {
		return n - 1
	}
	if n == 4 {
		return 4
	}
	ans := int(math.Pow(3, float64(n/3)))
	remain := n % 3
	if remain == 1 {
		ans = ans / 3 * 4
	}
	if remain == 2 {
		ans = ans * 2
	}
	return ans
}
```

> 执行用时: 0 ms
>
> 内存消耗: 1.9 MB

剪绳子II:

```go
func cuttingRope(n int) int {
    if n <= 3 {
        return n - 1
    }
    factor := n / 3 - 1
    maxFactor := 19
    base := int(math.Pow(3, float64(maxFactor))) % 1000000007
    sum := 1
    for factor > maxFactor {	\\若指数超出了范围
        sum = (sum * base) % 1000000007
        factor -= maxFactor
    }
    sum = (sum * int(math.Pow(3, float64(factor)))) % 1000000007
    remain := n % 3
    if remain == 0 {
        return (sum * 3) % 1000000007
    }
    if remain == 1 {
        return (sum * 4) % 1000000007
    }
    return (sum * 6) % 1000000007
}
```

> 执行用时: 0 ms
>
> 内存消耗: 1.9 MB

### DP解法

对于剪绳子I也可以通过动态规划的方法求解。

对于每一次切割，都考虑是直接切割相乘还是将前面的部分再次切割之后相乘。

状态转移方程为：dp[i]=max(dp[i], max(j*dp[i-j], j\*(i-j)))

初始状态为dp[2]=1 (由于m>1，必须至少一次切割)

```go
func cuttingRope(n int) int {
	dp := make([]int, n+1)
	dp[2] = 1
	for i := 3; i < n+1; i++ {
		for j := 2; j < i; j++ {
			dp[i] = int(math.Max(float64(dp[i]),math.Max(float64(j * dp[i-j]),float64(j*(i-j)))))
		}
	}
	return dp[n]
}
```

> 执行用时: 0 ms
>
> 内存消耗: 1.9 MB

但是对于剪绳子II，有超过范围的数字的情况，使用DP无法得到正确的中间结果，也就无法得到正确的最终结果。
