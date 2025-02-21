## 题目
[题目链接](https://www.nowcoder.com/practice/c087914fae584da886a0091e877f2c79?tpId=295&tqId=664&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息:
- 给定一个从小到大排好序的链表
- 删去链表中重复的元素，每个值只留下一个节点

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM16. 删除有序链表中重复的元素-II](https://www.nowcoder.com/practice/71cef9f8b5564579bf7ed93fbe0b2024?tpId=295&tqId=663)

##### 方法：遍历删除（推荐使用）
**思路：**

既然连续相同的元素只留下一个，我们留下哪一个最好呢？当然是遇到的第一个元素了！

```java
if(cur.val == cur.next.val) 
    cur.next = cur.next.next;
```

因为第一个元素直接就与前面的链表节点连接好了，前面就不用管了，只需要跳过后面重复的元素，连接第一个不重复的元素就可以了，在链表中连接后面的元素总比连接前面的元素更方便嘛，因为不能逆序访问。

**具体做法：**

- step 1：判断链表是否为空链表，空链表不处理直接返回。
- step 2：使用一个指针遍历链表，如果指针当前节点与下一个节点的值相同，我们就跳过下一个节点，当前节点直接连接下个节点的后一位。
- step 3：如果当前节点与下一个节点值不同，继续往后遍历。
- step 4：循环过程中每次用到了两个节点值，要检查连续两个节点是否为空。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220225/397721558_1645758612227/5ACEE4D24C4C24B71428151017FE8C9F)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode deleteDuplicates (ListNode head) {
        //空链表
        if(head == null) 
            return null;
        //遍历指针
        ListNode cur = head; 
        //指针当前和下一位不为空
        while(cur != null && cur.next != null){ 
            //如果当前与下一位相等则忽略下一位
            if(cur.val == cur.next.val) 
                cur.next = cur.next.next;
            //否则指针正常遍历
            else 
                cur = cur.next;
        }
        return head;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //空链表
        if(head == NULL) 
            return NULL;
        //遍历指针
        ListNode* cur = head; 
        //指针当前和下一位不为空
        while(cur != NULL && cur->next != NULL){ 
            //如果当前与下一位相等则忽略下一位
            if(cur->val == cur->next->val) 
                cur->next = cur->next->next;
            //否则指针正常遍历
            else 
                cur = cur->next;
        }
        return head;
    }
};
```
**Python实现代码**
```python
class Solution:
    def deleteDuplicates(self , head: ListNode) -> ListNode:
        #空链表
        if head == None: 
            return None
        #遍历指针
        cur = head 
        #指针当前和下一位不为空
        while cur and cur.next: 
            #如果当前与下一位相等则忽略下一位
            if(cur.val == cur.next.val):  
                cur.next = cur.next.next
            #否则指针正常遍历
            else: 
                cur = cur.next
        return head
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为链表长度，遍历一次链表
- 空间复杂度：$O(1)$，常数级指针变量使用，没有使用额外的辅助空间