## 题目
[题目链接](https://www.nowcoder.com/practice/cc727473d1e248ccb674eb31bd8683dc?tpId=182&tqId=69386&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一个排序和选择问题。需要从一组数中找出最小的k个数并按升序输出。

### 关键点：
1. 解析输入获取 $k$ 值
2. 对数组进行排序
3. 选择前 $k$ 个数
4. 按升序输出结果

### 算法步骤：
1. 读取输入并分离 $k$ 值
2. 对数组进行排序
3. 输出前 $k$ 个元素

---

## 代码

```cpp []
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    vector<int> solve(vector<int>& nums, int k) {
        // 对数组排序
        sort(nums.begin(), nums.end());
        
        // 返回前k个元素
        return vector<int>(nums.begin(), nums.begin() + k);
    }
};

int main() {
    string line;
    getline(cin, line);
    
    // 解析输入
    stringstream ss(line);
    vector<int> nums;
    int num;
    while (ss >> num) {
        nums.push_back(num);
    }
    
    // 获取k值并从数组中移除
    int k = nums.back();
    nums.pop_back();
    
    Solution solution;
    vector<int> result = solution.solve(nums, k);
    
    // 输出结果
    for (int i = 0; i < k; i++) {
        cout << result[i];
        if (i < k - 1) cout << " ";
    }
    cout << endl;
    
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    static class Solution {
        public int[] solve(int[] nums, int k) {
            // 对数组排序
            Arrays.sort(nums);
            
            // 返回前k个元素
            return Arrays.copyOf(nums, k);
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] input = sc.nextLine().split(" ");
        
        // 解析输入
        int[] nums = new int[input.length - 1];
        for (int i = 0; i < input.length - 1; i++) {
            nums[i] = Integer.parseInt(input[i]);
        }
        
        // 获取k值
        int k = Integer.parseInt(input[input.length - 1]);
        
        Solution solution = new Solution();
        int[] result = solution.solve(nums, k);
        
        // 输出结果
        for (int i = 0; i < k; i++) {
            System.out.print(result[i]);
            if (i < k - 1) System.out.print(" ");
        }
        System.out.println();
        
        sc.close();
    }
}
```
```python []
class Solution:
    def solve(self, nums, k):
        # 对数组排序
        nums.sort()
        # 返回前k个元素
        return nums[:k]

# 读取输入
nums = list(map(int, input().split()))
# 获取k值并从数组中移除
k = nums.pop()

solution = Solution()
result = solution.solve(nums, k)

# 输出结果
print(" ".join(map(str, result)))
```



---

## 算法及复杂度
- 算法：排序
- 时间复杂度：$\mathcal{O(n log n)}$，其中 $n$ 是数组长度
- 空间复杂度：$\mathcal{O(1)}$，不考虑输出数组

