## 题目
[题目链接](https://www.nowcoder.com/practice/81d2608fdd644982801ae8ce88bd10a9?tpId=182&tqId=176719&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路


### 关键点
1. $dp[i]$ 表示以第 $i$ 个数结尾的 LIS 长度
2. 或使用 tails  数组记录不同长度递增子序列的最小末尾值
3. 注意序列中可能有负数

---

## 代码
```cpp []
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    // O(nlogn)解法
    int lengthOfLIS(vector<int>& nums) {
        vector<int> tails;  // 存储每个长度递增子序列的最小末尾值
        
        for (int num : nums) {
            // 查找第一个大于等于num的位置
            auto it = lower_bound(tails.begin(), tails.end(), num);
            
            if (it == tails.end()) {
                // 如果没找到，说明num可以接在当前最长子序列后面
                tails.push_back(num);
            } else {
                // 找到了，更新该位置的值
                *it = num;
            }
        }
        
        return tails.size();
    }
};

int main() {
    vector<int> nums;
    int num;
    
    // 读取输入序列
    while (cin >> num) {
        nums.push_back(num);
    }
    
    Solution solution;
    cout << solution.lengthOfLIS(nums) << endl;
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Solution {
        // O(nlogn)解法
        public int lengthOfLIS(int[] nums) {
            int[] tails = new int[nums.length];
            int size = 0;
            
            for (int num : nums) {
                int left = 0, right = size;
                // 二分查找第一个大于等于num的位置
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    if (tails[mid] >= num) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                
                tails[left] = num;
                if (left == size) size++;
            }
            
            return size;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        List<Integer> list = new ArrayList<>();
        
        // 读取输入序列
        while (sc.hasNextInt()) {
            list.add(sc.nextInt());
        }
        
        int[] nums = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            nums[i] = list.get(i);
        }
        
        Solution solution = new Solution();
        System.out.println(solution.lengthOfLIS(nums));
        
        sc.close();
    }
}
```

```python []
def length_of_lis(nums):
    # O(nlogn)解法
    tails = []  # 存储每个长度递增子序列的最小末尾值
    
    for num in nums:
        # 二分查找第一个大于等于num的位置
        left, right = 0, len(tails)
        while left < right:
            mid = (left + right) // 2
            if tails[mid] >= num:
                right = mid
            else:
                left = mid + 1
        
        if left == len(tails):
            tails.append(num)
        else:
            tails[left] = num
    
    return len(tails)

def main():
    # 读取输入序列
    nums = list(map(int, input().split()))
    print(length_of_lis(nums))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：贪心 + 二分查找
- 时间复杂度：$\mathcal{O}(n\log n)$，每个数字需要一次二分查找
- 空间复杂度：$\mathcal{O}(n)$，需要额外数组存储 tails

