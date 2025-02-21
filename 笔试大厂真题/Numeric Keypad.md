## 题目
[题目链接](https://www.nowcoder.com/practice/f46db1d185114716abbeaea1d58ffd62?tpId=182&tqId=23747&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个数字构造问题，使用DFS解决：

1. **预处理可达数字**：
   - 对于每个位置，预处理可以到达的所有数字
   - 从大到小排序，便于找到最大可行解

2. **DFS策略**：
   - 从高位到低位逐位构造
   - 使用limit标记是否受到上界限制
   - 当找到第一个可行解时立即返回

3. **剪枝优化**：
   - 对于每个位置，只考虑不超过上界的数字
   - 从大到小尝试，保证找到的是最大解

---

## 代码

```c++ []
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class Solution {
private:
    vector<vector<int>> e;
    bool found;
    string result;
    
    void init() {
        e.resize(10);
        // 预处理每个位置可以到达的数字
        e[1] = {9,8,7,6,5,4,3,2,1,0};
        e[2] = {9,8,6,5,3,2,0};
        e[3] = {9,6,3};
        e[4] = {9,8,7,6,5,4,0};
        e[5] = {9,8,6,0};
        e[6] = {9,6};
        e[7] = {9,8,7,0};
        e[8] = {9,8,0};
        e[9] = {9};
        e[0] = {0};
    }
    
    void dfs(int pos, int now, bool limit, string curr, const string& target) {
        if (found) return;
        
        if (pos == -1) {
            found = true;
            result = curr;
            return;
        }
        
        int up = limit ? target[target.length()-pos-1] - '0' : 9;
        
        for (int v : e[now]) {
            if (v > up) continue;
            dfs(pos-1, v, limit && v==up, curr + to_string(v), target);
            if (found) return;
        }
    }
    
public:
    Solution() {
        init();
    }
    
    string findMaxNumber(string k) {
        found = false;
        result = "";
        int n = k.length();
        
        // 从最高位开始尝试
        for (int i = k[0]-'0'; i >= 0; i--) {
            dfs(n-2, i, i == k[0]-'0', to_string(i), k);
            if (found) break;
        }
        
        return result;
    }
};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int t;
    cin >> t;
    
    Solution solution;
    while (t--) {
        string k;
        cin >> k;
        cout << solution.findMaxNumber(k) << '\n';
    }
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Solution {
        private List<List<Integer>> e;
        private boolean found;
        private String result;
        
        public Solution() {
            init();
        }
        
        private void init() {
            e = new ArrayList<>(10);
            for (int i = 0; i < 10; i++) {
                e.add(new ArrayList<>());
            }
            
            e.get(1).addAll(Arrays.asList(9,8,7,6,5,4,3,2,1,0));
            e.get(2).addAll(Arrays.asList(9,8,6,5,3,2,0));
            e.get(3).addAll(Arrays.asList(9,6,3));
            e.get(4).addAll(Arrays.asList(9,8,7,6,5,4,0));
            e.get(5).addAll(Arrays.asList(9,8,6,0));
            e.get(6).addAll(Arrays.asList(9,6));
            e.get(7).addAll(Arrays.asList(9,8,7,0));
            e.get(8).addAll(Arrays.asList(9,8,0));
            e.get(9).addAll(Arrays.asList(9));
            e.get(0).addAll(Arrays.asList(0));
        }
        
        private void dfs(int pos, int now, boolean limit, String curr, String target) {
            if (found) return;
            
            if (pos == -1) {
                found = true;
                result = curr;
                return;
            }
            
            int up = limit ? target.charAt(target.length()-pos-1) - '0' : 9;
            
            for (int v : e.get(now)) {
                if (v > up) continue;
                dfs(pos-1, v, limit && v==up, curr + v, target);
                if (found) return;
            }
        }
        
        public String findMaxNumber(String k) {
            found = false;
            result = "";
            int n = k.length();
            
            for (int i = k.charAt(0)-'0'; i >= 0; i--) {
                dfs(n-2, i, i == k.charAt(0)-'0', String.valueOf(i), k);
                if (found) break;
            }
            
            return result;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        
        Solution solution = new Solution();
        while (t-- > 0) {
            String k = sc.next();
            System.out.println(solution.findMaxNumber(k));
        }
    }
}
```

```python []
class Solution:
    def __init__(self):
        self.e = [[] for _ in range(10)]
        self.init_edges()
        self.found = False
        self.result = ""
        
    def init_edges(self):
        self.e[1] = [9,8,7,6,5,4,3,2,1,0]
        self.e[2] = [9,8,6,5,3,2,0]
        self.e[3] = [9,6,3]
        self.e[4] = [9,8,7,6,5,4,0]
        self.e[5] = [9,8,6,0]
        self.e[6] = [9,6]
        self.e[7] = [9,8,7,0]
        self.e[8] = [9,8,0]
        self.e[9] = [9]
        self.e[0] = [0]
    
    def dfs(self, pos: int, now: int, limit: bool, curr: str, target: str):
        if self.found:
            return
            
        if pos == -1:
            self.found = True
            self.result = curr
            return
            
        up = int(target[len(target)-pos-1]) if limit else 9
        
        for v in self.e[now]:
            if v > up:
                continue
            self.dfs(pos-1, v, limit and v==up, curr + str(v), target)
            if self.found:
                return
    
    def find_max_number(self, k: str) -> str:
        self.found = False
        self.result = ""
        n = len(k)
        
        for i in range(int(k[0]), -1, -1):
            self.dfs(n-2, i, i == int(k[0]), str(i), k)
            if self.found:
                break
                
        return self.result

if __name__ == "__main__":
    t = int(input())
    solution = Solution()
    
    for _ in range(t):
        k = input()
        print(solution.find_max_number(k))
```

---

## 算法及复杂度
- 算法：DFS + 记忆化搜索
- 时间复杂度：$\mathcal{O}(10^n)$，其中 $n$为数字长度（实际会因剪枝优化大幅降低）
- 空间复杂度：$\mathcal{O}(n)$，递归栈深度