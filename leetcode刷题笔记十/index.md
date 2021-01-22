# LeetCode 剑指Offer 12

<!--more-->
# LeetCode刷题笔记--剑指Offer 12

## 题目：矩阵中的路径

[题目链接](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)。

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","**b**","c","e"], 

["s","**f**","**c**","s"],

["a","d","**e**","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

> 示例 1：

**输入**：

board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"

**输出**：

true

> 示例2：

**输入**:

board = [["a","b"],["c","d"]], word = "abcd"

**输出**:

false

**限制**：

1 <= board.length <= 200

1 <= board[i].length <= 200

## 题解

本题为经典的DFS，只需注意要将已经选过的字符设置为空字符即可。具体的算法过程见代码注释：

```go
func exist(board [][]byte, word string) bool {
	col := len(board[0])
    line := len(board)
    for i := 0; i < line; i++ {
        for j := 0; j < col; j++ {
            //找到第一个值，然后DFS递归查找整条路径
            if dfs(board, word , i, j, 0) {
				return true
            }
        }
    }
    return false
}

func dfs(board [][]byte, word string, i int, j int, k int) bool {
    //找到最后一个数，返回true
	if k == len(word) {
		return true
	}

    //i, j超范围
	if i < 0 || j < 0 || i >= len(board) || j >= len(board[0]) {
		return false
	}

    //进入DFS深度优先搜索
    //先把正在遍历的该值重新赋值，如果在该值的周围都搜索不到目标字符，则再把该值还原
    //如果在数组中找到第一个字符，则进入下一个字符的查找
	if board[i][j] == word[k] {
		temp := board[i][j]
		board[i][j] = ' '
        //递归在周围寻找
		if dfs(board, word, i+1, j, k+1) ||
		dfs(board, word, i, j+1, k+1) ||
		dfs(board, word, i-1, j, k+1) ||
		dfs(board, word, i, j-1, k+1) {
			return true
		}else {
            //没有找到目标字符，还原之前重新赋值的board[i][j]
			board[i][j] = temp
		}
	}
	return false
}
```

执行用时: 4 ms

内存消耗: 3.2 MB


