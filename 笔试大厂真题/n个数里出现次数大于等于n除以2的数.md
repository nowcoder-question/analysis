## 题目
[题目链接](https://www.nowcoder.com/practice/eac8c671a0c345b38aa0c07aba40097b?tpId=182&tqId=69387&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个查找主要元素（众数）的问题。需要找出数组中出现次数超过一半的数字。可以使用摩尔投票算法。

### 关键点：
1. 使用摩尔投票算法
2. 不需要额外空间统计频次
3. 保证一定存在答案
4. 处理计数过程

### 算法步骤：
1. 初始化候选数和计数器
2. 遍历数组进行投票
3. 返回最终的候选数

---

## 代码

```cpp []
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int solve(vector<int>& nums) {
        // 摩尔投票算法
        int candidate = nums[0];
        int count = 1;
        
        // 投票过程
        for (int i = 1; i < nums.size(); i++) {
            if (count == 0) {
                candidate = nums[i];
                count = 1;
            } else if (nums[i] == candidate) {
                count++;
            } else {
                count--;
            }
        }
        
        return candidate;
    }
};

int main() {
    vector<int> nums;
    int num;
    
    // 读取输入
    while (cin >> num) {
        nums.push_back(num);
    }
    
    Solution solution;
    cout << solution.solve(nums) << endl;
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Solution {
        public int solve(int[] nums) {
            // 摩尔投票算法
            int candidate = nums[0];
            int count = 1;
            
            // 投票过程
            for (int i = 1; i < nums.length; i++) {
                if (count == 0) {
                    candidate = nums[i];
                    count = 1;
                } else if (nums[i] == candidate) {
                    count++;
                } else {
                    count--;
                }
            }
            
            return candidate;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] input = sc.nextLine().split(" ");
        
        int[] nums = new int[input.length];
        for (int i = 0; i < input.length; i++) {
            nums[i] = Integer.parseInt(input[i]);
        }
        
        Solution solution = new Solution();
        System.out.println(solution.solve(nums));
        
        sc.close();
    }
}
```

```python []
class Solution:
    def solve(self, nums):
        # 摩尔投票算法
        candidate = nums[0]
        count = 1
        
        # 投票过程
        for i in range(1, len(nums)):
            if count == 0:
                candidate = nums[i]
                count = 1
            elif nums[i] == candidate:
                count += 1
            else:
                count -= 1
        
        return candidate

# 读取输入
nums = list(map(int, input().split()))
solution = Solution()
print(solution.solve(nums))
```


---

## 算法及复杂度
- 算法：摩尔投票算法
- 时间复杂度：$\mathcal{O(n)}$，只需遍历一次数组
- 空间复杂度：$\mathcal{O(1)}$，只需要常数级额外空间

