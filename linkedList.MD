#### 1. [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

解题思路：
- 定义三个指针： cur , p1 p2, cur与p1为了完成指针逆指， p2保存下一个应当逆转的节点，防止迷路
- 每次让 p1 的next 指向 cur，实现一次局部反转
- 局部反转完成之后，cur p1和 p2 同时往前移动一个位置
- 循环上述过程，直至 p1到达链表尾部

思路图解：

![](https://github.com/kejianfeng/algorithm-note/blob/main/images/link-reverse.gif)

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

#### 4. [移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

解题思路：

这道题的难点是处理首节点为目标删除节点的情况，所以需要设置个空的首节点。然后开始遍历，当下一个的值等于目标值，使当前节点指针的next等于next.next, 记住，这时候当前节点并没有前进一步，只有前节点指针的next.val 不等于目标值，当前节点才前进一位。

代码实现：
```
var removeElements = function(head, val) {
    let newNode = new ListNode(0)
    newNode.next = head
    let headNode  = newNode
    while(newNode.next) {
        if(newNode.next.val === val) {
            newNode.next = newNode.next.next
        } else {
            newNode = newNode.next
        }
    }
    return headNode.next

};
```

#### 5.[环形链表](https://leetcode-cn.com/problems/linked-list-cycle)

解题思路：

如果链表是一个环，那下一个永远没有尽头，怎么知道是在绕圈遍历呢？这个时候就可以用上双指针了！准确来说是快慢双指针
何为快慢双指针？例如初始化设置两个指针，p,q均指向head,每次p前进一个节点，而q每次前进两个节点。q比p走得快，如果是在绕圈，q肯定会追上p,这时我们只需要p与q有没有相等得时刻就知道该链表有没有环

代码实现：

```
var hasCycle = function(head) {
    if(!head) return false;
    let low = head
    let high = head.next ? head.next.next : head.next
    while(low && high) {
       if (low === high) {
            return true
       } else {
           low = low.next
           high = high.next ? high.next.next : high.next
       }
    }
    return false
    
};

```

#### 6. [删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

解题思路：

双指针大法 + 哨兵 , 设预先指针(哨兵) flag.next指向 head，设前指针为 p1，后指针为 p2，二者都等于 flag
p1先向前移动n步，之后 p1 和 p2共同向前移动，此时二者的距离为 n，当 p1.next = null时，p2的位置恰好为倒数第 n+1, p2.next=p2.next.next，从而删除倒数第n个节点
删除后返回 flag.next，有人说为什么不直接返回 head？因为 head 有可能是被删掉的点鸭，例如长度只有1时，删除倒数第一个，head就不复存在了;

注意，可能移动相隔了几个有点抽象，自己画下就知道了，一图胜千言：

![](https://github.com/kejianfeng/algorithm-note/blob/main/images/linked-pic-qwda.jpg)

代码实现：
```
var removeNthFromEnd = function(head, n) {
    let flag = new ListNode(0)
    flag.next = head
    let p1 = p2 = flag
    for(let i = 0; i < n; i++) {
        p1 = p1.next
    }
    while(p1.next) {
        p1 = p1.next
        p2 = p2.next
    }
    p2.next = p2.next.next
    return flag.next
};
```

#### 7.[相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists)

解题思路：

如果两个链表相交，那么相交点之后的长度是相同的。
指针 pA 指向 A 链表，指针 pB 指向 B 链表，依次往后遍历
如果 pA 到了末尾，则 pA = headB 继续遍历
如果 pB 到了末尾，则 pB = headA 继续遍历
比较长的链表指针指向较短链表head时，长度差就消除了
如此，只需要将最短链表遍历两次即可找到位置


```
var getIntersectionNode = function(headA, headB) {
    let curA = headA;
    let curB = headB
    while(curA !== curB) {
        curA = curA ? curA.next : headB
        curB = curB ? curB.next : headA
    }
    return curA
};
```

#### 8. [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists)

解题思路：

思路一：非递归实现，构造个新节点，next指向两链表首节点较小的一一个，然后开始遍历下去，边比较边next; 需要注意的是要用多一个哨兵节点保存好新链表首节点的位置，防止迷路
思路二：递归实现
终止条件：两条链表分别名为 l1 和 l2，当 l1 为空或 l2 为空时结束
返回值：每一层调用都返回排序好的链表头
本级递归内容：如果 l1 的 val 值更小，则将 l1.next 与排序好的链表头相接，l2 同理
O(m+n)O(m+n)，mm 为 l1的长度，nn 为 l2 的长度


思路一代码实现：

```
var mergeTwoLists = function(l1, l2) {
   if(!l1) {
       return l2
   }
   if(!l2) {
       return l1
   }
  let p = new ListNode(0)
  let flag = p  //哨兵节点
  while(l1 && l2) {
      if(l1.val <= l2.val) {
          p.next = l1
          l1 = l1.next
      }else {
          p.next = l2
          l2 = l2.next
      }
      p = p.next
  }
  if(l1) {
      p.next = l1
  }
  if(l2) {
      p.next = l2
  }
  return flag.next
}
```

思路二代码实现：

```
var mergeTwoLists = function(l1, l2) {
    if(l1 === null){
        return l2;
    }
    if(l2 === null){
        return l1;
    }
    if(l1.val < l2.val){
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }else{
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
```




