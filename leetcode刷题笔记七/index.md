# LeetCode 剑指offer 09

<!--more-->
# LeetCode刷题笔记--剑指Offer 09

## 题目：用两个栈实现队列

[题目链接](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)。

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数`appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

> 示例1

**输入**：

["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]


**输出**：

[null,null,3,-1]

> 示例2

**输入**：

["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]


**输出**：

[null,-1,null,null,5,2]

**限制**:

- 1 <= values <= 10000
- 最多会对 appendTail、deleteHead 进行 10000 次调用

## 题解

本题比较经典，使用两个栈实现队列。

维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。过程如图所示：

![image](https://cdn.jsdelivr.net/gh/LAShZ/blog-pic-repo@main/img/%E5%89%91%E6%8C%87Offer09-1.png)

Go语言没有单独的栈的数据结构，所以可以选择数组或是链表的形式进行存储。

### 数组形式

使用两个栈`stack1`, `stack2`。添加元素时，将元素加入`stack1`栈顶。取元素时，首先判断`stack2`是否为空，若为空则将 `stack1` 里的所有元素弹出插入到 `stack2` 里，如果 `stack2` 仍为空，则返回 `-1`，否则从 `stack2` 弹出一个元素并返回。

```go
type CQueue struct {
    stack1, stack2 []int
}

func Constructor() CQueue {
    return CQueue{}
}

func (this *CQueue) AppendTail(value int)  {
    this.stack1 = append(this.stack1, value)
}

func (this *CQueue) DeleteHead() int {
    if len(this.stack2) == 0 {
        if len(this.stack1) == 0 {
            return -1
        }
        index := len(this.stack1) - 1
        for ;index >= 0;index-- {
            this.stack2 = append(this.stack2, this.stack1[index])
            this.stack1 = this.stack1[:index]
        }
    }
    index := len(this.stack2) - 1
    value := this.stack2[index]
    this.stack2 = this.stack2[:index]
    return value
}

/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```

执行用时: 236 ms

内存消耗: 8.3 MB


### 链表

简单的建立链表，记录头尾节点。

```go
type node struct {
    val int
    next *node
}

type CQueue struct {
    head *node
    tail *node
}


func Constructor() CQueue {
    return CQueue{}
}


func (this *CQueue) AppendTail(value int)  {
    if this.head == nil {
        this.head = &node{val:value}
        this.tail = this.head
    } else {
        this.tail.next = &node{val:value}
        this.tail = this.tail.next
    }
}


func (this *CQueue) DeleteHead() int {
    val := -1
    if this.head != nil {
        val = this.head.val
        this.head = this.head.next
    }
    if this.head == nil {
        this.tail = nil
    }
    return val
}


/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```

执行用时: 232 ms

内存消耗: 8.2 MB
