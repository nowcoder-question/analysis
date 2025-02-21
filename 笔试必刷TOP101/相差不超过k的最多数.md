## 题目
[题目链接](https://www.nowcoder.com/practice/562630ca90ac40ce89443c91060574c6?tpId=308&tqId=2403293&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

### 一、问题分析

1. **题目要求**
   - 从数组中选择数字
   - 任意两数差值不超过 $k$
   - 求最多可选数字个数

2. **关键点**
   - 差值限制意味着选择的数字要在一个范围内
   - 排序后可以简化问题

3. **基本思路**
   1. 先对数组排序
   2. 使用滑动窗口
   3. 窗口内的数字差值都不超过 $k$
   4. 维护最大窗口大小
---
## 代码

```cpp []
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int n, k;
    cin >> n >> k;
    
    vector<int> nums(n);
    for(int i = 0; i < n; i++) {
        cin >> nums[i];
    }
    
    // 排序
    sort(nums.begin(), nums.end());
    
    // 滑动窗口
    int maxCount = 1;
    int left = 0;
    
    for(int right = 1; right < n; right++) {
        // 如果当前窗口的最大差值超过k，移动左指针
        while(left < right && nums[right] - nums[left] > k) {
            left++;
        }
        maxCount = max(maxCount, right - left + 1);
    }
    
    cout << maxCount << endl;
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        
        int[] nums = new int[n];
        for(int i = 0; i < n; i++) {
            nums[i] = sc.nextInt();
        }
        
        // 排序
        Arrays.sort(nums);
        
        // 滑动窗口
        int maxCount = 1;
        int left = 0;
        
        for(int right = 1; right < n; right++) {
            // 如果当前窗口的最大差值超过k，移动左指针
            while(left < right && nums[right] - nums[left] > k) {
                left++;
            }
            maxCount = Math.max(maxCount, right - left + 1);
        }
        
        System.out.println(maxCount);
    }
}
```


```python []
n, k = map(int, input().split())
nums = list(map(int, input().split()))

# 排序
nums.sort()

# 滑动窗口
max_count = 1
left = 0

for right in range(1, n):
    # 如果当前窗口的最大差值超过k，移动左指针
    while left < right and nums[right] - nums[left] > k:
        left += 1
    max_count = max(max_count, right - left + 1)

print(max_count)
```
---
## 算法及复杂度分析
**算法**：滑动窗口，双指针 
**时间复杂度**
   - 排序：$\mathcal{O}(n\log n)$
   - 滑动窗口：$\mathcal{O}(n)$
   - 总体：$\mathcal{O}(n\log n)$

**空间复杂度**
   - $\mathcal{O}(n)$，输入的数组。

## 关键点说明

1. **排序的必要性**
   - 排序后可以保证窗口内的数字连续
   - 只需要比较窗口两端的差值

2. **滑动窗口维护**
   - 右指针不断向右移动
   - 左指针在差值超过 $k$ 时移动
   - 记录最大窗口大小

3. **边界条件处理**
   - 数组长度为 $1$ 的情况
   - 所有数字都可以选择的情况

