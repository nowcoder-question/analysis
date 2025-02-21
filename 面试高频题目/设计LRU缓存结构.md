## 题目
[题目链接](https://www.nowcoder.com/practice/5dfded165916435d9defb053c63f1e84?tpId=196&tqId=2427094&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 实现LRU缓存的模拟结构，包括加入函数set，访问函数get
- 结构有长度限制，加入新数时，超出长度则需要删除最不常访问的，其中set与get都访问
- 两个函数都是$O(1)$

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM101. 设计LFU缓存结构](https://www.nowcoder.com/practice/93aacb4a887b46d897b00823f30bfea1?tpId=295&sfm=html&channel=nowcoder)

##### 方法：哈希表+双向链表（推荐使用）

**知识点1：哈希表**

哈希表是一种根据关键码（key）直接访问值（value）的一种数据结构。而这种直接访问意味着只要知道key就能在$O(1)$时间内得到value，因此哈希表常用来统计频率、快速检验某个元素是否出现过等。

**知识点2：双向链表**

双向链表是一种特殊的链表，它除了链表具有的每个节点指向后一个节点的指针外，还拥有一个每个节点指向前一个节点的指针，因此它可以任意向前或者向后访问，每次更改节点连接状态的时候，需要变动两个指针。

**思路：**

插入与访问值都是$O(1)$，没有任何一种数据结构可以直接做到。

于是我们可以想到数据结构的组合：访问$O(1)$很容易想到了哈希表；插入$O(1)$的数据结构有很多，但是如果访问到了这个地方再选择插入，且超出长度要在$O(1)$之内删除，我们可以想到用链表，可以用哈希表的key值对应链表的节点，完成直接访问。但是我们还需要把每次访问的key值节点加入链表头，同时删掉链表尾，所以选择双向链表，便于删除与移动。

于是我们的方法就是哈希表+双向链表。

**具体做法：**

- step 1：构建一个双向链表的类，记录key值与val值，同时一前一后两个指针。用哈希表存储key值和链表节点，这样我们可以根据key值在哈希表中直接锁定链表节点，从而实现在链表中直接访问，能够做到$O(1)$时间访问链表任意节点。
```java
    //设置双向链表结构
    class Node{ 
        int key;
        int val;
        Node pre;
        Node next;
        //初始化
        public Node(int key, int val) {
            this.key = key;
            this.val = val;
            this.pre = null;
            this.next = null;
        }
    }
```
- step 2：设置全局变量，记录双向链表的头、尾及LRU剩余的大小，并全部初始化，首尾相互连接好。
```java
//构建初始化连接
//链表剩余大小
this.size = k;
this.head.next = this.tail;
this.tail.pre = this.head;
```
- step 3：遍历函数的操作数组，检查第一个元素判断是属于set操作还是get操作。
- step 4：如果是set操作，即将key值与val值加入链表，我们先检查链表中是否有这个key值，可以通过哈希表检查出，如果有直接通过哈希表访问链表的相应节点，修改val值，并将访问过的节点移到表头；如果没有则需要新建节点加到表头，同时哈希表中增加相应key值（当然，还需要检查链表长度还有无剩余，若是没有剩余则需要删去链表尾）。
```java
//没有见过这个key，新值加入
if(!mp.containsKey(key)){ 
    Node node = new Node(key, val);
    mp.put(key, node);
    //超出大小，移除最后一个
    if(size <= 0) 
        removeLast();
    //大小还有剩余
    else 
        //大小减1
        size--; 
    //加到链表头
    insertFirst(node); 
}
//哈希表中已经有了，即链表里也已经有了
else{  
    mp.get(key).val = val;
    //访问过后，移到表头
    moveToHead(mp.get(key)); 
}
```

- step 5：不管是新节点，还是访问过的节点都需要加到表头，若是访问过的，需要断开原来的连接，再插入表头head的后面。
```java
//移到表头函数
void moveToHead(Node node){ 
    //已经到了表头
    if(node.pre == head)  
        return;
    //将节点断开，取出来
    node.pre.next = node.next;
    node.next.pre = node.pre;
    //插入第一个前面
    insertFirst(node);
}
```
- step 6：删除链表尾需要断掉尾节点前的连接，同时哈希表中去掉这个key值。
```java
void removeLast(){ 
    //哈希表去掉key
    mp.remove(tail.pre.key);
    //断连该节点
    tail.pre.pre.next = tail; 
    tail.pre = tail.pre.pre;
}
```
- step 7：如果是get操作，检验哈希表中有无这个key值，如果没有说明链表中也没有，返回-1，否则可以根据哈希表直接锁定链表中的位置进行访问，然后重复step 5，访问过的节点加入表头。
```java
if(mp.containsKey(key)){
    Node node = mp.get(key);
    res = node.val;
    moveToHead(node);
}
```

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220330/397721558_1648645714888/0F34B65037A5EA0C6CD439512A846548)

