## 题目
[题目链接](https://www.nowcoder.com/practice/9d4ac9f5eabd46bea7f41b66da9168fe?tpId=182&tqId=169710&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个前缀和与同余的问题。给定一个序列和整数 $K$，需要找到最长的子串，使得其元素和是 $K$ 的倍数。

解决方案：
1. 维护前缀和的余数
2. 使用数组记录每个余数第一次出现的位置
3. 当遇到相同的余数时，计算当前位置与第一次出现位置的差值
4. 更新最大长度

关键点：
- 使用前缀和可以快速计算区间和
- 如果两个前缀和对 $K$ 取余相同，则它们之间的子数组和一定是 $K$ 的倍数
- 需要初始化 $rec[0] = -1$，处理从开始位置就满足条件的情况

---

## 代码

``` cpp []
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <vector>
using namespace std;
const int MAX_SIZE = 100005;

int solve(int n, int k, int nums[]) {
    int ans = 0, cur = 0;
    vector<int> rec(k, 0x3f3f3f3f);
    rec[0] = -1;  // 初始化，处理从起点开始的子数组
    
    // 计算前缀和的余数
    for (int i = 0; i < n; ++i) {
        cur = (cur + nums[i]) % k;
        // 更新最大长度
        ans = max(ans, i - rec[cur]);
        // 记录余数第一次出现的位置
        rec[cur] = min(rec[cur], i);
    }
    
    return ans;
}

int main() {
    int n, k;
    while (scanf("%d", &n) == 1) {
        vector<int> nums(n);
        for (int i = 0; i < n; ++i) {
            scanf("%d", &nums[i]);
        }
        scanf("%d", &k);
        printf("%d\n", solve(n, k, nums.data()));
    }
    return 0;
}
```
``` java []
import java.util.*;

public class Main {
    public static int solve(int n, int k, int[] nums) {
        int ans = 0, cur = 0;
        int[] rec = new int[k];
        Arrays.fill(rec, Integer.MAX_VALUE);
        rec[0] = -1;  // 初始化，处理从起点开始的子数组
        
        // 计算前缀和的余数
        for (int i = 0; i < n; ++i) {
            cur = (cur + nums[i]) % k;
            // 更新最大长度
            ans = Math.max(ans, i - rec[cur]);
            // 记录余数第一次出现的位置
            rec[cur] = Math.min(rec[cur], i);
        }
        
        return ans;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int[] nums = new int[n];
            for (int i = 0; i < n; ++i) {
                nums[i] = sc.nextInt();
            }
            int k = sc.nextInt();
            System.out.println(solve(n, k, nums));
        }
    }
}
```
``` python []
def solve(n, k, nums):
    ans = cur = 0
    # 初始化记录数组，使用字典更方便
    rec = {0: -1}
    
    # 计算前缀和的余数
    for i in range(n):
        cur = (cur + nums[i]) % k
        # 更新最大长度
        if cur in rec:
            ans = max(ans, i - rec[cur])
        else:
            rec[cur] = i
    
    return ans

while True:
    try:
        n = int(input())
        nums = list(map(int, input().split()))
        k = int(input())
        print(solve(n, k, nums))
    except EOFError:
        break
```

---

## 算法及复杂度
- 算法：前缀和 + 同余定理  
- 时间复杂度：$\mathcal{O}(n)$ - 只需要遍历一次数组  
- 空间复杂度：$\mathcal{O}(k)$ - 需要存储 $k$ 个余数的首次出现位置