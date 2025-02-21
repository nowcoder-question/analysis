## 题目
[题目链接](https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&tqId=724&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 给定k个排好序的升序链表
- 将这k个链表合并成一个大的升序链表，并返回这个升序链表的头


##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM4.合并有序链表](https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=html&channel=nowcoder)

[BM6.判断链表中是否有环](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=295&sfm=html&channel=nowcoder)

[BM7.链表中环的入口节点](https://www.nowcoder.com/practice/6e630519bf86480296d0f1c868d425ad?tpId=295&sfm=html&channel=nowcoder)

[BM8.链表中倒数最后k个节点](https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=295&sfm=html&channel=nowcoder)

[BM9.删除链表的倒数第n个节点](https://www.nowcoder.com/practice/f95dcdafbde44b22a6d741baf71653f6?tpId=295&sfm=html&channel=nowcoder)

[BM10.两个链表的第一个公共节点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=295&sfm=html&channel=nowcoder)

[BM13.判断一个链表是否为回文结构](https://www.nowcoder.com/practice/3fed228444e740c8be66232ce8b87c2f?tpId=295&sfm=html&channel=nowcoder)

[BM14.链表的奇偶重排](https://www.nowcoder.com/practice/02bf49ea45cd486daa031614f9bd6fc3?tpId=295&sfm=html&channel=nowcoder)


##### 方法一：归并排序思想（推荐使用）

**知识点1：双指针**

双指针指的是在遍历对象的过程中，不是普通的使用单个指针进行访问，而是使用两个指针（特殊情况甚至可以多个），两个指针或是同方向访问两个链表、或是同方向访问一个链表（快慢指针）、或是相反方向扫描（对撞指针），从而达到我们需要的目的。

**知识点2：分治**

分治即“分而治之”，“分”指的是将一个大而复杂的问题划分成多个性质相同但是规模更小的子问题，子问题继续按照这样划分，直到问题可以被轻易解决；“治”指的是将子问题单独进行处理。经过分治后的子问题，需要将解进行合并才能得到原问题的解，因此整个分治过程经常用递归来实现。

**思路：**

如果是[两个有序链表合并](https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=html&channel=nowcoder)，我们可能会利用归并排序合并阶段的思想：准备双指针分别放在两个链表头，每次取出较小的一个元素加入新的大链表，将其指针后移，继续比较，这样我们出去的都是最小的元素，自然就完成了排序。

其实这道题我们也可以两两比较啊，只要遍历链表数组，取出开头的两个链表，按照上述思路合并，然后新链表再与后一个继续合并，如此循环，知道全部合并完成。但是，这样太浪费时间了。

既然都是归并排序的思想了，那我们可不可以直接归并的分治来做，而不是顺序遍历合并链表呢？答案是可以的！

归并排序是什么？简单来说就是将一个数组每次划分成等长的两部分，对两部分进行排序即是子问题。对子问题继续划分，直到子问题只有1个元素。还原的时候呢，将每个子问题和它相邻的另一个子问题利用上述双指针的方式，1个与1个合并成2个，2个与2个合并成4个，因为这每个单独的子问题合并好的都是有序的，直到合并成原本长度的数组。

对于这k个链表，就相当于上述合并阶段的k个子问题，需要划分为链表数量更少的子问题，直到每一组合并时是两两合并，然后继续往上合并，这个过程基于递归：

- **终止条件：** 划分的时候直到左右区间相等或左边大于右边。
- **返回值：** 每级返回已经合并好的子问题链表。
- **本级任务：** 对半划分，将划分后的子问题合并成新的链表。


**具体做法：**
- step 1：从链表数组的首和尾开始，每次划分从中间开始划分，划分成两半，得到左边$n/2$个链表和右边$n/2$个链表。
- step 2：继续不断递归划分，直到每部分链表数为1.
- step 3：将划分好的相邻两部分链表，按照[两个有序链表合并](https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=html&channel=nowcoder)的方式合并，合并好的两部分继续往上合并，直到最终合并成一个链表。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220224/397721558_1645677174826/826CB3016D45D4600C3A358F82ACC1EF)

**Java实现代码：**
```java
import java.util.ArrayList;
public class Solution {
    //两个链表合并函数
    public ListNode Merge(ListNode list1, ListNode list2) { 
        //一个已经为空了，直接返回另一个
        if(list1 == null) 
            return list2;
        if(list2 == null)
            return list1;
        //加一个表头
        ListNode head = new ListNode(0); 
        ListNode cur = head;
        //两个链表都要不为空
        while(list1 != null && list2 != null){ 
            //取较小值的节点
            if(list1.val <= list2.val){ 
                cur.next = list1;
                //只移动取值的指针
                list1 = list1.next; 
            }else{
                cur.next = list2;
                //只移动取值的指针
                list2 = list2.next; 
            }
            //指针后移
            cur = cur.next; 
        }
        //哪个链表还有剩，直接连在后面
        if(list1 != null) 
            cur.next = list1;
        else
            cur.next = list2;
        //返回值去掉表头
        return head.next; 
    }
    
    //划分合并区间函数
    ListNode divideMerge(ArrayList<ListNode> lists, int left, int right){ 
        if(left > right) 
            return null;
        //中间一个的情况
        else if(left == right) 
            return lists.get(left);
        //从中间分成两段，再将合并好的两段合并
        int mid = (left + right) / 2; 
        return Merge(divideMerge(lists, left, mid), divideMerge(lists, mid + 1, right));
    }
    
    public ListNode mergeKLists(ArrayList<ListNode> lists) {
        //k个链表归并排序
        return divideMerge(lists, 0, lists.size() - 1);
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    //两个有序链表合并函数
    ListNode* Merge2(ListNode* pHead1, ListNode* pHead2) { 
        //一个已经为空了，直接返回另一个
        if(pHead1 == NULL) 
            return pHead2;
        if(pHead2 == NULL)
            return pHead1;
        //加一个表头
        ListNode* head = new ListNode(0); 
        ListNode* cur = head;
        //两个链表都要不为空
        while(pHead1 && pHead2){ 
            //取较小值的节点
            if(pHead1->val <= pHead2->val){ 
                cur->next = pHead1;
                //只移动取值的指针
                pHead1 = pHead1->next; 
            }else{
                cur->next = pHead2;
                //只移动取值的指针
                pHead2 = pHead2->next; 
            }
            //指针后移
            cur = cur->next; 
        }
        //哪个链表还有剩，直接连在后面
        if(pHead1) 
            cur->next = pHead1;
        else
            cur->next = pHead2;
        //返回值去掉表头
        return head->next; 
    }
    
    //划分合并区间函数
    ListNode* divideMerge(vector<ListNode *> &lists, int left, int right){ 
        if(left > right) 
            return NULL;
        //中间一个的情况
        else if(left == right) 
            return lists[left];
        //从中间分成两段，再将合并好的两段合并
        int mid = (left + right) / 2; 
        return Merge2(divideMerge(lists, left, mid), divideMerge(lists, mid + 1, right));
    }
    
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        //k个链表归并排序
        return divideMerge(lists, 0, lists.size() - 1); 
    }
};
```
**Python代码实现：**
```python
import sys
#设置递归深度
sys.setrecursionlimit(100000) 

class Solution:
    #两个有序链表合并函数
    def Merge2(self, pHead1: ListNode, pHead2: ListNode) -> ListNode: 
        #一个已经为空了，直接返回另一个
        if pHead1 == None: 
            return pHead2
        if pHead2 == None:
            return pHead1
        #加一个表头
        head = ListNode(0) 
        cur = head
        #两个链表都要不为空
        while pHead1 and pHead2: 
            #取较小值的节点
            if pHead1.val <= pHead2.val: 
                cur.next = pHead1
                #只移动取值的指针
                pHead1 = pHead1.next 
            else:
                cur.next = pHead2
                #只移动取值的指针
                pHead2 = pHead2.next 
            #指针后移
            cur = cur.next 
        #哪个链表还有剩，直接连在后面
        if pHead1: 
            cur.next = pHead1
        else:
            cur.next = pHead2
        #返回值去掉表头
        return head.next 
    
    #划分合并区间函数
    def divideMerge(self, lists: List[ListNode], left: int, right: int) -> ListNode:         
        if left > right :
            return None
        #中间一个的情况
        elif left == right: 
            return lists[left]
        #从中间分成两段，再将合并好的两段合并
        mid = (int)((left + right) / 2) 
        return self.Merge2(self.divideMerge(lists, left, mid), self.divideMerge(lists, mid + 1, right))
    
    def mergeKLists(self , lists: List[ListNode]) -> ListNode:
        #k个链表归并排序
        return self.divideMerge(lists, 0, len(lists) - 1) 
```

**复杂度分析：**
- 时间复杂度：$O(nlog_2k)$，其中$n$为所有链表的总节点数，分治为二叉树型递归，最坏情况下二叉树每层合并都是$O(n)$个节点，因为分治一共有$O(log_2k)$层
- 空间复杂度：$O(log_2k)$，最坏情况下递归$log_2k$层，需要$log_2k$的递归栈

##### 方法二：优先队列（扩展思路）
**知识点：优先队列**

优先队列即PriorityQueue，是一种内置的机遇堆排序的容器，分为大顶堆与小顶堆，大顶堆的堆顶为最大元素，其余更小的元素在堆下方，小顶堆与其刚好相反。且因为容器内部的次序基于堆排序，因此每次插入元素时间复杂度都是$O(log_2n)$，而每次取出堆顶元素都是直接取出。

**思路：**

如果非要按照归并排序的合并思路，双指针不够用，我们可以直接准备$k$个指针，每次比较得出$k$个数字中的最小值。为了快速比较$k$个数字得到最小值，我们可以利用Java提供的PriorityQueue或者C++SLT提供的优先队列或者Python提供的PriorityQueue可以实现，它是一种参照堆排序的容器，容器中的元素是有序的，如果是小顶堆，顶部元素就是最小的，每次可以直接取出最小的元素。也就是说

每次该容器中有k个元素，我们可以直接拿出最小的元素，再插入下一个元素，相当于每次都是链表的k个指针在比较大小，只移动最小元素的指针。

**具体做法：**

- step 1：不管是Java还是C++都需要重载比较方法，构造一个比较链表节点大小的小顶堆。（Python版本直接加入节点值）
- step 2：先遍历k个链表头，将不是空节点的节点加入优先队列。
- step 3：每次依次弹出优先队列中的最小元素，将其连接在合并后的链表后面，然后将这个节点在原本链表中的后一个节点（如果不为空的话）加入队列，类似上述归并排序双指针的过程。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ListNode mergeKLists(ArrayList<ListNode> lists) {
        //小顶堆
        Queue<ListNode> pq = new PriorityQueue<>((v1, v2) -> v1.val - v2.val); 
        //遍历所有链表第一个元素
        for(int i = 0; i < lists.size(); i++){ 
            //不为空则加入小顶堆
            if(lists.get(i) != null) 
                pq.add(lists.get(i));
        }
        //加一个表头
        ListNode res = new ListNode(-1); 
        ListNode head = res;
        //直到小顶堆为空
        while(!pq.isEmpty()){ 
            //取出最小的元素
            ListNode temp = pq.poll(); 
            //连接
            head.next = temp; 
            head = head.next;
            //每次取出链表的后一个元素加入小顶堆
            if(temp.next != null) 
                pq.add(temp.next);
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
    struct cmp {
    //重载小顶堆比较方式
    bool operator()(ListNode* a, ListNode* b) { 
        return  a->val > b->val;  
    }
};
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        //小顶堆
        priority_queue<ListNode*, vector<ListNode*>, cmp> pq; 
        //遍历所有链表第一个元素
        for(int i = 0; i < lists.size(); i++){ 
            //不为空则加入小顶堆
            if(lists[i] != NULL) 
                pq.push(lists[i]);
        }
        //加一个表头
        ListNode* res = new ListNode(-1); 
        ListNode* head = res;
        //直到小顶堆为空
        while(!pq.empty()){ 
            //取出最小的元素
            ListNode* temp = pq.top(); 
            pq.pop();
            //连接
            head->next = temp; 
            head = head->next;
            //每次取出链表的后一个元素加入小顶堆
            if(temp->next != NULL)
                pq.push(temp->next);
        }
        return res->next;
    }
};
```
**Python代码实现：**
```python
from queue import PriorityQueue
class Solution:
    def mergeKLists(self , lists: List[ListNode]) -> ListNode:
        #小顶堆
        pq = PriorityQueue() 
        #遍历所有链表第一个元素
        for i in range(len(lists)): 
            #不为空则加入小顶堆
            if lists[i] != None: 
                pq.put((lists[i].val, i))
                lists[i] = lists[i].next
        #加一个表头
        res = ListNode(-1) 
        head = res
        #直到小顶堆为空
        while not pq.empty():
            #取出最小的元素
            val, idx = pq.get() 
            #连接
            head.next = ListNode(val) 
            head = head.next
            if lists[idx] != None: 
                #每次取出链表的后一个元素加入小顶堆
                pq.put((lists[idx].val, idx))
                lists[idx] = lists[idx].next
        return res.next
```
**复杂度分析：**
- 时间复杂度：$O(nlog_2k)$，其中$n$为所有链表的总节点数，最坏需要遍历所有的节点，每次加入优先队列排序需要$O(log_2k)$
- 空间复杂度：$O(k)$，优先队列的大小不会超过$k$