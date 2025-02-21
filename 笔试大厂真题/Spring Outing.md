## 题目
[题目链接](https://www.nowcoder.com/practice/84addf13322a42ad80da5fc89e784ea1?tpId=182&tqId=23748&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个投票选择问题，需要从后向前模拟投票过程：

1. **核心思路**：
   - 每个人都有自己的偏好排序（包括留在家里的选项0）
   - 从最后一个选项开始，逐个检查是否能获得多数支持
   - 如果某个选项比当前结果更受欢迎，则更新结果

2. **关键点**：
   - 使用二维 $\text{vector}$ 存储每个人对每个选项的偏好排名
   - 通过比较排名来判断支持度
   - 需要超过半数支持才能更新结果

3. **处理流程**：
   - 读取输入并构建偏好矩阵
   - 从后往前处理每个选项
   - 统计支持票数并更新结果

---

## 代码

```cpp []
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    // 处理投票过程
    void processVotes(const vector<vector<int>>& preferences, int n, int k, int& result) {
        for (int candidate = k; candidate > 0; candidate--) {
            int votes = 0;
            // 统计支持当前候选人的票数
            for (int i = 0; i < n; i++) {
                if (preferences[i][candidate] < preferences[i][result]) {
                    votes++;
                }
            }
            // 如果获得超过半数支持，更新结果
            if (votes > n / 2) {
                result = candidate;
            }
        }
    }

    void solve() {
        int n, k;
        while (cin >> n >> k) {
            // 存储每个人的偏好
            vector<vector<int>> preferences(n, vector<int>(k + 1, 0));
            
            // 读取并处理输入
            for (int i = 0; i < n; i++) {
                for (int j = 0; j <= k; j++) {
                    int temp;
                    cin >> temp;
                    preferences[i][temp] = j;  // 记录排名
                }
            }

            // 处理投票
            int result = 0;  // 初始结果为留在家里
            processVotes(preferences, n, k, result);

            // 输出结果
            cout << (result ? to_string(result) : "otaku") << endl;
        }
    }
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    Solution solution;
    solution.solve();
    
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    static class Solution {
        private void processVotes(int[][] preferences, int n, int k, int[] result) {
            for (int candidate = k; candidate > 0; candidate--) {
                int votes = 0;
                for (int i = 0; i < n; i++) {
                    if (preferences[i][candidate] < preferences[i][result[0]]) {
                        votes++;
                    }
                }
                if (votes > n / 2) {
                    result[0] = candidate;
                }
            }
        }

        public void solve(Scanner sc) {
            while (sc.hasNext()) {
                int n = sc.nextInt();
                int k = sc.nextInt();

                int[][] preferences = new int[n][k + 1];
                
                // 读取并处理输入
                for (int i = 0; i < n; i++) {
                    for (int j = 0; j <= k; j++) {
                        int temp = sc.nextInt();
                        preferences[i][temp] = j;
                    }
                }

                // 使用数组来模拟引用传递
                int[] result = {0};
                processVotes(preferences, n, k, result);

                // 输出结果
                System.out.println(result[0] == 0 ? "otaku" : result[0]);
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Solution solution = new Solution();
        solution.solve(sc);
        sc.close();
    }
}
```
```python []

def process_votes(preferences: list, n: int, k: int) -> int:
    result = 0
    for candidate in range(k, 0, -1):
        votes = sum(1 for i in range(n) 
                   if preferences[i][candidate] < preferences[i][result])
        if votes > n // 2:
            result = candidate
    return result

def solve():
    n, k = map(int, input().split())
    
    preferences = [[0] * (k + 1) for _ in range(n)]
    for i in range(n):
        temp = list(map(int, input().split()))
        for j, val in enumerate(temp):
            preferences[i][val] = j
    
    result = process_votes(preferences, n, k)
    print(result if result else "otaku")

while True:
    try:
        solve()
    except EOFError:
        break
```
---

## 算法及复杂度
- 算法：模拟 + 投票统计
- 时间复杂度：$\mathcal{O}(NK)$，其中 $N$ 是人数，$K$ 是候选地点数
- 空间复杂度：$\mathcal{O}(NK)$，用于存储偏好矩阵