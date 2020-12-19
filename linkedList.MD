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

#### 2. [链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)
解题思路：

其实就是利用快慢指针，后指针每次都比前指针多跑一部，那后指针经过的路程总为前指针的两倍，当后指针到达链表末尾时，前指针自然位于链表的中点;需要注意的是，如果快指针最后为空，那么这个链表的长度是偶数，如果不为空，链表长度为奇数
代码实现也很简单：

代码实现：

```
var middleNode = function(head) {
    let p1 = head
    while(p1 && p1.next) {
        head = head.next
        p1 = p1.next.next
    }
    return head
};
```

#### 3. [回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

解题思路：

思路一：利用数组把两者的顺序、逆序保存起来，然后挨个对比两数组的同一个位置的值，如果都一样，说明就是回文链表，否则不是
思路二： 通过找到链表的中间节点然后把链表后半部分反转，最后再用后半部分反转的链表和前半部分一个个比较即可

思路一代码实现：

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function(head) {
    let temp_stack_1 = []
    let temp_stack_2 = []
    while(head) {
        temp_stack_1.push(head.val)
        head = head.next
    }
    for(let i  = temp_stack_1.length - 1; i >=0 ; i --) {
        temp_stack_2.push(temp_stack_1[i])
    }
    let flag = true
    for(let j = 0; j < temp_stack_1.length; j++) {
        if(temp_stack_1[j] !== temp_stack_2[j]) flag = false
    }
    return flag
};
```
思路二代码实现：
```
var isPalindrome = function(head) {
    let fast = head, slow = head;
    //通过快慢指针找到中点
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }
    //如果fast不为空，说明链表的长度是奇数个
    if (fast != null) {
        slow = slow.next;
    }
    //反转后半部分链表
    slow = reverse(slow);

    fast = head;
    while (slow != null) {
        //然后比较，判断节点值是否相等
        if (fast.val != slow.val)
            return false;
        fast = fast.next;
        slow = slow.next;
    }
    return true;
}

//反转链表
function reverse(head) {
    let prev = null;
    while (head != null) {
        let next = head.next;
        head.next = prev;
        prev = head;
        head = next;
    }
    return prev;
}
```


