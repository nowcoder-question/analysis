## 题目
[题目链接](https://www.nowcoder.com/practice/7f24eb7266ce4b0792ce8721d6259800?tpId=182&tqId=58542&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道动态规划题目，主要思路如下：

1. 问题分析：
   - 给定 $n$ 个正整数和目标和 $sum$
   - 从数组中选择若干数字求和
   - 不同位置的相同数字视为不同方案
   - 求所有可能的方案数

2. 解决方案：
   - 使用动态规划
   - $dp[j]$ 表示和为 $j$ 的方案数
   - 对每个数字，更新所有可能的和
   - 从大到小更新避免重复计算

3. 实现细节：
   - 处理数字大于 $sum$ 的情况
   - 使用 `long long` 避免溢出
   - 从后向前更新 $dp$ 数组

---

## 代码

```cpp []
#include <iostream>
#include <vector>
using namespace std;

int main() {
    long long n, sum;
    cin >> n >> sum;
    
    // 读入数组
    vector<long long> a(n);
    for(int i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    // dp[i]表示和为i的方案数
    vector<long long> dp(sum + 1, 0);
    
    // 遍历每个数字
    for(int i = 0; i < n; i++) {
        // 跳过大于sum的数字
        if(a[i] > sum) continue;
        
        // 从大到小更新dp数组
        for(long long j = sum; j > 0; j--) {
            if(dp[j] > 0 && j + a[i] <= sum) {
                dp[j + a[i]] += dp[j];
            }
        }
        // 单独使用当前数字
        dp[a[i]]++;
    }
    
    cout << dp[sum] << endl;
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int sum = sc.nextInt();
        
        // 读入数组
        long[] a = new long[n];
        for(int i = 0; i < n; i++) {
            a[i] = sc.nextLong();
        }
        
        // dp[i]表示和为i的方案数
        long[] dp = new long[sum + 1];
        
        // 遍历每个数字
        for(int i = 0; i < n; i++) {
            if(a[i] > sum) continue;
            
            // 从大到小更新dp数组
            for(int j = sum; j > 0; j--) {
                if(dp[j] > 0 && j + a[i] <= sum) {
                    dp[(int)(j + a[i])] += dp[j];
                }
            }
            dp[(int)a[i]]++;
        }
        
        System.out.println(dp[sum]);
    }
}
```

```python []
def count_combinations(n: int, target: int, nums: list) -> int:
    # dp[i]表示和为i的方案数
    dp = [0] * (target + 1)
    
    # 遍历每个数字
    for num in nums:
        if num > target:
            continue
            
        # 从大到小更新dp数组
        for j in range(target, 0, -1):
            if dp[j] > 0 and j + num <= target:
                dp[j + num] += dp[j]
        dp[num] += 1
    
    return dp[target]

def main():
    n, target = map(int, input().split())
    nums = list(map(int, input().split()))
    print(count_combinations(n, target, nums))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：动态规划
- 时间复杂度：$\mathcal{O}(n \times sum)$ - $n$ 为数组长度，$sum$ 为目标和
- 空间复杂度：$\mathcal{O}(sum)$ - $dp$ 数组空间