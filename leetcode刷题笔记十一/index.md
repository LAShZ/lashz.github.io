# LeetCode 剑指Offer 13

<!--more-->

## 题目：机器人的运动范围

[题目链接](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)。

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

> 示例 1：

**输入**：

m = 2, n = 3, k = 1

**输出**：

3

> 示例2：

**输入**:

m = 3, n = 1, k = 0

**输出**:

1

**限制**：

1 <= m,n <= 100

0 <= k <= 20

## 题解

本题依然是经典的**DFS**，只需使用一个数组存放各个坐标的状态，然后在访问之前判断该坐标的周围的点是否可以被访问即可。

```go
func movingCount(m int, n int, k int) int {
	block := make([][]bool, m)
	for i := range block {
		block[i] = make([]bool, n)
	}
	return dfs(0, 0, m, n, k, block)
}

func dfs(x, y, m, n, k int, block [][]bool) int {
	if !numberCount(x, y, k) || x >= m || y >= n || block[x][y]{
		return 0
	}
	block[x][y] = true
	sum := 1
	sum += dfs(x+1, y, m, n, k, block)
	sum += dfs(x-1, y, m, n, k, block)
	sum += dfs(x, y+1, m, n, k, block)
	sum += dfs(x, y-1, m, n, k, block)
	return sum
}

func numberCount(x int, y int, k int) bool {
	if x < 0 || y < 0 {
		return false
	}
	sum := 0
	for ; x > 0; x /= 10 {
		sum += x %10
	}
	for ; y > 0; y /= 10 {
		sum += y %10
	}
	if sum <= k {
		return true
	}
	return false
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.6 MB

不过考虑到这是一个方块内四个方向上的移动，且起点处固定且绝对可达，可以将运动方向固定为向右和向上简化运算，代码如下：

```go
func movingCount(m int, n int, k int) int {
	block := make([][]bool, m+1)
	for i := range block {
		block[i] = make([]bool, n+1)
	}

    // 起点处绝对可达
	block[0][0] = true
	count := 1

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
            // 判断左边和下方是否可达
			if ((i-1 >= 0 && block[i-1][j]) || (j-1 >= 0 && block[i][j-1])) && numberCount(i, j, k) {
				count++
				block[i][j] = true
			}
		}
	}
	return count
}
func numberCount(x int, y int, k int) bool {
	if x < 0 || y < 0 {
		return false
	}
	sum := 0
	for ; x > 0; x /= 10 {
		sum += x %10
	}
	for ; y > 0; y /= 10 {
		sum += y %10
	}
	if sum <= k {
		return true
	}
	return false
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2 MB
