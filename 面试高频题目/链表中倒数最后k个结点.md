## 题目
[题目链接](https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=196&tqId=1377477&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 一个长度为$n$的链表，返回原链表中从倒数第k个节点至尾节点的全部节点
- 如果该链表长度小于k，请返回一个长度为 0 的链表

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM4.合并有序链表](https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=html&channel=nowcoder)

[BM5.合并k个已排序的链表](https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&sfm=html&channel=nowcoder)

[BM6.判断链表中是否有环](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=295&sfm=html&channel=nowcoder)

[BM7.链表中环的入口节点](https://www.nowcoder.com/practice/6e630519bf86480296d0f1c868d425ad?tpId=295&sfm=html&channel=nowcoder)

[BM9.删除链表的倒数第n个节点](https://www.nowcoder.com/practice/f95dcdafbde44b22a6d741baf71653f6?tpId=295&sfm=html&channel=nowcoder)

[BM10.两个链表的第一个公共节点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=295&sfm=html&channel=nowcoder)

[BM13.判断一个链表是否为回文结构](https://www.nowcoder.com/practice/3fed228444e740c8be66232ce8b87c2f?tpId=295&sfm=html&channel=nowcoder)

[BM14.链表的奇偶重排](https://www.nowcoder.com/practice/02bf49ea45cd486daa031614f9bd6fc3?tpId=295&sfm=html&channel=nowcoder)

##### 方法一：快慢双指针（推荐使用）

**知识点：双指针**

双指针指的是在遍历对象的过程中，不是普通的使用单个指针进行访问，而是使用两个指针（特殊情况甚至可以多个），两个指针或是同方向访问两个链表、或是同方向访问一个链表（快慢指针）、或是相反方向扫描（对撞指针），从而达到我们需要的目的。

**思路：**

我们无法逆序遍历链表，就很难得到链表的倒数第$k$个元素，那我们可以试试反过来考虑，如果当前我们处于倒数第$k$的位置上，即距离链表尾的距离是$k$，那我们假设双指针指向这两个位置，二者同步向前移动，当前面个指针到了链表头的时候，两个指针之间的距离还是$k$。虽然我们没有办法让指针逆向移动，但是我们刚刚这个思路却可以正向实施。

**具体做法：**

- step 1：准备一个快指针，从链表头开始，在链表上先走$k$步。
- step 2：准备慢指针指向原始链表头，代表当前元素，则慢指针与快指针之间的距离一直都是$k$。
- step 3：快慢指针同步移动，当快指针到达链表尾部的时候，慢指针正好到了倒数$k$个元素的位置。

**图示：**
![alt](https://uploadfiles.nowcoder.com/images/20211001/397721558_1633081607162/15B43339B02E276F1B14C9F3B03A53CF)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode FindKthToTail (ListNode pHead, int k) {
        int n = 0;
        ListNode fast = pHead; 
        ListNode slow = pHead;
        //快指针先行k步
        for(int i = 0; i < k; i++){  
            if(fast != null)
                fast = fast.next;
            //达不到k步说明链表过短，没有倒数k
            else 
                return slow = null;
        }
        //快慢指针同步，快指针先到底，慢指针指向倒数第k个
        while(fast != null){ 
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pHead, int k) {
        ListNode* fast = pHead; 
        ListNode* slow = pHead;
        //快指针先行k步
        for(int i = 0; i < k; i++){  
            if(fast != NULL)
                fast = fast->next;
            //达不到k步说明链表过短，没有倒数k
            else 
                return slow = NULL;
        }
        //快慢指针同步，快指针先到底，慢指针指向倒数第k个
        while(fast != NULL){ 
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```
**Python代码实现：**
```python
class Solution:
    def FindKthToTail(self , pHead , k ):
        fast = pHead
        slow = pHead
        #快指针先行k步
        for i in range(0,k): 
            if fast != None:
                fast = fast.next
            #达不到k步说明链表过短，没有倒数k
            else: 
                return None
        #快慢指针同步，快指针先到底，慢指针指向倒数第k个
        while fast:
            fast = fast.next
            slow = slow.next
        return slow
```
**复杂度分析：**
- 时间复杂度：$O(n)$，总共遍历$n$个链表元素
- 空间复杂度：$O(1)$，常数级指针变量，无额外辅助空间使用

##### 方法二：先找长度再找最后k（扩展思路）
**思路：**

链表不能逆向遍历，也不能直接访问。但是对于倒数第$k$个位置，我们只需要知道是正数多少位还是可以直接遍历得到的。

**具体做法：**

- step 1：可以先遍历一次链表找到链表的长度。
- step 2：然后比较链表长度是否比$k$小，如果比$k$小返回一个空节点。
- step 3：如果链表足够长，则我们从头节点往后遍历$n-k$次即可找到所求。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode FindKthToTail (ListNode pHead, int k) {
        int n = 0;
        ListNode p = pHead;
         //遍历链表，统计链表长度
        while(p != null){
            n++;
            p = p.next;
        }
        //长度过小，返回空链表
        if(n < k) 
            return null;
        p = pHead;
        //遍历n-k次
        for(int i = 0; i < n - k; i++) 
            p = p.next;
        return p;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pHead, int k) {
        int n = 0;
        ListNode* p = pHead;
        //统计链表长度
        while(p != NULL){ 
            n++;
            p = p->next;
        }
        //长度过小，返回空链表
        if(n < k) 
            return NULL;
        p = pHead;
        //遍历n-k次
        for(int i = 0; i < n - k; i++) 
            p = p->next;
        return p;
    }
};
```
**Python代码实现：**
```python
class Solution:
    def FindKthToTail(self , pHead , k ):
        n = 0
        p = pHead
        #统计链表长度
        while p: 
            n = n + 1
            p = p.next
        #长度过小，返回空链表
        if n < k: 
            return None
        p = pHead
        #遍历n-k次
        for i in range(n-k): 
            p = p.next
        return p
```
**复杂度分析：**
- 时间复杂度：$O(n)$，最坏情况下两次遍历链表$n$个元素
- 空间复杂度：$O(1)$，常数级指针变量，无额外辅助空间使用