## 题目
[题目链接](https://www.nowcoder.com/practice/41796daa4c7e4e5ab984b2c16c24a1de?tpId=182&tqId=2324921&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

为了找到两个升序数列中第 $K$ 小的值，我们可以使用合并的方法。具体步骤如下：

1. **合并两个数组**：
   - 使用 `merge` 函数将两个升序数组合并成一个新的升序数组。

2. **返回第 K 小的值**：
   - 由于数组是从 0 开始索引的，因此返回合并后数组的第 `target - 1` 个元素。

---

## 代码

```cpp []
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * @param arr1 int整型vector 
     * @param arr2 int整型vector 
     * @param target int整型 
     * @return int整型
     */
    int findKthNum(vector<int>& arr1, vector<int>& arr2, int target) {
        // 合并两个升序数组
        vector<int> merged(arr1.size() + arr2.size());
        merge(arr1.begin(), arr1.end(), arr2.begin(), arr2.end(), merged.begin());
        return merged[target - 1];  // 返回第 K 小的值
    }
};
```

```java []
import java.util.*;
public class Solution {
    public static int findKthNum(int[] arr1, int[] arr2, int target) {
        int[] merged = new int[arr1.length + arr2.length];
        System.arraycopy(arr1, 0, merged, 0, arr1.length);
        System.arraycopy(arr2, 0, merged, arr1.length, arr2.length);
        Arrays.sort(merged);  // 合并并排序
        return merged[target - 1];  // 返回第 K 小的值
    }
}
```

```python []
from typing import List

class Solution:
    def findKthNum(self, arr1: List[int], arr2: List[int], target: int) -> int:
        # 合并两个升序数组
        merged = sorted(arr1 + arr2)
        return merged[target - 1]  # 返回第 K 小的值
```

---

## 算法及复杂度
- 算法：合并排序
- 时间复杂度：$\mathcal{O(n + m \log(n + m))}$，其中n和m分别是两个数组的长度
- 空间复杂度：$\mathcal{O(n + m)}$，用于存储合并后的数组