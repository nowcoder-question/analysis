## 题目
[题目链接](https://www.nowcoder.com/practice/512619bee5874e85bd2812a0c9066125?tpId=308&tqId=270516&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 状态定义：
   - $dp[i][j]$ 表示区间 $[i,j]$ 染色需要的最少次数

2. 转移方程：
   - 当 $s[i] == s[j]$ 时：$dp[i][j] = \min(dp[i+1][j], dp[i][j-1])$
   - 当 $s[i] != s[j]$ 时：$dp[i][j] = \min(dp[i][j], dp[i][k] + dp[k+1][j])$

3. 实现要点：
   - 字符串下标从 $1$ 开始，方便处理
   - 初始化 $dp$ 数组为较大值
   - 长度为 $1$ 的区间初始化为 $1$

---

## 代码

```c++ []
#include <bits/stdc++.h>
using namespace std;

int dp[51][51];

int main() {
    string s;
    cin >> s;
    int n = s.size();
    
    // 将字符串下标从1开始
    s = " " + s;
    
    // 初始化dp数组为较大值
    memset(dp, 0x3f, sizeof dp);
    
    // 初始化长度为1的区间
    for(int i = 1; i <= n; i++) {
        dp[i][i] = 1;
    }
    
    // 枚举区间长度
    for(int len = 2; len <= n; len++) {
        // 枚举左端点
        for(int i = 1; i + len - 1 <= n; i++) {
            int j = i + len - 1;  // 右端点
            
            // 如果区间两端颜色相同
            if(s[i] == s[j]) {
                dp[i][j] = min(dp[i+1][j], dp[i][j-1]);
            } else {
                // 枚举分割点
                for(int k = i; k < j; k++) {
                    dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j]);
                }
            }
        }
    }
    
    cout << dp[1][n];
    return 0;
}
```


```java []
import java.util.Scanner;
import java.util.Arrays;

public class Main {
    static int[][] dp = new int[51][51];
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        int n = s.length();
        
        // 将字符串下标从1开始
        s = " " + s;
        
        // 初始化dp数组为较大值
        for(int[] row : dp) {
            Arrays.fill(row, 0x3f3f3f3f);
        }
        
        // 初始化长度为1的区间
        for(int i = 1; i <= n; i++) {
            dp[i][i] = 1;
        }
        
        // 枚举区间长度
        for(int len = 2; len <= n; len++) {
            // 枚举左端点
            for(int i = 1; i + len - 1 <= n; i++) {
                int j = i + len - 1;  // 右端点
                
                // 如果区间两端颜色相同
                if(s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = Math.min(dp[i+1][j], dp[i][j-1]);
                } else {
                    // 枚举分割点
                    for(int k = i; k < j; k++) {
                        dp[i][j] = Math.min(dp[i][j], dp[i][k] + dp[k+1][j]);
                    }
                }
            }
        }
        
        System.out.println(dp[1][n]);
        sc.close();
    }
}
```


```python []
def solve(s: str) -> int:
    n = len(s)
    # 将字符串下标从1开始
    s = " " + s
    
    # 初始化dp数组
    dp = [[float('inf')] * (n + 1) for _ in range(n + 1)]
    
    # 初始化长度为1的区间
    for i in range(1, n + 1):
        dp[i][i] = 1
    
    # 枚举区间长度
    for length in range(2, n + 1):
        # 枚举左端点
        for i in range(1, n - length + 2):
            j = i + length - 1  # 右端点
            
            # 如果区间两端颜色相同
            if s[i] == s[j]:
                dp[i][j] = min(dp[i+1][j], dp[i][j-1])
            else:
                # 枚举分割点
                for k in range(i, j):
                    dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j])
    
    return dp[1][n]

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：区间动态规划
- 时间复杂度：$\mathcal{O}(n^3)$，其中 $n$ 是字符串长度
- 空间复杂度：$\mathcal{O}(n^2)$，用于存储 $dp$ 数组
