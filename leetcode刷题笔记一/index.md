# LeetCode 剑指Offer 03

<!--more-->
## 题目：数组中重复的数字

[找出数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

> 示例 1：

**输入**：

[2, 3, 1, 0, 2, 5, 3]

**输出**：

2 或 3 

**限制**：

2 <= n <= 100000

## 题解

本题为基础题，难度不高，看到题目即可想到最为简单的暴力哈希解法，不过其实还存在更优解法，下面给予讨论。

### 哈希表

直接使用一个map存放数字是否出现过，遍历整个slice，若数字对应的value为true则返回该数字，否则将其value置为true。

```go
func findRepeatNumber(nums []int) int {
    stat := make(map[int] bool)
    for _, val := range nums {
        if(stat[val]){
            return val
        }else{
            stat[val] = true
        }
    }
    return -1
}
```

执行用时: 48 ms

内存消耗: 8.9 MB

### 改进哈希法

由于数组的长度为n，而所有数字都在 0～n-1 的范围内，所以只需要构建一个大小为n的数组temp，nums内的值就是temp的下标，每次遍历到都将其对应的值加一，当该下标对应的值大于1的时候返回该下标。

```go
func findRepeatNumber(nums []int) int {
    n := len(nums)
    temp := make([]int, n)
    for i := 0; i<n;i++{
        temp[nums[i]]++
        if temp[nums[i]] > 1{
            return nums[i]
        }
    }
    return -1
}
```

执行用时: 36 ms

内存消耗: 8 MB

### 排序+遍历

由于只需要找到重复数字，所以可以将nums内的元素排序后对其遍历，将当前数字与下一个数字进行比较，若相等则找到了重复数字，直接返回该值。

```go
func findRepeatNumber(nums []int) int {
    sort.Ints(nums)
    for sub, val := range nums{
        if(val == nums[sub + 1]){
            return val
        }
    }
    return -1
}
```

执行用时: 56 ms

内存消耗: 8.7 MB
