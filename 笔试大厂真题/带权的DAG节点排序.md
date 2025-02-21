## 题目
[题目链接](https://www.nowcoder.com/practice/8bbe82c9bf30434797bc17693771d9bb?tpId=182&tqId=314239&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 基本思路：
   - 使用拓扑排序算法对DAG进行排序，同时考虑节点的权重。
   - 在拓扑排序过程中，优先选择权重大的节点进行排序。

2. 实现方法：
   - 使用Kahn算法进行拓扑排序。
   - 使用优先队列（最大堆）来确保每次选择权重最大的节点。

---

## 代码

```cpp []
#include <iostream>
#include <vector>
#include <queue>
#include <utility>
using namespace std;

int main() {
    int n, e;
    cin >> n >> e;

    vector<int> weights(n + 1);
    vector<vector<int>> graph(n + 1);
    vector<int> in_degree(n + 1, 0);

    // 读取节点的权重
    for (int i = 1; i <= n; i++) {
        int seq, weight;
        cin >> seq >> weight;
        weights[seq] = weight;
    }

    // 读取边的信息
    for (int i = 0; i < e; i++) {
        int s, t;
        cin >> s >> t;
        graph[s].push_back(t);
        in_degree[t]++;
    }

    // 使用优先队列进行拓扑排序
    priority_queue<pair<int, int>> pq; // (weight, node)
    for (int i = 1; i <= n; i++) {
        if (in_degree[i] == 0) {
            pq.push({weights[i], i});
        }
    }

    vector<int> result;
    while (!pq.empty()) {
        int node = pq.top().second;
        pq.pop();
        result.push_back(node);

        for (int neighbor : graph[node]) {
            in_degree[neighbor]--;
            if (in_degree[neighbor] == 0) {
                pq.push({weights[neighbor], neighbor});
            }
        }
    }

    // 输出结果
    for (int i = 0; i < result.size(); i++) {
        cout << result[i];
        if (i < result.size() - 1) {
            cout << " ";
        }
    }
    cout << endl;

    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int e = sc.nextInt();

        int[] weights = new int[n + 1];
        List<List<Integer>> graph = new ArrayList<>(n + 1);
        int[] inDegree = new int[n + 1];

        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }

        // 读取节点的权重
        for (int i = 1; i <= n; i++) {
            int seq = sc.nextInt();
            int weight = sc.nextInt();
            weights[seq] = weight;
        }

        // 读取边的信息
        for (int i = 0; i < e; i++) {
            int s = sc.nextInt();
            int t = sc.nextInt();
            graph.get(s).add(t);
            inDegree[t]++;
        }

        // 使用优先队列进行拓扑排序
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]); // (weight, node)
        for (int i = 1; i <= n; i++) {
            if (inDegree[i] == 0) {
                pq.offer(new int[]{weights[i], i});
            }
        }

        List<Integer> result = new ArrayList<>();
        while (!pq.isEmpty()) {
            int node = pq.poll()[1];
            result.add(node);

            for (int neighbor : graph.get(node)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
                    pq.offer(new int[]{weights[neighbor], neighbor});
                }
            }
        }

        // 输出结果
        for (int i = 0; i < result.size(); i++) {
            System.out.print(result.get(i));
            if (i < result.size() - 1) {
                System.out.print(" ");
            }
        }
        System.out.println();
    }
}
```

```python []
import heapq

def topological_sort(n, e, weights, edges):
    graph = [[] for _ in range(n + 1)]
    in_degree = [0] * (n + 1)

    # 构建图和入度数组
    for s, t in edges:
        graph[s].append(t)
        in_degree[t] += 1

    # 使用优先队列进行拓扑排序
    pq = []
    for i in range(1, n + 1):
        if in_degree[i] == 0:
            heapq.heappush(pq, (-weights[i], i))  # 使用负值以实现最大堆

    result = []
    while pq:
        weight, node = heapq.heappop(pq)
        result.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                heapq.heappush(pq, (-weights[neighbor], neighbor))

    return result

if __name__ == "__main__":
    n, e = map(int, input().split())
    weights = [0] * (n + 1)
    for _ in range(n):
        seq, weight = map(int, input().split())
        weights[seq] = weight

    edges = [tuple(map(int, input().split())) for _ in range(e)]
    result = topological_sort(n, e, weights, edges)

    print(" ".join(map(str, result)))
```

---

## 算法及复杂度
- 算法：拓扑排序
- 时间复杂度：$\mathcal{O}(n + e)$，其中 $n$ 为节点数，$e$ 为边数
- 空间复杂度：$\mathcal{O}(n + e)$，用于存储图和入度数组