## 题目
[题目链接](https://www.nowcoder.com/practice/3da4aeb1c76042f2bc70dbcb94513338?tpId=182&tqId=365881&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道数据结构设计题，主要思路如下：

1. 问题分析：
   - 需要实现LRU(Least Recently Used)缓存
   - 支持get和put两个操作
   - get操作需要更新访问时间
   - put操作需要处理容量限制
   - 所有操作要求O(1)时间复杂度

2. 解决方案：
   - 使用双向链表存储键值对
   - 使用哈希表维护键到链表节点的映射
   - 最近使用的元素放在链表尾部
   - 需要淘汰时删除链表头部元素

3. 实现细节：
   - 使用STL的list实现双向链表
   - 使用unordered_map实现哈希表
   - 使用splice操作优化节点移动

---

## 代码
```cpp []
#include <cstdio>
#include <list>
#include <unordered_map>
using namespace std;

class LRUCache {
    list<pair<int, int>> l;  // 双向链表存储(key,value)对
    unordered_map<int, list<pair<int, int>>::iterator> m;  // 哈希表存储key到节点的映射
    unsigned limit;  // 缓存容量
    
public:
    LRUCache(unsigned limit_) : limit(limit_) {}
    
    int get(int key) {
        if (m.count(key)) {
            auto it = m[key];
            l.splice(l.end(), l, it);  // 移动到末尾
            return it->second;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if (m.count(key)) {
            // key存在，更新值
            m[key]->second = value;
        }
        else if (limit) {
            // key不存在且有容量
            if (m.size() == limit) {
                // 删除最久未使用的
                m.erase(l.begin()->first);
                l.erase(l.begin());
            }
            // 插入新节点
            m[key] = l.insert(l.end(), make_pair(key, value));
        }
    }
};

int main() {
    int limit, x, y;
    scanf("%d", &limit);
    LRUCache cache(unsigned(limit < 0 ? 0 : limit));
    
    char op[4];
    while (scanf("%s%d", op, &x) != EOF) {
        if (*op == 'g') {
            printf("%d\n", cache.get(x));
        } else {
            scanf("%d", &y);
            cache.put(x, y);
        }
    }
    return 0;
}
```

```java []
import java.io.*;
import java.util.*;
   
public class Main{
   
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        LRUCache lru = new LRUCache(n);
        String str = null;
        StringBuilder sb = new StringBuilder();
        if (n == 0){
            while ((str = br.readLine())!= null){
                if (str.charAt(0) == 'g')
                    sb.append(-1).append("\n");
            }
            System.out.println(sb.substring(0, sb.length()-1));
            return;
        }
        while ((str = br.readLine())!= null){
            String[] strs = str.split(" ");
            if (str.charAt(0) == 'p'){
                lru.put(Integer.parseInt(strs[1]), Integer.parseInt(strs[2]));
            }else{
                sb.append(lru.get(Integer.parseInt(strs[1]))).append("\n");
            }
        }
        System.out.println(sb.substring(0, sb.length()-1));
    }
   
    public static class DLinkedNode{
        int key;
        int val;
        DLinkedNode pre;
        DLinkedNode next;
   
        public DLinkedNode(int key, int val){
            this.key = key;
            this.val = val;
        }
    }
   
    public static class LRUCache{
        int maxSize;
        DLinkedNode head;
        Map<Integer, DLinkedNode> map;
   
        public LRUCache(int maxSize){
            this.maxSize = maxSize;
            head = new DLinkedNode(-1, -1);
            head.pre = head;
            head.next = head;
            map = new HashMap<>();
        }
   
        public int get(int key){
            if (map.containsKey(key)){
                DLinkedNode node = map.get(key);
                node.next.pre = node.pre;
                node.pre.next = node.next;
                insertFirst(node);
                return node.val;
            }
            return -1;
        }
   
        public void put(int key, int value){
            if (map.containsKey(key)){
                map.get(key).val = value;
                return;
            }
            if (map.size() == maxSize){
                map.remove(head.pre.key);
                head.pre = head.pre.pre;
                head.pre.next = head;
            }
            DLinkedNode node = new DLinkedNode(key, value);
            map.put(key, node);
            insertFirst(node);
        }
   
        public void insertFirst(DLinkedNode node){
            head.next.pre = node;
            node.next = head.next;
            node.pre = head;
            head.next = node;
        }
    }
}
```

```python []
import sys
  
def solve():
    k = int(sys.stdin.readline().strip())
    data, dic = [], {}
    if k <=0:
        while True:
            line = sys.stdin.readline().strip()
            if not line:
                break
            else:
                line = line.split()
                if line[0] == 'g':
                    print(-1)
    else:
        while True:
            line = sys.stdin.readline().strip()
            if not line:
                break
            else:
                line = line.split()
            if line[0] == 'p':
                if line[1] in dic.keys():
                    dic[line[1]] = line[2]
                elif len(data) < k:
                    dic[line[1]] = line[2]
                    data.append(line[1])
                else:
                    key = data.pop(0)
                    dic.pop(key)
                    data.append(line[1])
                    dic[line[1]] = line[2]
            elif line[0] == 'g':
                if line[1] not in dic.keys():
                    print(-1)
                else:
                    data.remove(line[1])
                    data.append(line[1])
                    print(dic[line[1]])

if __name__ == '__main__':
    solve()

```
---

## 算法及复杂度
- 算法：哈希表 + 双向链表
- 时间复杂度：$\mathcal{O}(1)$ - 所有操作都是常数时间
- 空间复杂度：$\mathcal{O}(capacity)$ - 缓存容量