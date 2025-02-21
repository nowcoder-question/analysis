## 题目
[题目链接](https://www.nowcoder.com/practice/ec2575fb877d41c9a33d9bab2694ba47?tpId=182&tqId=25271&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个数组处理问题，需要找出数组中最小的 $k$ 个数，并保持原数组中的相对顺序。

### 关键点：
1. 不能直接排序，因为需要保持原数组顺序
2. 可以使用临时数组存储原数组的副本并排序
3. 找出第 $k$ 小的数作为阈值
4. 遍历原数组，保留小于等于阈值的前 $k$ 个数

### 算法步骤：
1. 复制原数组并排序，找到第 $k$ 小的数
2. 遍历原数组，将小于等于阈值的数加入结果数组
3. 如果结果数组长度不足 $k$，继续添加等于阈值的数直到达到 $k$ 个

---

## 代码

```cpp []
class KthNumbers {
public:
    vector<int> findKthNumbers(vector<int>& A, int n, int k) {
        if (k <= 0 || n <= 0) return vector<int>();
        
        // 复制数组并排序
        vector<int> sorted = A;
        sort(sorted.begin(), sorted.end());
        
        // 获取第k小的数
        int kthValue = sorted[k - 1];
        
        // 遍历原数组，保留小于等于kthValue的前k个数
        vector<int> result;
        for (int i = 0; i < n && result.size() < k; i++) {
            if (A[i] <= kthValue) {
                result.push_back(A[i]);
            }
        }
        
        return result;
    }
};
```

```java []
import java.util.*;

public class KthNumbers {
    public int[] findKthNumbers(int[] A, int n, int k) {
        if (k <= 0 || n <= 0) return new int[0];
        
        // 复制数组并排序
        int[] sorted = A.clone();
        Arrays.sort(sorted);
        
        // 获取第k小的数
        int kthValue = sorted[k - 1];
        
        // 遍历原数组，保留小于等于kthValue的前k个数
        int[] result = new int[k];
        int index = 0;
        
        for (int i = 0; i < n && index < k; i++) {
            if (A[i] <= kthValue) {
                result[index++] = A[i];
            }
        }
        
        return result;
    }
}
```

```python []
class KthNumbers:
    def findKthNumbers(self, A, n, k):
        if k <= 0 or n <= 0:
            return []
            
        sorted_arr = sorted(A)
        kth_value = sorted_arr[k - 1]
        
        result = []
        for num in A:
            if len(result) < k and num <= kth_value:
                result.append(num)
                
        return result

```

---

## 算法及复杂度
- 算法：排序 + 遍历
- 时间复杂度：$\mathcal{O(n \log n)}$，主要来自排序操作
- 空间复杂度：$\mathcal{O(n)}$，需要存储排序后的数组副本
