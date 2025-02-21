## 题目
[题目链接](https://www.nowcoder.com/practice/bc858b8477c44951bfde0c5941396622?tpId=182&tqId=162091&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道树形结构的路径选择问题，主要约束条件如下：

1. 每条道路要么全部布置花灯，要么完全不布置
2. 至少要有一条通向首都的路径布置花灯
3. 每个城市最多选择两条相连道路布置花灯
4. 所有布置花灯的道路必须构成连通子图
5. 总路径长度不能超过给定上限 $m$

---

## 代码

```cpp []
#include <bits/stdc++.h>
using namespace std;

using Array = vector<int>;

set<int> dfs(int root, vector<Array>& sons, Array& d, int m) {
    if (sons[root].size() == 0)
        return set<int>({0});
    vector<set<int>> sts;
    for (auto it = sons[root].begin(); it != sons[root].end(); ++it) {
        sts.push_back(dfs(*it, sons, d, m));
    }
    set<int> ret({0});
    // 选择单条路径
    for (auto it = sts.begin(); it != sts.end(); ++it)
        for (auto it2 = it->begin(); it2 != it->end(); ++it2)
            if (d[sons[root][it - sts.begin()]] + *it2 <= m)
                ret.insert(d[sons[root][it - sts.begin()]] + *it2);
    // 选择两条路径
    for (auto it_i = sts.begin(); next(it_i) != sts.end(); ++it_i) {
        for (auto it_j = it_i + 1; it_j != sts.end(); ++it_j) {
            for (auto it = it_i->begin(); it != it_i->end(); ++it) {
                for (auto it2 = it_j->begin(); it2 != it_j->end(); ++it2) {
                    if (d[sons[root][it_i - sts.begin()]] + d[sons[root][it_j - sts.begin()]] + *it + *it2 <= m)
                        ret.insert(d[sons[root][it_i - sts.begin()]] + d[sons[root][it_j - sts.begin()]] + *it + *it2);
                }
            }
        }
    }
    return ret;
}

int main() {
    int m, n;
    while (cin >> m >> n) {
        vector<Array> sons(n);
        Array father(n, -1), d(n, 0);
        for (int i = 0, u, v, dd; i < n - 1; i++) {
            cin >> u >> v >> dd;
            d[v] = dd;
            sons[u].push_back(v);
            father[v] = u;
        }
        int root = 0;
        for (; root < father.size() && father[root] != -1; ++root) {}
        set<int> st = dfs(root, sons, d, m);
        cout << (st.empty() ? 0 : *st.rbegin()) << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Node {
        List<Integer> sons = new ArrayList<>();
    }
    
    static TreeSet<Integer> dfs(int root, Node[] nodes, int[] dist, int m) {
        if (nodes[root].sons.isEmpty()) {
            return new TreeSet<>(Arrays.asList(0));
        }
        
        List<TreeSet<Integer>> childPaths = new ArrayList<>();
        for (int son : nodes[root].sons) {
            childPaths.add(dfs(son, nodes, dist, m));
        }
        
        TreeSet<Integer> result = new TreeSet<>();
        result.add(0);
        
        // 选择单条路径
        for (int i = 0; i < childPaths.size(); i++) {
            for (int len : childPaths.get(i)) {
                if (dist[nodes[root].sons.get(i)] + len <= m) {
                    result.add(dist[nodes[root].sons.get(i)] + len);
                }
            }
        }
        
        // 选择两条路径
        for (int i = 0; i < childPaths.size() - 1; i++) {
            for (int j = i + 1; j < childPaths.size(); j++) {
                for (int len1 : childPaths.get(i)) {
                    for (int len2 : childPaths.get(j)) {
                        int total = dist[nodes[root].sons.get(i)] + 
                                  dist[nodes[root].sons.get(j)] + len1 + len2;
                        if (total <= m) result.add(total);
                    }
                }
            }
        }
        return result;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int m = sc.nextInt();
            int n = sc.nextInt();
            Node[] nodes = new Node[n];
            int[] father = new int[n];
            int[] dist = new int[n];
            Arrays.fill(father, -1);
            
            for (int i = 0; i < n; i++) nodes[i] = new Node();
            
            for (int i = 0; i < n - 1; i++) {
                int u = sc.nextInt();
                int v = sc.nextInt();
                int d = sc.nextInt();
                nodes[u].sons.add(v);
                father[v] = u;
                dist[v] = d;
            }
            
            int root = 0;
            while (root < n && father[root] != -1) root++;
            
            TreeSet<Integer> paths = dfs(root, nodes, dist, m);
            System.out.println(paths.isEmpty() ? 0 : paths.last());
        }
    }
}
```

```python []
from collections import defaultdict

def dfs(root, sons, dist, m):
    if not sons[root]:
        return {0}
    
    child_paths = []
    for son in sons[root]:
        child_paths.append(dfs(son, sons, dist, m))
    
    result = {0}
    
    # 选择单条路径
    for i, paths in enumerate(child_paths):
        for length in paths:
            total = dist[sons[root][i]] + length
            if total <= m:
                result.add(total)
    
    # 选择两条路径
    for i in range(len(child_paths)):
        for j in range(i + 1, len(child_paths)):
            for len1 in child_paths[i]:
                for len2 in child_paths[j]:
                    total = dist[sons[root][i]] + dist[sons[root][j]] + len1 + len2
                    if total <= m:
                        result.add(total)
    
    return result

while True:
    try:
        m = int(input())
        n = int(input())
        sons = defaultdict(list)
        father = [-1] * n
        dist = [0] * n
        
        for _ in range(n - 1):
            u, v, d = map(int, input().split())
            sons[u].append(v)
            father[v] = u
            dist[v] = d
        
        root = next(i for i in range(n) if father[i] == -1)
        paths = dfs(root, sons, dist, m)
        print(max(paths) if paths else 0)
        
    except EOFError:
        break
```

---

## 算法及复杂度
- 算法：DFS + 动态规划
- 时间复杂度：$\mathcal{O}(n \cdot 2^d)$ - $n$ 为节点数，$d$ 为最大度数
- 空间复杂度：$\mathcal{O}(n)$ - 需要存储树结构和路径长度