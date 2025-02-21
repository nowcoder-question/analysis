## 题目
[题目链接](https://www.nowcoder.com/practice/9e7a6bcdbce74feb8013d252d76855da?tpId=182&tqId=25274&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个查找未出现最小正整数的问题。由于数组大小不超过500，所以最小未出现的正整数一定不会超过501。

### 关键点：
1. 最小未出现正整数的范围是 $[1, n+1]$
2. 可以使用原数组作为标记数组
3. 将每个在范围内的正整数放到对应位置

### 算法步骤：
1. 遍历数组，将每个在 $[1, n]$ 范围内的数放到对应位置
2. 再次遍历数组，找到第一个位置 $i$ 上的数不是 $i+1$ 的位置
3. 该位置 $i+1$ 就是最小未出现的正整数

---

## 代码

```cpp []
class ArrayMex {
public:
    int findArrayMex(vector<int>& A, int n) {
        // 将每个数放到正确的位置
        for (int i = 0; i < n; i++) {
            while (A[i] > 0 && A[i] <= n && A[A[i]-1] != A[i]) {
                swap(A[i], A[A[i]-1]);
            }
        }
        
        // 找到第一个不在正确位置的数
        for (int i = 0; i < n; i++) {
            if (A[i] != i + 1) {
                return i + 1;
            }
        }
        
        return n + 1;
    }
};
```

```java []
import java.util.*;

public class ArrayMex {
    public int findArrayMex(int[] A, int n) {
        // 将每个数放到正确的位置
        for (int i = 0; i < n; i++) {
            while (A[i] > 0 && A[i] <= n && A[A[i]-1] != A[i]) {
                int temp = A[i];
                A[i] = A[temp-1];
                A[temp-1] = temp;
            }
        }
        
        // 找到第一个不在正确位置的数
        for (int i = 0; i < n; i++) {
            if (A[i] != i + 1) {
                return i + 1;
            }
        }
        
        return n + 1;
    }
}
```

```python []
class ArrayMex:
    def findArrayMex(self, A, n):
        i = 0
        while i < n:
            if A[i] > 0 and A[i] <= n and A[A[i]-1] != A[i]:
                A[A[i]-1], A[i] = A[i], A[A[i]-1]
            else:
                i += 1
        
        for i in range(n):
            if A[i] != i + 1:
                return i + 1
                
        return n + 1
```

---

## 算法及复杂度
- 算法：原地哈希
- 时间复杂度：$\mathcal{O}(n)$，每个数最多被交换一次
- 空间复杂度：$\mathcal{O}(1)$，只使用常数额外空间
