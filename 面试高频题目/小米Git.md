## 题目
[题目链接](https://www.nowcoder.com/practice/4fcd94851d9142458329fd1d3e5802a8?tpId=196&tqId=2324684&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道查找Git版本树中最近公共祖先的题目，主要思路如下：

1. 问题分析：
   - 使用邻接矩阵表示Git版本树
   - 需要找到两个版本的最近公共祖先
   - 树的根节点为0
   - 不保证是二叉树

2. 解决方案：
   - 使用DFS遍历树
   - 记录已访问节点避免环
   - 从根节点开始查找目标版本
   - 当找到两个版本时，当前节点即为最近公共祖先

3. 返回值处理：
   - 找到目标版本返回版本号
   - 未找到返回-1
   - 找到两个版本返回当前节点

---

## 代码

```cpp []
class Solution {
public:
    vector<int> visited;
    
    int Git(vector<string>& matrix, int versionA, int versionB) {
        int n = matrix.size();
        visited.resize(n, 0);
        return traverse(matrix, versionA, versionB, 0);
    }
    
    int traverse(vector<string>& matrix, int versionA, int versionB, int index) {
        // 找到目标版本
        if (index == versionA || index == versionB) {
            return index;
        }
        
        visited[index] = 1;  // 标记已访问
        vector<int> found;   // 存储找到的版本
        
        // 遍历所有相邻节点
        for (int i = 0; i < matrix.size(); i++) {
            if (matrix[index][i] == '1' && !visited[i]) {
                int result = traverse(matrix, versionA, versionB, i);
                if (result != -1) {
                    found.push_back(result);
                }
                if (found.size() > 1) break;  // 找到两个版本就停止
            }
        }
        
        // 处理返回值
        if (found.empty()) return -1;         // 未找到任何版本
        if (found.size() == 1) return found[0];  // 找到一个版本
        return index;  // 找到两个版本，当前节点为最近公共祖先
    }
};
```

```java []
import java.util.*;
public class Solution {
    private boolean[] visited;
    
    public int Git(String[] matrix, int versionA, int versionB) {
        int n = matrix.length;
        visited = new boolean[n];
        return traverse(matrix, versionA, versionB, 0);
    }
    
    private int traverse(String[] matrix, int versionA, int versionB, int index) {
        if (index == versionA || index == versionB) {
            return index;
        }
        
        visited[index] = true;
        List<Integer> found = new ArrayList<>();
        
        for (int i = 0; i < matrix.length; i++) {
            if (matrix[index].charAt(i) == '1' && !visited[i]) {
                int result = traverse(matrix, versionA, versionB, i);
                if (result != -1) {
                    found.add(result);
                }
                if (found.size() > 1) break;
            }
        }
        
        if (found.isEmpty()) return -1;
        if (found.size() == 1) return found.get(0);
        return index;
    }
}
```

```python []
class Solution:
    def Git(self, matrix, versionA, versionB):
        n = len(matrix)
        self.visited = [False] * n
        return self.traverse(matrix, versionA, versionB, 0)
    
    def traverse(self, matrix, versionA, versionB, index):
        # 找到目标版本
        if index == versionA or index == versionB:
            return index
            
        self.visited[index] = True
        found = []
        
        # 遍历相邻节点
        for i in range(len(matrix)):
            if matrix[index][i] == '1' and not self.visited[i]:
                result = self.traverse(matrix, versionA, versionB, i)
                if result != -1:
                    found.append(result)
                if len(found) > 1:
                    break
        
        # 处理返回值
        if not found:
            return -1
        if len(found) == 1:
            return found[0]
        return index
```

---

## 算法及复杂度
- 算法：深度优先搜索(DFS)
- 时间复杂度：$\mathcal{O}(n^2)$ - 需要遍历邻接矩阵
- 空间复杂度：$\mathcal{O}(n)$ - 需要 `visited` 数组和递归栈空间
