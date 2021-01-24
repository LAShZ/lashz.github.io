# LeetCode 剑指Offer 20


<!--more-->

## 题目：表示数值的字符串

[题目链接](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

## 题解

利用有限状态机的思想，只需要判断是否为数字，是否为小数，是否为指数，是否为符号即可。

```go
func isNumber(s string) bool {
    isNum := false	// 是否为数字
	isDec := false	// 是否为小数
    isIndex := false	// 是否为指数
	hasPreNum := false	// 指数前是否有数字
	s = strings.TrimSpace(s)
	s = strings.ToLower(s)

	for i, v := range s {
		if v >= '0' && v <= '9' {
			isNum = true
			if !isIndex {
				hasPreNum = true
			}
		} else if v == '.' && !isDec && !isIndex {
			isDec = true
		} else if v == 'e' && !isIndex && isNum {
			isIndex = true
			isNum = false	// 这里需要将isNum置false，因为指数后面若没有数字的话最后结果也应该为假
		} else if (v == '+' || v == '-') && (i == 0 || s[i-1] == 'e') {

		} else {
			return false
		}
	}
	return isNum && hasPreNum
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.3 MB
