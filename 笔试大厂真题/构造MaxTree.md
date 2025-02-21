## 题目
[题目链接](https://www.nowcoder.com/practice/a502c7c3c65e41fdaf65eec9e0654dcb?tpId=182&tqId=25277&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个构建最大树的问题。对于每个元素，需要找到其左边和右边第一个比它大的数，取较小值作为父节点。

### 关键点：
1. 使用单调栈找左右第一个大的元素
2. 栈中保持递减顺序
3. 每个元素的父节点是左右第一个大的数中较小的那个
4. 时间复杂度要求 $\mathcal{O}(n)$

### 算法步骤：
1. 使用单调栈找左边第一个大的数
2. 使用单调栈找右边第一个大的数
3. 比较左右两个数，取较小值作为父节点

---

## 代码

```cpp []
class MaxTree {
public:
    vector<int> buildMaxTree(vector<int>& A, int n) {
        vector<int> result(n);
        vector<int> leftBig(n);
        vector<int> rightBig(n);
        stack<int> st;
        
        // 找左边第一个大的数
        for (int i = 0; i < n; i++) {
            while (!st.empty() && A[st.top()] < A[i]) {
                st.pop();
            }
            leftBig[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }
        
        // 清空栈
        while (!st.empty()) st.pop();
        
        // 找右边第一个大的数
        for (int i = n - 1; i >= 0; i--) {
            while (!st.empty() && A[st.top()] < A[i]) {
                st.pop();
            }
            rightBig[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }
        
        // 确定每个节点的父节点
        for (int i = 0; i < n; i++) {
            if (leftBig[i] == -1 && rightBig[i] == -1) {
                result[i] = -1;  // 根节点
            } else if (leftBig[i] == -1) {
                result[i] = rightBig[i];
            } else if (rightBig[i] == -1) {
                result[i] = leftBig[i];
            } else {
                result[i] = A[leftBig[i]] < A[rightBig[i]] ? leftBig[i] : rightBig[i];
            }
        }
        
        return result;
    }
};

```

```java []
import java.util.*;

public class MaxTree {
    public int[] buildMaxTree(int[] A, int n) {
        int[] result = new int[n];
        int[] leftBig = new int[n];
        int[] rightBig = new int[n];
        Stack<Integer> stack = new Stack<>();
        
        // 找左边第一个大的数
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && A[stack.peek()] < A[i]) {
                stack.pop();
            }
            leftBig[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }
        
        // 清空栈
        stack.clear();
        
        // 找右边第一个大的数
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && A[stack.peek()] < A[i]) {
                stack.pop();
            }
            rightBig[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }
        
        // 确定每个节点的父节点
        for (int i = 0; i < n; i++) {
            if (leftBig[i] == -1 && rightBig[i] == -1) {
                result[i] = -1;  // 根节点
            } else if (leftBig[i] == -1) {
                result[i] = rightBig[i];
            } else if (rightBig[i] == -1) {
                result[i] = leftBig[i];
            } else {
                result[i] = A[leftBig[i]] < A[rightBig[i]] ? leftBig[i] : rightBig[i];
            }
        }
        
        return result;
    }
}
```

```python []
class MaxTree: 
    def buildMaxTree(self, A, n):
        result = [0] * n
        left_big = [-1] * n
        right_big = [-1] * n
        stack = []
        
        # Find first bigger element on the left
        for i in range(n):
            while stack and A[stack[-1]] < A[i]:
                stack.pop()
            left_big[i] = stack[-1] if stack else -1
            stack.append(i)
            
        # Clear stack
        stack = []
        
        # Find first bigger element on the right
        for i in range(n-1, -1, -1):
            while stack and A[stack[-1]] < A[i]:
                stack.pop()
            right_big[i] = stack[-1] if stack else -1
            stack.append(i)
            
        # Determine parent for each node
        for i in range(n):
            if left_big[i] == -1 and right_big[i] == -1:
                result[i] = -1  # Root node
            elif left_big[i] == -1:
                result[i] = right_big[i]
            elif right_big[i] == -1:
                result[i] = left_big[i]
            else:
                result[i] = left_big[i] if A[left_big[i]] < A[right_big[i]] else right_big[i]
                
        return result
```

---

## 算法及复杂度
- 算法：单调栈
- 时间复杂度：$\mathcal{O(n)}$，每个元素最多入栈出栈一次
- 空间复杂度：$\mathcal{O(n)}$，需要额外数组存储结果
