## 题目
[题目链接](https://www.nowcoder.com/practice/71cef9f8b5564579bf7ed93fbe0b2024?tpId=295&tqId=663&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 在一个非降序的链表中，存在重复的节点，删除该链表中重复的节点
- 重复的节点一个元素也不保留

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM15.删除有序链表中重复的元素-I](https://www.nowcoder.com/practice/c087914fae584da886a0091e877f2c79?tpId=295&sfm=html&channel=nowcoder)

##### 方法一：直接比较删除（推荐使用）
**思路：**

这是一个升序链表，重复的节点都连在一起，我们就可以很轻易地比较到重复的节点，然后将所有的连续相同的节点都跳过，连接不相同的第一个节点。

```java
//遇到相邻两个节点值相同
if(cur.next.val == cur.next.next.val){ 
    int temp = cur.next.val;
    //将所有相同的都跳过
    while (cur.next != null && cur.next.val == temp) 
        cur.next = cur.next.next;
}
```

**具体做法：**

- step 1：给链表前加上表头，方便可能的话删除第一个节点。
```java
ListNode res = new ListNode(0);
//在链表前加一个表头
res.next = head; 
```
- step 2：遍历链表，每次比较相邻两个节点，如果遇到了两个相邻节点相同，则新开内循环将这一段所有的相同都遍历过去。
- step 3：在step 2中这一连串相同的节点前的节点直接连上后续第一个不相同值的节点。
- step 4：返回时去掉添加的表头。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20211202/397721558_1638444061470/0A01E83A481A4919FAE203E7BB77FDD3)


**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode deleteDuplicates (ListNode head) {
        //空链表
        if(head == null) 
            return null;
        ListNode res = new ListNode(0);
        //在链表前加一个表头
        res.next = head; 
        ListNode cur = res;
        while(cur.next != null && cur.next.next != null){ 
            //遇到相邻两个节点值相同
            if(cur.next.val == cur.next.next.val){ 
                int temp = cur.next.val;
                //将所有相同的都跳过
                while (cur.next != null && cur.next.val == temp) 
                    cur.next = cur.next.next;
            }
            else 
                cur = cur.next;
        }
        //返回时去掉表头
        return res.next; 
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
        ListNode* res = new ListNode(0);
        //在链表前加一个表头
        res->next = head; 
        ListNode* cur = res;
        while(cur->next != NULL && cur->next->next != NULL){ 
            //遇到相邻两个节点值相同
            if(cur->next->val == cur->next->next->val){ 
                int temp = cur->next->val;
                //将所有相同的都跳过
                while (cur->next != NULL && cur->next->val == temp) 
                    cur->next = cur->next->next;
            }
            else 
                cur = cur->next;
        }
        //返回时去掉表头
        return res->next; 
    }
};
```
**Phthon实现代码：**
```python
class Solution:
    def deleteDuplicates(self , head: ListNode) -> ListNode:
        #空链表
        if head == None: 
            return None
        res = ListNode(0)
        #在链表前加一个表头
        res.next = head 
        cur = res
        while cur.next and cur.next.next: 
            #遇到相邻两个节点值相同
            if cur.next.val == cur.next.next.val: 
                temp = cur.next.val
                #将所有相同的都跳过
                while cur.next != None and cur.next.val == temp: 
                    cur.next = cur.next.next
            else:
                cur = cur.next
        #返回时去掉表头
        return res.next 


```

**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为链表节点数，只有一次遍历
- 空间复杂度：$O(1)$，只开辟了临时指针，常数级空间

##### 方法二：哈希表（扩展思路）
**知识点：哈希表**

哈希表是一种根据关键码（key）直接访问值（value）的一种数据结构。而这种直接访问意味着只要知道key就能在$O(1)$时间内得到value，因此哈希表常用来统计频率、快速检验某个元素是否出现过等。

**思路：**

这道题幸运的是链表有序，我们可以直接与旁边的元素比较，然后删除重复。那我们扩展一点，万一遇到的链表无序呢？我们这里给出一种通用的解法，有序无序都可以使用，即利用哈希表来统计是否重复。

**具体做法：**

- step 1：遍历一次链表用哈希表记录每个节点值出现的次数。
- step 2：在链表前加一个节点值为0的表头，方便可能的话删除表头元素。
- step 3：再次遍历该链表，对于每个节点值检查哈希表中的计数，只留下计数为1的，其他情况都删除。
- step 4：返回时去掉增加的表头。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode deleteDuplicates (ListNode head) {
        //空链表
        if(head == null) 
            return null;
        Map<Integer,Integer> mp = new HashMap<>();
        ListNode cur = head;
        //遍历链表统计每个节点值出现的次数
        while(cur != null){ 
            if(mp.containsKey(cur.val))
                mp.put(cur.val, (int)mp.get(cur.val) + 1);
            else
                mp.put(cur.val,1);
            cur = cur.next;
        }
        ListNode res = new ListNode(0);
        //在链表前加一个表头
        res.next = head; 
        cur = res;
        //再次遍历链表
        while(cur.next != null){
            //如果节点值计数不为1 
            if(mp.get(cur.next.val) != 1) 
                //删去该节点
                cur.next = cur.next.next; 
            else
                cur = cur.next; 
        }
        //去掉表头
        return res.next; 
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
        unordered_map<int, int> mp;
        ListNode* cur = head;
        //遍历链表统计每个节点值出现的次数
        while(cur != NULL){ 
            mp[cur->val]++;
            cur = cur->next;
        }
        ListNode* res = new ListNode(0);
        //在链表前加一个表头
        res->next = head; 
        cur = res;
        //再次遍历链表
        while(cur->next != NULL){ 
            //如果节点值计数不为1
            if(mp[cur->next->val] != 1) 
                //删去该节点
                cur->next = cur->next->next; 
            else
                cur = cur->next; 
        }
        //去掉表头
        return res->next; 
    }
};
```

**Python实现代码：**
```python
class Solution:
    def deleteDuplicates(self , head: ListNode) -> ListNode:
        #空链表
        if head == None: 
            return None
        mp = dict()
        cur = head
        #遍历链表统计每个节点值出现的次数
        while cur: 
            if cur.val in mp:
                mp[cur.val] += 1
            else:
                mp[cur.val] = 1
            cur = cur.next
        res = ListNode(0)
        #在链表前加一个表头
        res.next = head 
        cur = res
        #再次遍历链表
        while cur.next: 
            #如果节点值计数不为1
            if mp[cur.next.val] != 1: 
                #删去该节点
                cur.next = cur.next.next 
            else:
                cur = cur.next
        #去掉表头
        return res.next 
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为链表节点数，一共两次遍历，哈希表每次计数、每次查询都是$O(1)$
- 空间复杂度：$O(n)$，最坏情况下$n$个节点都不相同，哈希表长度为$n$