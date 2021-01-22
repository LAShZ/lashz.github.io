# LeetCode 剑指Offer 05

<!--more-->
# LeetCode刷题笔记--剑指Offer 05

## 题目：把数字翻译成字符串

[题目链接](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof)。

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

> 示例


**输入**：

s = "We are happy."

**输出**：

"We%20are%20happy."

**限制**:

0 <= s 的长度 <= 10000

## 题解

直接使用strings下的Replaces接口函数即可。

函数声明为: `func Replace(s, old, new string, n int) string`

功能为: 返回将s中前n个不重叠old子串都替换为new的新字符串，如果n<0会替换所有old子串。

代码：

```go
func replaceSpace(s string) string {
    return strings.Replace(s, " ", "%20", -1)
}
```

执行用时: 0 ms

内存消耗: 1.9 MB
