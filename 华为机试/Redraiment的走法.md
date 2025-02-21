## 题目
[题目链接](https://www.nowcoder.com/practice/24e6243b9f0446b081b1d6d32f2aa3aa?tpId=37&tqId=36927&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个最长递增子序列(LIS)问题。Redraiment从一个位置开始，只能从低处往高处走，求最多能走多少步。

### 关键点
1. 数据范围：
   - 数组长度：$1 \leq n \leq 200$
   - 数组元素：$1 \leq val \leq 350$

2. 问题分析：
   - 从任意位置开始
   - 只能走到比当前位置高的地方
   - 需要找出最长的递增序列长度

---

## 代码

```cpp []
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int getLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);  // dp[i]表示以i结尾的最长递增子序列长度
    int maxLen = 1;
    
    // 对每个位置，检查前面所有较小的数
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
        maxLen = max(maxLen, dp[i]);
    }
    
    return maxLen;
}

int main() {
    int n;
    while (cin >> n) {
        vector<int> heights(n);
        for (int i = 0; i < n; i++) {
            cin >> heights[i];
        }
        
        cout << getLIS(heights) << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static int getLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);  // 初始化dp数组
        int maxLen = 1;
        
        // 对每个位置，检查前面所有较小的数
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxLen = Math.max(maxLen, dp[i]);
        }
        
        return maxLen;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int[] heights = new int[n];
            for (int i = 0; i < n; i++) {
                heights[i] = sc.nextInt();
            }
            
            System.out.println(getLIS(heights));
        }
    }
}
```

```python []
def get_lis(nums):
    n = len(nums)
    dp = [1] * n  # dp[i]表示以i结尾的最长递增子序列长度
    
    # 对每个位置，检查前面所有较小的数
    for i in range(n):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)

while True:
    try:
        n = int(input())
        heights = list(map(int, input().split()))
        print(get_lis(heights))
    except:
        break
```

---

## 算法及复杂度

### 算法分析
1. 动态规划思路：
   - $dp[i]$ 表示以第 $i$ 个数结尾的最长递增子序列长度
   - 对每个位置 $i$，检查前面所有小于 $nums[i]$ 的数 $j$
   - 如果 $nums[i] > nums[j]$，则可以接在 $j$ 后面形成更长的序列

2. 状态转移：
   - $dp[i] = \max(dp[i], dp[j] + 1)$ 其中 $j < i$ 且 $nums[j] < nums[i]$
   - 最终结果是 $dp$ 数组中的最大值

### 复杂度分析
- 时间复杂度：$\mathcal{O}(n^2)$ - 两层循环遍历
- 空间复杂度：$\mathcal{O}(n)$ - 需要 $dp$ 数组存储中间结果
