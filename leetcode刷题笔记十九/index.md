# LeetCode 剑指Offer 21


<!--more-->

## 题目：调整数组顺序使奇数位于偶数前面

[题目链接](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

> 示例1

**输入**：

nums = [1,2,3,4]

**输出**：

[1,3,2,4]

**注**: 

[3,1,2,4] 也是正确的答案之一。

> 说明

1. `1 <= nums.length <= 50000`
2. `1 <= nums[i] <= 10000`

## 题解

本题类似排序算法，不过由于只需要区分奇偶数，只需要记录一个未划分的起始位置`head`和结束位置`tail`，然后两头朝中间遍历，遇到一奇偶相反的数就交换，直至遍历完成。可以使用**位操作**代替取余判断奇偶数简化计算。

```go
func exchange(nums []int) []int {
	l := len(nums)
	if l == 0 {
		return nums
	}

	head := 0
	tail := l - 1
	for {
		for head < tail && nums[head]&1 == 1 {	// 找到正数第一个偶数
			head++
		}
		for head < tail && nums[tail]&1 == 0 {	// 找到倒数第一个奇数
			tail--
		}
		if(head >= tail) {	// 遍历完成
			break
		}
		nums[head], nums[tail] = nums[tail], nums[head]	// 交换错序的奇偶数

	}
	return nums
}
```

> 执行时间: 20 ms
>
> 内存消耗: 6.3 MB
