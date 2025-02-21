## 题目
[题目链接](https://www.nowcoder.com/practice/181a1a71c7574266ad07f9739f791506?tpId=37&tqId=36889&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个经典的最长公共子串问题，可以使用动态规划来解决：

1. 创建二维dp数组，$dp[i][j]$ 表示以 $str1[i]$ 和 $str2[j]$ 结尾的最长公共子串长度
2. 当 $str1[i] == str2[j]$ 时，$dp[i][j] = dp[i-1][j-1] + 1$
3. 否则 $dp[i][j] = 0$
4. 在填充dp数组的同时，记录最大长度和对应的结束位置
5. 最后根据最大长度和结束位置截取子串

## 代码

```c++ []
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string findLongestCommonSubstring(string str1, string str2) {
    // 确保str1是较短的字符串
    if (str1.length() > str2.length()) {
        swap(str1, str2);
    }
    
    int len1 = str1.length(), len2 = str2.length();
    vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));
    
    int maxLen = 0;
    int endIndex = 0;
    
    // 填充dp数组
    for (int i = 1; i <= len1; i++) {
        for (int j = 1; j <= len2; j++) {
            if (str1[i-1] == str2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
                if (dp[i][j] > maxLen) {
                    maxLen = dp[i][j];
                    endIndex = i - 1;  // 记录在较短串中的位置
                }
            }
        }
    }
    
    return str1.substr(endIndex - maxLen + 1, maxLen);
}

int main() {
    string str1, str2;
    while (cin >> str1 >> str2) {
        cout << findLongestCommonSubstring(str1, str2) << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static String findLongestCommonSubstring(String str1, String str2) {
        // 确保str1是较短的字符串
        if (str1.length() > str2.length()) {
            String temp = str1;
            str1 = str2;
            str2 = temp;
        }
        
        int len1 = str1.length(), len2 = str2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];
        
        int maxLen = 0;
        int endIndex = 0;
        
        // 填充dp数组
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (str1.charAt(i-1) == str2.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                    if (dp[i][j] > maxLen) {
                        maxLen = dp[i][j];
                        endIndex = i - 1;
                    }
                }
            }
        }
        
        return str1.substring(endIndex - maxLen + 1, endIndex + 1);
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str1 = sc.next();
            String str2 = sc.next();
            System.out.println(findLongestCommonSubstring(str1, str2));
        }
    }
}
```

```python []
def find_longest_common_substring(str1, str2):
    # 确保str1是较短的字符串
    if len(str1) > len(str2):
        str1, str2 = str2, str1
        
    len1, len2 = len(str1), len(str2)
    dp = [[0] * (len2 + 1) for _ in range(len1 + 1)]
    
    max_len = 0
    end_index = 0
    
    # 填充dp数组
    for i in range(1, len1 + 1):
        for j in range(1, len2 + 1):
            if str1[i-1] == str2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
                if dp[i][j] > max_len:
                    max_len = dp[i][j]
                    end_index = i - 1
    
    return str1[end_index - max_len + 1:end_index + 1]

while True:
    try:
        str1 = input().strip()
        str2 = input().strip()
        print(find_longest_common_substring(str1, str2))
    except:
        break
```

---

## 算法及复杂度
- 算法：动态规划
- 时间复杂度：$\mathcal{O}(mn)$，其中 $m$ 和 $n$ 是两个字符串的长度
- 空间复杂度：$\mathcal{O}(mn)$，需要一个二维dp数组

这个解法使用动态规划来找出最长公共子串。通过dp数组记录以每对位置结尾的最长公共子串长度，同时记录最大长度和结束位置，最后可以直接截取得到结果。如果有多个最长公共子串，这个算法会返回第一个找到的。
