## 题目
[题目链接](https://www.nowcoder.com/practice/097ab63cffa847d89716f2ca8c23524f?tpId=182&tqId=225528&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道经典的第 $K$ 大数问题，有多种解法：

1. 排序法：
   - 将数组排序后直接返回第 $K$ 大的数
   - 时间复杂度 $\mathcal{O}(n \log n)$
   - 空间复杂度 $\mathcal{O}(1)$

2. 快速选择法（最优解）：
   - 基于快速排序的思想
   - 每次只需要处理一半的数据
   - 时间复杂度 $\mathcal{O}(n)$
   - 空间复杂度 $\mathcal{O}(1)$

3. 堆排序法：
   - 维护一个大小为 $K$ 的小顶堆
   - 时间复杂度 $\mathcal{O}(n \log k)$
   - 空间复杂度 $\mathcal{O}(k)$

---

## 代码

```cpp []
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        return quickSelect(nums, 0, nums.size() - 1, k);
    }
    
private:
    int quickSelect(vector<int>& nums, int left, int right, int k) {
        int pivot = partition(nums, left, right);
        
        // 找到第K大的数
        if (pivot == nums.size() - k) {
            return nums[pivot];
        }
        // 在右半部分查找
        else if (pivot < nums.size() - k) {
            return quickSelect(nums, pivot + 1, right, k);
        }
        // 在左半部分查找
        else {
            return quickSelect(nums, left, pivot - 1, k);
        }
    }
    
    int partition(vector<int>& nums, int left, int right) {
        int pivot = nums[right];
        int i = left;
        
        for (int j = left; j < right; j++) {
            if (nums[j] <= pivot) {
                swap(nums[i], nums[j]);
                i++;
            }
        }
        swap(nums[i], nums[right]);
        return i;
    }
};

int main() {
    int n;
    vector<int> nums;
    int num;
    
    // 读入数组
    while (cin >> num) {
        nums.push_back(num);
        if (cin.get() == '\n') break;
    }
    
    // 读入K
    int k;
    cin >> k;
    
    Solution solution;
    cout << solution.findKthLargest(nums, k) << endl;
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public static int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, k);
    }
    
    private static int quickSelect(int[] nums, int left, int right, int k) {
        int pivot = partition(nums, left, right);
        
        if (pivot == nums.length - k) {
            return nums[pivot];
        }
        else if (pivot < nums.length - k) {
            return quickSelect(nums, pivot + 1, right, k);
        }
        else {
            return quickSelect(nums, left, pivot - 1, k);
        }
    }
    
    private static int partition(int[] nums, int left, int right) {
        int pivot = nums[right];
        int i = left;
        
        for (int j = left; j < right; j++) {
            if (nums[j] <= pivot) {
                swap(nums, i, j);
                i++;
            }
        }
        swap(nums, i, right);
        return i;
    }
    
    private static void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] input = sc.nextLine().split(" ");
        int[] nums = new int[input.length];
        
        for (int i = 0; i < input.length; i++) {
            nums[i] = Integer.parseInt(input[i]);
        }
        
        int k = sc.nextInt();
        System.out.println(findKthLargest(nums, k));
    }
}
```

```python []
def find_kth_largest(nums: list, k: int) -> int:
    def quick_select(left: int, right: int, k: int) -> int:
        pivot = partition(left, right)
        
        if pivot == len(nums) - k:
            return nums[pivot]
        elif pivot < len(nums) - k:
            return quick_select(pivot + 1, right, k)
        else:
            return quick_select(left, pivot - 1, k)
    
    def partition(left: int, right: int) -> int:
        pivot = nums[right]
        i = left
        
        for j in range(left, right):
            if nums[j] <= pivot:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
        
        nums[i], nums[right] = nums[right], nums[i]
        return i
    
    return quick_select(0, len(nums) - 1, k)

def main():
    nums = list(map(int, input().split()))
    k = int(input())
    print(find_kth_largest(nums, k))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：快速选择（Quick Select）
- 时间复杂度：$\mathcal{O}(n)$ - 平均情况
- 空间复杂度：$\mathcal{O}(1)$ - 原地操作