**Java代码实现:**
```java
import java.util.*;
public class Solution {
    //设置双向链表结构
    static class Node{ 
        int key;
        int val;
        Node pre;
        Node next;
        //初始化
        public Node(int key, int val) {
            this.key = key;
            this.val = val;
            this.pre = null;
            this.next = null;
        }
    }
    
    //哈希表
    private Map<Integer, Node> mp = new HashMap<>();
    //设置一个头
    private Node head = new Node(-1, -1); 
    //设置一个尾
    private Node tail = new Node(-1, -1); 
    private int size = 0; 
    
    public int[] LRU (int[][] operators, int k) {
        //构建初始化连接
        //链表剩余大小
        this.size = k;
        this.head.next = this.tail;
        this.tail.pre = this.head;
        //获取操作数
        int len = (int)Arrays.stream(operators).filter(x -> x[0] == 2).count();
        int[] res = new int[len];
        //遍历所有操作
        for(int i = 0, j = 0; i < operators.length; i++){
            if(operators[i][0] == 1)
                //set操作
                set(operators[i][1], operators[i][2]);
            else
                //get操作
                res[j++] = get(operators[i][1]);
        }
        return res;
    }
    
     //插入函数
    private void set(int key, int val){
        //没有见过这个key，新值加入
        if(!mp.containsKey(key)){ 
            Node node = new Node(key, val);
            mp.put(key, node);
            //超出大小，移除最后一个
            if(size <= 0) 
                removeLast();
            //大小还有剩余
            else 
                //大小减1
                size--; 
            //加到链表头
            insertFirst(node); 
        }
        //哈希表中已经有了，即链表里也已经有了
        else{  
            mp.get(key).val = val;
            //访问过后，移到表头
            moveToHead(mp.get(key)); 
        }
    }
    
    //获取数据函数
    private int get(int key){
        int res = -1;
        if(mp.containsKey(key)){
            Node node = mp.get(key);
            res = node.val;
            moveToHead(node);
        }
        return res;
    }
    //移到表头函数
    private void moveToHead(Node node){ 
        //已经到了表头
        if(node.pre == head)  
            return;
        //将节点断开，取出来
        node.pre.next = node.next;
        node.next.pre = node.pre;
        //插入第一个前面
        insertFirst(node);
    }
    
    //将节点插入表头函数
    private void insertFirst(Node node){ 
        node.pre = head;
        node.next = head.next;
        head.next.pre = node;
        head.next = node;
    }
    
    //删去表尾函数，最近最少使用
    private void removeLast(){ 
        //哈希表去掉key
        mp.remove(tail.pre.key);
        //断连该节点
        tail.pre.pre.next = tail; 
        tail.pre = tail.pre.pre;
    }
}
```
**C++代码实现**
```cpp
//双向链表
struct Node{ 
    int key;
    int val;
    Node* pre;
    Node* next;
    //初始化
    Nodke(int k, int v): key(k), val(v), pre(NULL), next(NULL){}; 
};

class Solution {
public:
    int size = 0;
    //双向链表头尾
    Node* head = NULL; 
    Node* tail = NULL;
    //哈希表记录key值
    unordered_map<int, Node*> mp;

    vector<int> LRU(vector<vector<int> >& operators, int k) {
        vector<int> res;
        //构建初始化连接
        //链表剩余大小
        size = k; 
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->pre = head;
        //操作数组为空
        if(operators.size() == 0) 
            return res;
        //遍历所有操作
        for(int i = 0; i < operators.size(); i++){ 
            auto op = operators[i];
            if(op[0] == 1) 
                //set操作
                set(op[1], op[2]);
            else if(op[0] == 2) 
                //get操作
                res.push_back(get(op[1]));
        }
        return res;
    }

    //插入函数
    void set(int key, int val){ 
        //没有见过这个key，新值加入
        if(mp.find(key) == mp.end()){ 
            Node* node = new Node(key, val);
            mp[key] = node;
            //超出大小，移除最后一个
            if(size <= 0) 
                removeLast();
            //大小还有剩余
            else 
                //大小减1
                size--; 
            //加到链表头
            insertFirst(node); 
        }
        //哈希表中已经有了，即链表里也已经有了
        else{  
            mp[key]->val = val;
            //访问过后，移到表头
            moveToHead(mp[key]); 
        }
    }

    //获取数据函数
    int get(int key){ 
        //找不到返回-1
        int res = -1; 
        //哈希表中找到
        if(mp.find(key) != mp.end()){ 
            //获取
            res = mp[key]->val;
            //访问过后移到表头
            moveToHead(mp[key]); 
        }
        return res;
    }
    
    //移到表头函数
    void moveToHead(Node* node){ 
        //已经到了表头
        if(node->pre == head)  
            return;
        //将节点断开，取出来
        node->pre->next = node->next;
        node->next->pre = node->pre;
        //插入第一个前面
        insertFirst(node);
    }
    
    //将节点插入表头函数
    void insertFirst(Node* node){ 
        node->pre = head;
        node->next = head->next;
        head->next->pre = node;
        head->next = node;
    }
    
    //删去表尾函数，最近最少使用
    void removeLast(){ 
        //哈希表去掉key
        mp.erase(tail->pre->key);
        //断连该节点
        tail->pre->pre->next = tail; 
        tail->pre = tail->pre->pre;
    }
};
```
**Python代码实现：**
```Python
#构建双向链表
class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.pre = None
        self.next = None
        
class Solution:
    def __init__(self):
        #双向链表头尾
        self.size = 0
        self.head = None
        self.tail = None
        #哈希表记录key值
        self.mp = dict()
    
    #将节点插入表头函数
    def insertFirst(self, node: Node):
        node.pre = self.head
        node.next = self.head.next
        self.head.next.pre = node
        self.head.next = node
    
    #移到表头函数
    def moveToHead(self, node: Node):
        #已经到了表头
        if node.pre == self.head:  
            return
        #将节点断开，取出来
        node.pre.next = node.next
        node.next.pre = node.pre
        #插入第一个前面
        self.insertFirst(node)
    
    #删去表尾函数，最近最少使用
    def removeLast(self):
        #哈希表去掉key
        self.mp.pop(self.tail.pre.key)
        #断连该节点
        self.tail.pre.pre.next = self.tail; 
        self.tail.pre = self.tail.pre.pre
    
    #插入函数
    def set(self, key: int, val: int):
        #没有见过这个key，新值加入
        if key not in self.mp:
            node = Node(key, val)
            self.mp[key] = node
            #超出大小，移除最后一个
            if self.size <= 0:
                self.removeLast()
            #大小还有剩余
            else:
                #大小减1
                self.size -= 1 
            #加到链表头
            self.insertFirst(node); 
        #哈希表中已经有了，即链表里也已经有了
        else:
            self.mp[key].val = val
            #访问过后，移到表头
            self.moveToHead(self.mp[key])
    
    #获取数据函数
    def get(self, key: int) -> int:
        #找不到返回-1
        res = -1
        #哈希表中找到
        if key in self.mp:
            #获取
            res = self.mp[key].val
            #访问过后移到表头
            self.moveToHead(self.mp[key])
        return res

    def LRU(self , operators: List[List[int]], k: int) -> List[int]:
        res = []
        #构建初始化连接
        #链表剩余大小
        self.size = k 
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        self.head.next = self.tail
        self.tail.pre = self.head
        #遍历所有操作
        for i in range(len(operators)):
            op = operators[i]
            if op[0] == 1: 
                #set操作
                self.set(op[1], op[2])
            else:
                #get操作
                res.append(self.get(op[1]))
        return res
```

**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为操作数组大小，map为哈希表，每次插入的复杂度都是$O(1)$
- 空间复杂度：$O(k)$，链表和哈希表都是O(k)的辅助空间