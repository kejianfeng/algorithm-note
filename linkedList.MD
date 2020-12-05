#### 1. [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

解题思路：
- 定义三个指针： cur , p1 p2, cur与p1为了完成指针逆指， p2保存下一个应当逆转的节点，防止迷路
- 每次让 p1 的next 指向 cur，实现一次局部反转
- 局部反转完成之后，cur p1和 p2 同时往前移动一个位置
- 循环上述过程，直至 p1到达链表尾部

思路图解：
![](https://image.jianfengke.com/20201205121132.png)

代码实现：
```
var reverseList = function(head) {
    if(!head) return null
    let cur = null
    let p1 = head
    let p2 = head.next
    while(p1) {
        p1.next = cur
        cur = p1
        p1 = p2
        p2 = p2 ? p2.next : null
    }
    return cur
};
```
