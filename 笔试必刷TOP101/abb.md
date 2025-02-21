## 题目
[题目链接](https://www.nowcoder.com/practice/0a8bbf8b9b5b4280957849ef4f240f07?tpId=308&tqId=1831946&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

动态规划设计：
   - 状态定义：$dp[i][c]$ 表示处理到第 $i$ 个位置时：
     - $dp[i][0]$：作为 $a$ 的数量
     - $dp[i][1]$：作为 $ab$ 的数量
     - $dp[i][2]$：作为 $abb$ 的数量（最终答案）
   - 状态转移：
     - 对于每个新字符，可以：
       1. 作为 $a$：更新 $dp[i][0]$
       2. 作为 $b$（与前面的 $a$ 配对）：更新 $dp[i][1]$
       3. 作为第二个 $b$（与前面的 $ab$ 配对）：更新 $dp[i][2]$

---

## 代码

```c++ []
#include <iostream>
#include <string>
#include <vector>
using namespace std;

long long countABBSequences(int n, string& s) {
    // dp[i][0]: 以i结尾的a的个数
    // dp[i][1]: 以i结尾的ab的个数
    // dp[i][2]: 以i结尾的abb的个数
    vector<vector<long long>> dp(n, vector<long long>(3, 0));
    
    // 记录每个字符作为a出现的次数
    vector<long long> count_a(26, 0);
    // 记录每个字符作为b出现的次数（与前面的a配对）
    vector<long long> count_ab(26, 0);
    
    for(int i = 0; i < n; i++) {
        int curr = s[i] - 'a';
        
        // 当前字符可以作为a
        dp[i][0] = 1;
        
        // 当前字符作为b，可以和前面所有不同的a配对
        for(int j = 0; j < 26; j++) {
            if(j != curr) {
                dp[i][1] += count_a[j];
            }
        }
        
        // 当前字符作为第二个b，可以和前面所有相同字符的ab配对
        dp[i][2] = count_ab[curr];
        
        // 更新计数
        count_a[curr]++;
        count_ab[curr] += dp[i][1];
    }
    
    // 最终答案是所有位置的abb数量之和
    long long result = 0;
    for(int i = 0; i < n; i++) {
        result += dp[i][2];
    }
    
    return result;
}

int main() {
    int n;
    string s;
    cin >> n >> s;
    cout << countABBSequences(n, s) << endl;
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static long countABBSequences(int n, String s) {
        // dp[i][0]: 以i结尾的a的个数
        // dp[i][1]: 以i结尾的ab的个数
        // dp[i][2]: 以i结尾的abb的个数
        long[][] dp = new long[n][3];
        
        // 记录每个字符作为a出现的次数
        long[] count_a = new long[26];
        // 记录每个字符作为b出现的次数（与前面的a配对）
        long[] count_ab = new long[26];
        
        for(int i = 0; i < n; i++) {
            int curr = s.charAt(i) - 'a';
            
            // 当前字符可以作为a
            dp[i][0] = 1;
            
            // 当前字符作为b，可以和前面所有不同的a配对
            for(int j = 0; j < 26; j++) {
                if(j != curr) {
                    dp[i][1] += count_a[j];
                }
            }
            
            // 当前字符作为第二个b，可以和前面所有相同字符的ab配对
            dp[i][2] = count_ab[curr];
            
            // 更新计数
            count_a[curr]++;
            count_ab[curr] += dp[i][1];
        }
        
        // 最终答案是所有位置的abb数量之和
        long result = 0;
        for(int i = 0; i < n; i++) {
            result += dp[i][2];
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String s = sc.next();
        System.out.println(countABBSequences(n, s));
        sc.close();
    }
}
```

```python []
def count_abb_sequences(n: int, s: str) -> int:
    # dp[i][0]: 以i结尾的a的个数
    # dp[i][1]: 以i结尾的ab的个数
    # dp[i][2]: 以i结尾的abb的个数
    dp = [[0] * 3 for _ in range(n)]
    
    # 记录每个字符作为a出现的次数
    count_a = [0] * 26
    # 记录每个字符作为b出现的次数（与前面的a配对）
    count_ab = [0] * 26
    
    for i in range(n):
        curr = ord(s[i]) - ord('a')
        
        # 当前字符可以作为a
        dp[i][0] = 1
        
        # 当前字符作为b，可以和前面所有不同的a配对
        for j in range(26):
            if j != curr:
                dp[i][1] += count_a[j]
        
        # 当前字符作为第二个b，可以和前面所有相同字符的ab配对
        dp[i][2] = count_ab[curr]
        
        # 更新计数
        count_a[curr] += 1
        count_ab[curr] += dp[i][1]
    
    # 最终答案是所有位置的abb数量之和
    return sum(dp[i][2] for i in range(n))

if __name__ == "__main__":
    n = int(input())
    s = input()
    print(count_abb_sequences(n, s))
```

---

## 算法及复杂度
- 算法：动态规划
- 时间复杂度：$\mathcal{O}(n \times 26)$，其中 $n$ 是字符串长度
- 空间复杂度：$\mathcal{O}(n)$，使用 $dp$ 数组和计数数组
