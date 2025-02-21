## 题目
[题目链接](https://www.nowcoder.com/practice/d3df40bd23594118b57554129cadf47b?tpId=295&tqId=1499549&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 给定一个元素升序的、无重复数字的整型数组 nums 和一个目标值 target 
- 找到目标值的下标
- 如果找不到返回-1

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM18.二维数组中的查找](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=295&tqId=23256)

[BM19.寻找峰值](https://www.nowcoder.com/practice/fcf87540c4f347bcb4cf720b5b350c76?tpId=295&tqId=2227748)

[BM21.旋转数组](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=295&tqId=23269)

##### 方法：二分法（推荐使用）
**知识点：分治**

分治即“分而治之”，“分”指的是将一个大而复杂的问题划分成多个性质相同但是规模更小的子问题，子问题继续按照这样划分，直到问题可以被轻易解决；“治”指的是将子问题单独进行处理。经过分治后的子问题，需要将解进行合并才能得到原问题的解，因此整个分治过程经常用递归来实现。

**思路：**

本来我们可以遍历数组直接查找，每次检查当前元素是不是要找的值。

```java
for(int i = 0; i < nums.length; i++)
    if(nums[i] == target)
        return i;
```

但是这样这个有序的数组我们就没有完全利用起来。我们想想，若是目标值比较小，肯定在前半区间，若是目标值比较大，肯定在后半区间，怎么评价大小？我们可以用中点值作为一个标杆，将整个数组分为两个区间，目标值与中点值比较就能知道它会在哪个区间，这就是分治的思维。

**具体做法：**
- step 1：从数组首尾开始，每次取中点值。
- step 2：如果中间值等于目标即找到了，可返回下标，如果中点值大于目标，说明中点以后的都大于目标，因此目标在中点左半区间，如果中点值小于目标，则相反。
- step 3：根据比较进入对应的区间，直到区间左右端相遇，意味着没有找到。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20211206/397721558_1638791662895/BD9B94E79756B5639D9B9F3F09A66631)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public int search (int[] nums, int target) {
        int l = 0;
        int r = nums.length - 1;
        //从数组首尾开始，直到二者相遇
        while(l <= r){ 
            //每次检查中点的值
            int m = (l + r) / 2; 
            if(nums[m] == target)
                return m;
            //进入左的区间
            if(nums[m] > target) 
                r = m - 1;
            //进入右区间
            else 
                l = m + 1;
        }
        //未找到
        return -1; 
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0;
        int r = nums.size() - 1;
        //从数组首尾开始，直到二者相遇
        while(l <= r){ 
            //每次检查中点的值
            int m = (l + r) / 2; 
            if(nums[m] == target)
                return m;
            //进入左的区间
            if(nums[m] > target) 
                r = m - 1;
            //进入右区间
            else 
                l = m + 1;
        }
        //未找到
        return -1; 
    }
};
```
**Python代码实现**
```python
class Solution:
    def search(self , nums: List[int], target: int) -> int:
        l = 0
        r = len(nums) - 1
        # 从数组首尾开始，直到二者相遇
        while l <= r :
            # 每次检查中点的值 
            m = int((l+r)/2) 
            if nums[m] == target:
                return m
            # 进入左的区间
            if nums[m] > target: 
                r = m - 1
            # 进入右区间
            else: 
                l = m + 1
        # 未找到
        return -1 
```
**复杂度分析：**
- 时间复杂度：$O(log_2n)$，对长度为$n$的数组进行二分，最坏情况就是取2的对数
- 空间复杂度：$O(1)$，常数级变量，无额外辅助空间