## 题目
[题目链接](https://www.nowcoder.com/practice/b49c3dc907814e9bbfa8437c251b028e?tpId=196&tqId=722&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息:
- 给定一个链表，从头开始每k个作为一组，将每组的链表节点翻转
- 组与组之间的位置不变
- 如果最后链表末尾剩余不足k个元素，则不翻转，直接放在最后

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM1.反转链表](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=295&sfm=html&channel=nowcoder)

[BM2.链表内指定区间反转](https://www.nowcoder.com/practice/b58434e200a648c589ca2063f1faf58c?tpId=295&sfm=html&channel=nowcoder)


##### 方法：递归（推荐使用）
**思路：**

现在我们想一想，如果拿到一个链表，想要像上述一样分组翻转应该做些什么？首先肯定是分段吧，至少我们要先分成一组一组，才能够在组内翻转，之后就是组内翻转，最后是将反转后的分组连接。

但是连接的时候遇到问题了：首先如果能够翻转，链表第一个元素一定是第一组，它翻转之后就跑到后面去了，而第一组的末尾元素才是新的链表首，我们要返回的也是这个元素，而原本的链表首要连接下一组翻转后的头部，即翻转前的尾部，如果不建立新的链表，看起来就会非常难。但是如果我们从最后的一个组开始翻转，得到了最后一个组的链表首，是不是可以直接连在倒数第二个组翻转后的尾（即翻转前的头）后面，这样从后往前是不是看起来就容易多了。

怎样从后往前呢？我们这时候可以用到自上而下再自下而上的递归或者说栈。接下来我们说说为什么能用递归？如果这个链表有$n$个分组可以反转，我们首先对第一个分组反转，那么是不是接下来将剩余$n-1$个分组反转后的结果接在第一组后面就行了，那这剩余的$n-1$组就是一个子问题。我们来看看递归的三段式模版：

- **终止条件：** 当进行到最后一个分组，即不足k次遍历到链表尾（0次也算），就将剩余的部分直接返回。
- **返回值：** 每一级要返回的就是翻转后的这一分组的头，以及连接好它后面所有翻转好的分组链表。
- **本级任务：** 对于每个子问题，先遍历k次，找到该组结尾在哪里，然后从这一组开头遍历到结尾，依次翻转，结尾就可以作为下一个分组的开头，而先前指向开头的元素已经跑到了这一分组的最后，可以用它来连接它后面的子问题，即后面分组的头。

**具体做法：**
- step 1：每次从进入函数的头节点优先遍历链表k次，分出一组，若是后续不足k个节点，不用反转直接返回头。
- step 2：从进入函数的头节点开始，依次反转接下来的一组链表，反转过程同[BM1.反转链表](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=295&sfm=html&channel=nowcoder)。
- step 3：这一组经过反转后，原来的头变成了尾，后面接下一组的反转结果，下一组采用上述递归继续。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220330/397721558_1648627803280/D024AA6BA7A670402678A9ACAD54EB10)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode reverseKGroup (ListNode head, int k) {
        //找到每次翻转的尾部
        ListNode tail = head;
        //遍历k次到尾部 
        for(int i = 0; i < k; i++){ 
            //如果不足k到了链表尾，直接返回，不翻转
            if(tail == null) 
                return head;
            tail = tail.next; 
        }
        //翻转时需要的前序和当前节点
        ListNode pre = null; 
        ListNode cur = head;
        //在到达当前段尾节点前
        while(cur != tail){ 
            //翻转
            ListNode temp = cur.next; 
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        //当前尾指向下一段要翻转的链表
        head.next = reverseKGroup(tail, k); 
        return pre;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        //找到每次翻转的尾部
        ListNode* tail = head; 
        //遍历k次到尾部
        for(int i = 0; i < k; i++){ 
            //如果不足k到了链表尾，直接返回，不翻转
            if(tail == NULL) 
                return head;
            tail = tail->next; 
        }
        //翻转时需要的前序和当前节点
        ListNode* pre = NULL; 
        ListNode* cur = head;
        //在到达当前段尾节点前
        while(cur != tail){ 
            //翻转
            ListNode* temp = cur->next; 
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        //当前尾指向下一段要翻转的链表
        head->next = reverseKGroup(tail, k); 
        return pre;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def reverseKGroup(self , head: ListNode, k: int) -> ListNode:
        #找到每次翻转的尾部
        tail = head 
        #遍历k次到尾部
        for i in range(0,k): 
            #如果不足k到了链表尾，直接返回，不翻转
            if tail == None: 
                return head
            tail = tail.next
        #翻转时需要的前序和当前节点
        pre = None 
        cur = head
        #在到达当前段尾节点前
        while cur != tail: 
            #翻转
            temp = cur.next 
            cur.next = pre
            pre = cur
            cur = temp
        #当前尾指向下一段要翻转的链表
        head.next = self.reverseKGroup(tail, k) 
        return pre
```
**复杂度分析：**
- 时间复杂度：$O(n)$，一共遍历链表$n$个节点
- 空间复杂度：$O(n)$，递归栈最大深度为$n/k$