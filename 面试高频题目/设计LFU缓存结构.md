## 题目
[题目链接](https://www.nowcoder.com/practice/93aacb4a887b46d897b00823f30bfea1?tpId=196&tqId=1006014&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 实现LFU的set与get函数，且复杂度为$O(1)$
- 每次调用这两个函数会给一个频率赋值，超出长度则移除频率最少的，若有频率相同，则移除访问时间最早的

##### 举一反三：
学习完本题的思路你可以解决如下题目：

[BM100. 设计LRU缓存结构](https://www.nowcoder.com/practice/e3769a5f49894d49b871c09cadd13a61?tpId=295&sfm=html&channel=nowcoder)

##### 方法：双哈希表（推荐使用）

**知识点：哈希表**

哈希表是一种根据关键码（key）直接访问值（value）的一种数据结构。而这种直接访问意味着只要知道key就能在$O(1)$时间内得到value，因此哈希表常用来统计频率、快速检验某个元素是否出现过等。

**思路：**

需要在$O(1)$时间内实现两个操作，我们第一时间想到的还是哈希表，利用哈希表保存LFU的key值，而哈希表的value值对应了另一边存着每个缓存需要的类的节点，这样就实现了直接访问。

但是我们还需要每次最快找到最久未使用的频率最小的节点，这时候我们可以考虑使用一个全局变量，跟踪记录最小的频率，有了最小的频率，怎样直接找到这个频率最小的节点，还是使用哈希表，key值记录各个频率，而value值就是后面接了一串相同频率的节点。如何保证每次都是最小频率的最久为使用，我们用双向链表将统一频率的节点连起来就好了，每次新加入这个频率的都在链表头，而需要去掉的都在链表尾。

这样我们方法就是双哈希表。

**具体做法：**

- step 1：需要建立每个节点，每个节点代表加入LFU中的结构，需要频率，key值，val值。（C++代码中使用数组的三维实现）
```java
class Node{ 
    int freq;
    int key;
    int val;
    //初始化
    public Node(int freq, int key, int val) {
        this.freq = freq;
        this.key = key;
        this.val = val;
    }
}
```
- step 2：建立第一个哈希表mp，在每个节点的key值与节点直接建立映射连接，建立第二个哈希表freq_mp，在频率与该频率下的处在双向链表中的节点们直接建立映射连接（即与这个双向链表建立映射）。这样可以根据key值直接访问每个节点，通过频率直接找到最少使用且最久未使用的节点。复杂度都是$O(1)$。
```java
//频率到双向链表的哈希表
Map<Integer, LinkedList<Node> > freq_mp = new HashMap<>();
//key到节点的哈希表
Map<Integer, Node> mp = new HashMap<>();
```
- step 3：LFU的剩余容量和当前最小频率设置为全局变量，并初始化。
- step 4：遍历函数的操作数组，检查第一个元素判断是属于set操作还是get操作。
- step 5：对于set操作，如果哈希表中已经有了key值，说明它已经在LFU中了，直接修改节点频率和val即可。
```java
//在哈希表中找到key值
if(mp.containsKey(key)) 
    //若是哈希表中有，则更新值与频率
    update(mp.get(key), key, value);
```
- step 6：如果哈希表中没有，此时要考虑还有没有容量，若是有容量，容量需要减1；若是没有容量，从第二个哈希表中根据当前最小频率取出最小频率的双向链表，链表最后一个就是使用频率最低且最久未使用的，将其取出：链表中删除，哈希表中也有删除，如果链表为空了，则相应频率在哈希表中也要删除。
```java
//哈希表中没有，即链表中没有
if(size == 0){
    //满容量取频率最低且最早的删掉
    int oldkey = freq_mp.get(min_freq).getLast().key; 
    //频率哈希表的链表中删除
    freq_mp.get(min_freq).removeLast(); 
    if(freq_mp.get(min_freq).isEmpty()) 
        freq_mp.remove(min_freq); 
    //链表哈希表中删除
    mp.remove(oldkey); 
}
```
- step 7：加入的时候，添加新的节点，频次为1，节点加入第一个哈希表，同时加入第二个哈希表相应频率链表的首部。
```java
if(!freq_mp.containsKey(freq + 1))
    freq_mp.put(freq + 1, new LinkedList<Node>());
//插入频率加一的双向链表表头，链表中对应：freq key value
freq_mp.get(freq + 1).addFirst(new Node(freq + 1, key, value)); 
mp.put(key, freq_mp.get(freq + 1).getFirst());
```
- step 8：对于get操作，直接看第一个哈希表key值有没有找到，有则可以访问，然后修改频率，没有则返回-1.
```java
if(mp.containsKey(key)){ 
    Node node = mp.get(key);
    //根据哈希表直接获取值
    res = node.val;
    //更新频率 
    update(node, key, res); 
}
```
- step 9：修改频率的时候，将该频率下该节点取出，放入哈希表该频率加1的链表首部。若是该频率下链表没有节点了，则哈希表中删除这个频率，同时若是修改前的链表频率与最低频率相等，说明最低已经增长了。
```java
//找到频率
int freq = node.freq;
//原频率中删除该节点
freq_mp.get(freq).remove(node); 
//哈希表中该频率已无节点，直接删除
if(freq_mp.get(freq).isEmpty()){ 
    freq_mp.remove(freq);
    //若当前频率为最小，最小频率加1
    if(min_freq == freq) 
        min_freq++;
    }
......//插入频率加一的双向链表表头，链表中对应：freq key value
```

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220330/397721558_1648644872065/317C904D0A74208D0CAE3857783F8413)

**Java代码实现:**
```java
import java.util.*;
public class Solution {
    //设置节点结构
    static class Node{ 
        int freq;
        int key;
        int val;
        //初始化
        public Node(int freq, int key, int val) {
            this.freq = freq;
            this.key = key;
            this.val = val;
        }
    }
    //频率到双向链表的哈希表
    private Map<Integer, LinkedList<Node> > freq_mp = new HashMap<>();
    //key到节点的哈希表
    private Map<Integer, Node> mp = new HashMap<>();
    //记录缓存剩余容量
    private int size = 0; 
    //记录当前最小频次
    private int min_freq = 0;
    
    public int[] LFU (int[][] operators, int k) {
        //构建初始化连接
        //链表剩余大小
        this.size = k;
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
    
    //调用函数时更新频率或者val值
    private void update(Node node, int key, int value) { 
        //找到频率
        int freq = node.freq;
        //原频率中删除该节点
        freq_mp.get(freq).remove(node); 
        //哈希表中该频率已无节点，直接删除
        if(freq_mp.get(freq).isEmpty()){ 
            freq_mp.remove(freq);
            //若当前频率为最小，最小频率加1
            if(min_freq == freq) 
                min_freq++;
        }
        if(!freq_mp.containsKey(freq + 1))
            freq_mp.put(freq + 1, new LinkedList<Node>());
        //插入频率加一的双向链表表头，链表中对应：freq key value
        freq_mp.get(freq + 1).addFirst(new Node(freq + 1, key, value)); 
        mp.put(key, freq_mp.get(freq + 1).getFirst());
    }
    
    //set操作函数
    private void set(int key, int value) {
        //在哈希表中找到key值
        if(mp.containsKey(key)) 
            //若是哈希表中有，则更新值与频率
            update(mp.get(key), key, value);
        else{ 
            //哈希表中没有，即链表中没有
            if(size == 0){
                //满容量取频率最低且最早的删掉
                int oldkey = freq_mp.get(min_freq).getLast().key; 
                //频率哈希表的链表中删除
                freq_mp.get(min_freq).removeLast(); 
                if(freq_mp.get(min_freq).isEmpty()) 
                    freq_mp.remove(min_freq); 
                //链表哈希表中删除
                mp.remove(oldkey); 
            }
            //若有空闲则直接加入，容量减1
            else 
                size--; 
            //最小频率置为1
            min_freq = 1; 
            //在频率为1的双向链表表头插入该键
            if(!freq_mp.containsKey(1))
                freq_mp.put(1, new LinkedList<Node>());
            freq_mp.get(1).addFirst(new Node(1, key, value)); 
            //哈希表key值指向链表中该位置
            mp.put(key, freq_mp.get(1).getFirst()); 
        }
    }
    
    //get操作函数
    private int get(int key) {
        int res = -1;
        //查找哈希表
        if(mp.containsKey(key)){ 
            Node node = mp.get(key);
            //根据哈希表直接获取值
            res = node.val;
            //更新频率 
            update(node, key, res); 
        }
        return res;
    }
}
```
**C++代码实现**
```cpp
class Solution {
public:
    //用list模拟双向链表，双向链表中数组第0位为频率，第1位为key，第2位为val
    //频率到双向链表的哈希表
    unordered_map<int, list<vector<int> > > freq_mp; 
    //key到双向链表节点的哈希表
    unordered_map<int, list<vector<int> > ::iterator> mp;
    //记录当前最小频次
    int min_freq = 0; 
    //记录缓存剩余容量
    int size = 0; 
    
    vector<int> LFU(vector<vector<int> >& operators, int k) {
        //记录输出
        vector<int> res; 
        size = k; 
        //遍历所有操作
        for(int i = 0; i < operators.size(); i++){
            auto op = operators[i];
            if(op[0] == 1)
                //set操作
                set(op[1], op[2]);
            else
                //get操作
                res.push_back(get(op[1]));
        }
        return res;
    }
    
    //调用函数时更新频率或者val值
    void update(list<vector<int> >::iterator iter, int key, int value) { 
        //找到频率
        int freq = (*iter)[0];
        //原频率中删除该节点
        freq_mp[freq].erase(iter); 
        //哈希表中该频率已无节点，直接删除
        if(freq_mp[freq].empty()){ 
            freq_mp.erase(freq);
            //若当前频率为最小，最小频率加1
            if(min_freq == freq) 
                min_freq++;
        }
        //插入频率加一的双向链表表头，链表中对应：freq key value
        freq_mp[freq + 1].push_front({freq + 1, key, value}); 
        mp[key] = freq_mp[freq + 1].begin(); 
    }
    
    //set操作函数
    void set(int key, int value) {
        //在哈希表中找到key值
        auto it = mp.find(key); 
        if(it != mp.end())
            //若是哈希表中有，则更新值与频率
            update(it->second, key, value);
        else{ 
            //哈希表中没有，即链表中没有
            if(size == 0){
                //满容量取频率最低且最早的删掉
                int oldkey = freq_mp[min_freq].back()[1]; 
                //频率哈希表中删除
                freq_mp[min_freq].pop_back(); 
                if(freq_mp[min_freq].empty()) 
                    freq_mp.erase(min_freq); 
                //链表哈希表中删除
                mp.erase(oldkey); 
            }
            //若有空闲则直接加入，容量减1
            else 
                size--; 
            //最小频率置为1
            min_freq = 1; 
            //在频率为1的双向链表表头插入该键
            freq_mp[1].push_front({1, key, value}); 
            //哈希表key值指向链表中该位置
            mp[key] = freq_mp[1].begin(); 
        }
    }
    
    //get操作函数
    int get(int key) {
        int res = -1;
        //查找哈希表
        auto it = mp.find(key);
        if(it != mp.end()){ 
            auto iter = it->second; 
            //根据哈希表直接获取值
            res = (*iter)[2];
            //更新频率 
            update(iter, key, res); 
        }
        return res;
    }
};
```
**Python代码实现：**
```Python
import collections
class Node:
    def __init__(self, freq, key, val):
        self.freq = freq
        self.key = key
        self.val = val
        
class Solution:
    def __init__(self):
        #记录剩余空间
        self.size = 0
        #key到双向链表节点的哈希表
        self.mp = dict()
        #频率到链表的哈希表
        self.freq_mp = dict(collections.deque())
        #记录当前最小频次
        self.min_freq = 0
    
    #调用函数时更新频率或者val值
    def update(self, node: Node, key: int, value: int): 
        #找到频率
        freq = node.freq
        #原频率中删除该节点
        self.freq_mp[freq].remove(node) 
        #哈希表中该频率已无节点，直接删除
        if len(self.freq_mp[freq]) == 0: 
            self.freq_mp.pop(freq)
            #若当前频率为最小，最小频率加1
            if self.min_freq == freq: 
                self.min_freq += 1
        #插入频率加一的双向链表表头，链表中对应：freq key value
        node = Node(freq + 1, key, value)
        if freq + 1 not in self.freq_mp:
            self.freq_mp[freq + 1] = collections.deque()
        self.freq_mp[freq + 1].appendleft(node)
        self.mp[key] = self.freq_mp[freq + 1][0]
    
    #set操作函数
    def set(self, key:int, value: int):
        #在哈希表中找到key值
        if key in self.mp:
            #若是哈希表中有，则更新值与频率
            self.update(self.mp[key], key, value)
        else:
            #哈希表中没有，即链表中没有
            if self.size == 0:
                #满容量取频率最低且最早的删掉
                oldnode = self.freq_mp[self.min_freq].pop() 
                #频率哈希表的链表中删除
                if len(self.freq_mp[self.min_freq]) == 0:
                    self.freq_mp.pop(self.min_freq) 
                #链表哈希表中删除
                self.mp.pop(oldnode.key)
            #若有空闲则直接加入，容量减1
            else: 
                self.size -= 1
            #最小频率置为1
            self.min_freq = 1
            node = Node(1, key, value)
            if 1 not in self.freq_mp:
                self.freq_mp[1] = collections.deque()
            self.freq_mp[1].appendleft(node)
            #哈希表key值指向链表中该位置
            self.mp[key] = self.freq_mp[1][0]
            
    #get操作函数
    def get(self, key: int) -> int:
        res = -1
        #查找哈希表
        if key in self.mp:
            node = self.mp[key]
            #根据哈希表直接获取值
            res = node.val
            #更新频率 
            self.update(node, key, res)
        return res

    def LFU(self , operators: List[List[int]], k: int) -> List[int]:
        res = []
        #构建初始化连接
        #链表剩余大小
        self.size = k
        #遍历所有操作
        for i in range(len(operators)):
            op = operators[i]
            if op[0] == 1: 
                #set操作
                self.set(op[1], op[2])
            else:
                #get操作
                res.append(self.get(op[1]))
        return resnd(self.get(op[1]))
        return res
```

**复杂度分析：**
- 时间复杂度：$O(n)$，取决于操作数$n$
- 空间复杂度：$O(k)$，取决于缓存容量$k$