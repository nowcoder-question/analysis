## 题目
[题目链接](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=295&tqId=23263&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息：

- 对于一个给定无序数组，返回最小的k个元素，顺序任意
- k和数组有特殊情况需要单独讨论

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM48. 数据流中的中位数](https://www.nowcoder.com/practice/9be0172896bd43948f8a32fb954e1be1?tpId=295&tqId=23457)

[BM5. 合并k个有序链表](https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&tqId=724)

##### 方法一：堆排序（推荐使用）

**知识点：优先队列**

优先队列即PriorityQueue，是一种内置的机遇堆排序的容器，分为大顶堆与小顶堆，大顶堆的堆顶为最大元素，其余更小的元素在堆下方，小顶堆与其刚好相反。且因为容器内部的次序基于堆排序，因此每次插入元素时间复杂度都是$O(log_2n)$，而每次取出堆顶元素都是直接取出。

**思路：**

要找到最小的k个元素，只需要准备k个数字，之后每次遇到一个数字能够快速的与这k个数字中最大的值比较，每次将最大的值替换掉，那么最后剩余的就是k个最小的数字了。

如何快速比较k个数字的最大值，并每次替换成较小的新数字呢？我们可以考虑使用优先队列（大根堆），只要限制堆的大小为k，那么堆顶就是k个数字的中最大值，如果需要替换，将这个最大值拿出，加入新的元素就好了。

```java
//较小元素入堆
if(q.peek() > input[i]){  
    q.poll();
    q.offer(input[i]);
}
```

**具体做法：**

- step 1：利用input数组中前k个元素，构建一个大小为k的大顶堆，堆顶为这k个元素的最大值。
- step 2：对于后续的元素，依次比较其与堆顶的大小，若是比堆顶小，则堆顶弹出，再将新数加入堆中，直至数组结束，保证堆中的k个最小。
- step 3：最后将堆顶依次弹出即是最小的k个数。

**图示：**

![图片说明](https://uploadfiles.nowcoder.com/images/20210722/397721558_1626945012109/6A105C4B5BE11C9FE59934C5B4E772BF "图片标题") 

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        //排除特殊情况
        if(k == 0 || input.length == 0) 
            return res;
        //大根堆 
        PriorityQueue<Integer> q = new PriorityQueue<>((o1, o2)->o2.compareTo(o1));
        //构建一个k个大小的堆  
        for(int i = 0; i < k; i++)
            q.offer(input[i]);
        for(int i = k; i < input.length; i++){
            //较小元素入堆
            if(q.peek() > input[i]){  
                q.poll();
                q.offer(input[i]);
            }
        }
        //堆中元素取出入数组
        for(int i = 0; i < k; i++) 
            res.add(q.poll());
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        //排除特殊情况
        if(k == 0 || input.size() == 0) 
            return res;
        priority_queue<int> q; 
        //构建一个k个大小的堆 
        for(int i = 0; i < k; i++)
            q.push(input[i]);
        for(int i = k; i < input.size(); i++){
            //较小元素入堆
            if(q.top() > input[i]){  
                q.pop();
                q.push(input[i]);
            }
        }
        //堆中元素取出入vector
        for(int i = 0; i < k; i++){ 
            res.push_back(q.top());
            q.pop();
        }
        return res;
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def GetLeastNumbers_Solution(self , input: List[int], k: int) -> List[int]:
        res = []
        if len(input) >= k and k != 0:
            import heapq
            #小根堆，每次输入要乘-1 
            pq = []  
            for i in range(k):
                #构建一个k个大小的堆
                heapq.heappush(pq, (-1 * input[i])) 
            for i in range(k, len(input)):
                #较小元素入堆
                if (-1 * pq[0]) > input[i]:   
                     heapq.heapreplace(pq, (-1 * input[i]))
            #堆中元素取出入数组
            for i in range(k): 
                res.append(-1 * pq[0])
                heapq.heappop(pq)
        return res
```

**复杂度分析：**
- 时间复杂度：$O(nlog_2k)$，构建和维护大小为$k$的堆，需要$log_2k$，加上遍历整个数组
- 空间复杂度：$O(k)$，堆空间为$k$个元素

##### 方法二：sort排序法（扩展思路）
**思路：**

当然，如果这个数组本来就是有序的（递增序），那最小的k个数字，是不是就是数组前k个呢？那我们只要对整个数组进行了一次排序，那最小的k个元素不就手到擒来了。

**具体做法：**

- step 1：优先判断k为0或者输入数组长度为0的特殊情况。
- step 2：使用sort函数对整个数组排序。
- step 3：遍历排序后的数组前k个元素即可获取最小的k个。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        //排除特殊情况
        if(k == 0 || input.length == 0) 
            return res;
        //排序
        Arrays.sort(input); 
        //因为k<=input.length,取前k小
        for(int i = 0; i < k; i++){ 
            res.add(input[i]);
        }
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        //排除特殊情况
        if(k == 0 || input.size() == 0) 
            return res;
        //排序
        sort(input.begin(), input.end()); 
        //因为k<=input.size(),取前k小
        for(int i = 0; i < k; i++){ 
            res.push_back(input[i]);
        }
        return res;
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def GetLeastNumbers_Solution(self , input: List[int], k: int) -> List[int]:
        list=[]
        #排除特殊情况
        if k == 0 or len(input) == 0:
            return list
        else:
            #排序
            input.sort()
            #输出前k小
            return input[:k]
```

**复杂度分析：**
- 时间复杂度：$O(nlog_2n)$，sort函数属于优化后的快速排序，复杂度为$O(nlog_2n)$
- 空间复杂度：$O(1)$，无额外辅助空间使用