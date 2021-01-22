# LeetCode 剑指Offer 04

<!--more-->
# LeetCode刷题笔记--剑指Offer 04

## 题目：二维数组中的查找

[题目链接](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)。

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

> 示例 

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。

**限制**：

0 <= n <= 1000

0 <= m <= 1000

## 题解

这道题最简单的做法就是遍历整个数组，每一个元素都拿来比较，找到了就返回`true`，遍历完之后还没找到就返回`false`，但这种方法的时间复杂度为O(NM)，且并未利用数组内元素递增的特点。

对于这道题我们先将数组画出来，可以发现对于任意一个元素，总是要大于它的左下方的元素。

![img1](https://cdn.jsdelivr.net/gh/LAShZ/blog-pic-repo@main//img/%E5%89%91%E6%8C%87Offer04-1.png)

所以，我们可以先在第一行进行搜索，找到第一个`大于`目标值的数，然后一直朝着**左下方**搜索，该处的值`大于`目标值就`朝左`，`小于`目标值就`朝右`。整个搜索过程中，找到对应值就返回true，超出边界就立即停止搜索，并返回false。在上图中搜索14的过程如下图所示：

![img2](https://cdn.jsdelivr.net/gh/LAShZ/blog-pic-repo@main/img/%E5%89%91%E6%8C%87Offer04-2.png)

程序代码如下：

```go
func findNumberIn2DArray(matrix [][]int, target int) bool {
    i := 0
    if (len(matrix) == 0 || len(matrix[0]) == 0) {	// 为空数组直接返回false
        return false
    }
	for ; ((i < len(matrix[0]) - 1)  && matrix[0][i] < target); i++ {	// 在第一行大于target的或是超出边界
	}
	for j := 0; ; {
	    if i < 0 || j > (len(matrix)-1) {
	        return false
	    }
	    if matrix[j][i] == target {
            return true
        } else if matrix[j][i] > target {	// 朝左
            i--
        } else {	// 朝下
            j++
        }
    }
}
```

执行用时: 24 ms

内存消耗: 6.6 MB
