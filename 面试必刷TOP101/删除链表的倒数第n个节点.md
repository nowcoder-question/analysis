## 题目
[题目链接](https://www.nowcoder.com/practice/f95dcdafbde44b22a6d741baf71653f6?tpId=295&tqId=727&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 给定一个链表，要删除链表倒数第n个节点，并返回链表的头
- 题目保证链表长度一定大于等于$n$

##### 举一反三：
学习完本题的思路你可以解决如下题目：

[BM4.合并有序链表](https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=html&channel=nowcoder)

[BM5.合并k个已排序的链表](https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&sfm=html&channel=nowcoder)

[BM6.判断链表中是否有环](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=295&sfm=html&channel=nowcoder)

[BM7.链表中环的入口节点](https://www.nowcoder.com/practice/6e630519bf86480296d0f1c868d425ad?tpId=295&sfm=html&channel=nowcoder)

[BM8.链表中倒数最后k个节点](https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=295&sfm=html&channel=nowcoder)

[BM10.两个链表的第一个公共节点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=295&sfm=html&channel=nowcoder)

[BM13.判断一个链表是否为回文结构](https://www.nowcoder.com/practice/3fed228444e740c8be66232ce8b87c2f?tpId=295&sfm=html&channel=nowcoder)

[BM14.链表的奇偶重排](https://www.nowcoder.com/practice/02bf49ea45cd486daa031614f9bd6fc3?tpId=295&sfm=html&channel=nowcoder)

##### 方法一：双指针（推荐使用）
**知识点：双指针**

双指针指的是在遍历对象的过程中，不是普通的使用单个指针进行访问，而是使用两个指针（特殊情况甚至可以多个），两个指针或是同方向访问两个链表、或是同方向访问一个链表（快慢指针）、或是相反方向扫描（对撞指针），从而达到我们需要的目的。

**思路：**
我们无法逆序遍历链表，就很难得到链表的倒数第$n$个元素，那我们可以试试反过来考虑，如果当前我们处于倒数第$n$的位置上，即距离链表尾的距离是$n$，那我们假设双指针指向这两个位置，二者同步向前移动，当前面个指针到了链表头的时候，两个指针之间的距离还是$n$。虽然我们没有办法让指针逆向移动，但是我们刚刚这个思路却可以正向实施。

**具体做法：**

- step 1：给链表添加一个表头，处理删掉第一个元素时比较方便。
- step 2：准备一个快指针，在链表上先走$n$步。
- step 3：准备慢指针指向原始链表头，代表当前元素，前序节点指向添加的表头，这样两个指针之间相距就是一直都是$n$。
- step 4：快慢指针同步移动，当快指针到达链表尾部的时候，慢指针正好到了倒数$n$个元素的位置。
- step 5：最后将该节点前序节点的指针指向该节点后一个节点，删掉这个节点。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220224/397721558_1645685535667/6F9E6DE9C2A84A7BD8A7655272E366AD)
**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode removeNthFromEnd (ListNode head, int n) {
        //添加表头
        ListNode res = new ListNode(-1); 
        res.next = head;
        //当前节点
        ListNode cur = head; 
        //前序节点
        ListNode pre = res; 
        ListNode fast = head;
        //快指针先行n步
        while(n != 0){ 
            fast = fast.next;
            n--;
        }
        //快慢指针同步，快指针到达末尾，慢指针就到了倒数第n个位置
        while(fast != null){
            fast = fast.next;
            pre = cur;
            cur = cur.next;
        }
        //删除该位置的节点
        pre.next = cur.next; 
        //返回去掉头
        return res.next; 
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //添加表头
        ListNode* res = new ListNode(-1); 
        res->next = head;
        //当前节点
        ListNode* cur = head;
        //前序节点 
        ListNode* pre = res; 
        ListNode* fast = head;
        //快指针先行n步
        while(n--) 
            fast = fast->next;
        //快慢指针同步，快指针到达末尾，慢指针就到了倒数第n个位置
        while(fast != NULL){ 
            fast = fast->next;
            pre = cur;
            cur = cur->next;
        }
        //删除该位置的节点
        pre->next = cur->next; 
        //返回去掉头
        return res->next; 
    }
};
```

**Python实现代码：**
```python
class Solution:
    def removeNthFromEnd(self , head: ListNode, n: int) -> ListNode:
        #添加表头
        res = ListNode(-1) 
        res.next = head
        #当前节点
        cur = head 
        #前序节点
        pre = res 
        fast = head
        #快指针先行n步
        while n: 
            fast = fast.next
            n = n - 1
        #快慢指针同步，快指针到达末尾，慢指针就到了倒数第n个位置
        while fast: 
            fast = fast.next
            pre = cur
            cur = cur.next
        #删除该位置的节点
        pre.next = cur.next 
        #返回去掉头
        return res.next 
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为链表长度，最坏情况遍历整个链表1次
- 空间复杂度：$O(1)$，常数级指针，无额外辅助空间使用

##### 方法二：长度统计法（思路扩展）

**思路：**

既然要删掉倒数第n个元素，那肯定得先找到这个元素，试想一下，如果是数组我们怎么找这个元素的？肯定是先统计数组长度$L$，然后根据$L-n$得到下标，再根据下标访问。

但是很可惜，链表无法直接统计长度，也无法根据下标直接访问，我们能做的就是遍历链表。既然如此，那就两次遍历，第一次统计长度：
```java
while(cur != null)
    length++
```
第二次找到倒数第n个位置：
```java
for(int i = 0; i < length - n; i++)
```

**具体做法：**

- step 1：给链表添加一个表头，处理删掉第一个元素时比较方便。
- step 2：遍历整个链表，统计链表长度。
- step 3：准备两个指针，一个指向原始链表头，表示当前节点，一个指向我们添加的表头，表示前序节点。两个指针同步遍历$L-n$次找到倒数第$n$个位置。
- step 4：前序指针的next指向当前节点的next，代表越过当前节点连接，即删去该节点。
- step 5：返回去掉添加的表头。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode removeNthFromEnd (ListNode head, int n) {
        //记录链表长度
        int length = 0; 
        //添加表头
        ListNode res = new ListNode(-1); 
        res.next = head;
        //当前节点
        ListNode cur = head; 
        //前序节点
        ListNode pre = res; 
        //找到链表长度
        while(cur != null){ 
            length++;
            cur = cur.next;
        }
        //回到头部
        cur = head; 
        //从头遍历找到倒数第n个位置
        for(int i = 0; i < length - n; i++){ 
            pre = cur;
            cur = cur.next;
        }
        //删去倒数第n个节点
        pre.next = cur.next; 
        //返回去掉头节点
        return res.next; 
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //记录链表长度
        int length = 0; 
        //添加表头
        ListNode* res = new ListNode(-1); 
        res->next = head;
        //当前节点
        ListNode* cur = head; 
        //前序节点
        ListNode* pre = res; 
        //找到链表长度
        while(cur != NULL){ 
            length++;
            cur = cur->next;
        }
        //回到头部
        cur = head; 
        //从头遍历找到倒数第n个位置
        for(int i = 0; i < length - n; i++){ 
            pre = cur;
            cur = cur->next;
        }
        //删去倒数第n个节点
        pre->next = cur->next; 
        //返回去掉头节点
        return res->next; 
    }
};
```
**Python实现代码：**
```python
class Solution:
    def removeNthFromEnd(self , head: ListNode, n: int) -> ListNode:
        #记录链表长度
        length = 0 
        #添加表头
        res = ListNode(-1) 
        res.next = head
        #当前节点
        cur = head 
        #前序节点
        pre = res 
        #找到链表长度
        while cur != None: 
            length += 1
            cur = cur.next
        #回到头部
        cur = head 
        #从头遍历找到倒数第n个位置
        for i in range(length - n): 
            pre = cur
            cur = cur.next
        #删去倒数第n个节点
        pre.next = cur.next 
        #返回去掉头节点
        return res.next 
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为链表长度，最坏情况遍历整个链表2次
- 空间复杂度：$O(1)$，常数级指针，无额外辅助空间使用