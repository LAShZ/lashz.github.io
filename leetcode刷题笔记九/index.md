# LeetCode 剑指Offer 11

<!--more-->
## 题目：旋转数组的最小数字

[题目链接](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)。

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。

> 示例 1：

**输入**：

 [3, 4, 5 , 1, 2]

**输出**：

1

> 示例2：

**输入**:

[2, 2, 2, 0 , 1]

**输出**:

0

## 题解

本题最容易想到的解法为直接遍历整个数组，找到第一个小于前一个元素的值的元素，若都相等则返回第一个元素即可，代码如下：

```go
func minArray(numbers []int) int {
    if len(numbers) == 0 {
        return -1
    }
    
    for seq:= 1; seq < len(numbers); seq++ {
       if numbers[seq] < numbers[seq-1]{
           return numbers[seq]
       }
    }
    return numbers[0]
}
```

执行用时: 4 ms

内存消耗: 3.1 MB

十分简单且结果也不错。但其实这种方法并未用到旋转数组的性质，只是单纯的当作一个部分有序数组来解决问题。时间复杂度为`O(n)`但实际上，旋转递增数组的定义决定了其最小值的左侧的值一定大于右侧的值，故可以使用二分法解决，时间复杂度为`O(log n)`。代码如下：

```go
func minArray(numbers []int) int {
	low := 0
	high := len(numbers) - 1
	for low < high {
		pivot := low + (high - low) / 2
        if numbers[pivot] < numbers[high] {
            high = pivot
        } else if numbers[pivot] > numbers[high] {
            low = pivot + 1
        } else {
            high--
        }
	}
	return numbers[low]
}
```

执行用时: 4 ms

内存消耗: 3.1 MB
