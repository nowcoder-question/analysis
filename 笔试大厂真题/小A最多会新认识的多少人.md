## 题目
[题目链接](https://www.nowcoder.com/practice/1fe6c3136d2a45fa8ef555b459b6dd26?tpId=182&tqId=362277&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个并查集问题。关键点如下：
1. $n$ 个人参加活动，每人有唯一编号 $(0 \leq i < n)$
2. $m$ 对人互相认识
3. 通过认识的人可以认识新的人
4. 需要计算小A最多能认识多少新朋友

解题思路：
1. 使用并查集记录所有人的连通关系
2. 记录小A已经直接认识的人
3. 找到小A所在连通分量的大小
4. 最终结果 = 连通分量大小 - 1(去掉自己) - 已认识的人数

---

## 代码

``` cpp []
#include <iostream>
#include <vector>
#include <set>
using namespace std;

class UnionSet {
private:
    vector<int> parents;
    vector<int> rank;
    
public:
    UnionSet(int n) {
        parents.resize(n);
        rank.resize(n, 1);
        for (int i = 0; i < n; i++) {
            parents[i] = i;
        }
    }
    
    int find(int x) {
        if (parents[x] != x) {
            parents[x] = find(parents[x]);
        }
        return parents[x];
    }
    
    void union_set(int x, int y) {
        int px = find(x), py = find(y);
        if (px != py) {
            if (rank[px] > rank[py]) {
                parents[py] = px;
                rank[px] += rank[py];
            } else {
                parents[px] = py;
                rank[py] += rank[px];
            }
        }
    }
    
    int get_rank(int x) {
        return rank[find(x)];
    }
};

int main() {
    int n, A, m;
    cin >> n >> A >> m;
    
    UnionSet us(n);
    set<int> known;
    
    for (int i = 0; i < m; i++) {
        int x, y;
        scanf("%d,%d", &x, &y);
        if (x == A) known.insert(y);
        else if (y == A) known.insert(x);
        us.union_set(x, y);
    }
    
    cout << us.get_rank(A) - 1 - known.size() << endl;
    return 0;
}
```

``` java []
import java.util.*;

public class Main {
    static class UnionSet {
        private int[] parents;
        private int[] rank;
        
        public UnionSet(int n) {
            parents = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; i++) {
                parents[i] = i;
                rank[i] = 1;
            }
        }
        
        public int find(int x) {
            if (parents[x] != x) {
                parents[x] = find(parents[x]);
            }
            return parents[x];
        }
        
        public void union(int x, int y) {
            int px = find(x), py = find(y);
            if (px != py) {
                if (rank[px] > rank[py]) {
                    parents[py] = px;
                    rank[px] += rank[py];
                } else {
                    parents[px] = py;
                    rank[py] += rank[px];
                }
            }
        }
        
        public int getRank(int x) {
            return rank[find(x)];
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int A = sc.nextInt();
        int m = sc.nextInt();
        sc.nextLine();
        
        UnionSet us = new UnionSet(n);
        Set<Integer> known = new HashSet<>();
        
        for (int i = 0; i < m; i++) {
            String[] line = sc.nextLine().split(",");
            int x = Integer.parseInt(line[0]);
            int y = Integer.parseInt(line[1]);
            if (x == A) known.add(y);
            else if (y == A) known.add(x);
            us.union(x, y);
        }
        
        System.out.println(us.getRank(A) - 1 - known.size());
    }
}
```

``` python []
class UnionSet:
    def __init__(self, n):
        self.parents = [i for i in range(n)]
        self.rank = [1] * n
        
    def find(self, x):
        if self.parents[x] != x:
            self.parents[x] = self.find(self.parents[x])
        return self.parents[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px != py:
            if self.rank[px] > self.rank[py]:
                self.parents[py] = px
                self.rank[px] += self.rank[py]
            else:
                self.parents[px] = py
                self.rank[py] += self.rank[px]

n = int(input())
A = int(input())
m = int(input())

us = UnionSet(n)
known = set()

for _ in range(m):
    x, y = map(int, input().split(','))
    if x == A:
        known.add(y)
    elif y == A:
        known.add(x)
    us.union(x, y)

root = us.find(A)
total = sum(1 for i in range(n) if us.find(i) == root)
print(total - 1 - len(known))
```

---

## 算法及复杂度
- 算法：并查集
- 时间复杂度：$\mathcal{O}(m\alpha(n))$ - $m$ 为边数，$\alpha(n)$ 为阿克曼函数的反函数，近似为常数
- 空间复杂度：$\mathcal{O}(n)$ - 需要存储并查集的父节点数组和秩数组