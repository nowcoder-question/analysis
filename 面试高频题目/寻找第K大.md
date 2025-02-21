## 题目
[题目链接](https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=196&tqId=44581&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息：
- 利用快速排序的思想寻找数组中的第k大元素
- 有重复数字，不用去重，也不用管稳定性与否

##### 举一反三：

学习完本题的思路你可以解决快速排序或者分治类的问题：

[BM5. 合并k个有序链表](https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&tqId=724)

[BM20. 数组中的逆序对](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=295&tqId=23260)

##### 方法：快排+二分查找（推荐使用）

**知识点：分治**

分治即“分而治之”，“分”指的是将一个大而复杂的问题划分成多个性质相同但是规模更小的子问题，子问题继续按照这样划分，直到问题可以被轻易解决；“治”指的是将子问题单独进行处理。经过分治后的子问题，需要将解进行合并才能得到原问题的解，因此整个分治过程经常用递归来实现。

**思路：**

本题需要使用快速排序的思想，快速排序：每次移动，可以找到一个标杆元素，然后将大于它的移到左边，小于它的移到右边，由此整个数组划分成为两部分，然后分别对左边和右边使用同样的方法进行排序，不断划分左右子段，直到整个数组有序。这也是分治的思想，将数组分化成为子段，分而治之。

放到这道题中，如果标杆元素左边刚好有$k-1$个比它大的，那么该元素就是第$k$大：
```java
//若从0开始，刚好是第K个点，则找到
if(K == j + 1)  
    return a[p];
```
如果它左边的元素比$k - 1$少，说明第$k$大在其右边，直接二分法进入右边，不用管标杆元素左边:
```java
if(j + 1 < k)
    return partition(a, j + 1, high, k);
```

同理如果它右边的元素比$k-1$少，那第$k$大在其左边，右边不用管。

```java
return partition(a, low, j - 1, k);
```

**具体做法：**

- step 1：进行一次快排，大元素在左，小元素在右，得到的标杆j点.在此之前要使用随机数获取标杆元素，防止数据分布导致每次划分不能均衡。
- step 2：如果 j + 1 = k ，那么j点就是第K大。
- step 3：如果 j + 1 > k，则第k大的元素在左半段，更新high = j - 1，执行step 1。
- step 4：如果 j + 1 < k，则第k大的元素在右半段，更新low = j + 1, 再执行step 1.

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220330/397721558_1648640954779/F4E329B8F3EA5F301F7B038444F5A83A)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    //交换函数
    Random r = new Random();
    public static void swap(int arr[], int a, int b) {
		int temp = arr[a];
		arr[a] = arr[b];
		arr[b] = temp;
	}
    public int partition(int[] a, int low, int high, int k){
        //随机快排划分
        int x = Math.abs(r.nextInt()) % (high - low + 1) + low;
        swap(a, low, x);
        int v = a[low];
        int i = low + 1;
        int j = high;
        while(true){
            //小于标杆的在右
            while(j >= low + 1 && a[j] < v) 
                j--;
            //大于标杆的在左
            while(i <= high && a[i] > v) 
                i++;
            if(i > j) 
                break;
            swap(a, i, j);
            i++;
            j--;
        }
        swap(a, low, j);
        //从0开始，所以为第j+1大
        if(j + 1 == k)
            return a[j];
        //继续划分
        else if(j + 1 < k)
            return partition(a, j + 1, high, k);
        else
            return partition(a, low, j - 1, k);
    }
    public int findKth(int[] a, int n, int K) {
        return partition(a, 0, n - 1, K);
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    int partition(vector<int>& a, int low, int high, int k){
        //随机快排划分
        swap(a[low], a[rand() % (high - low + 1) + low]);
        int v = a[low];
        int i = low + 1;
        int j = high;
        while(true){
            //小于标杆的在右
            while(j >= low + 1 && a[j] < v) 
                j--;
            //大于标杆的在左
            while(i <= high && a[i] > v) 
                i++;
            if(i > j) 
                break;
            swap(a[i], a[j]);
            i++;
            j--;
        }
        swap(a[low], a[j]);
        //从0开始，所以为第j+1大
        if(j + 1 == k)
            return a[j];
        //继续划分
        else if(j + 1 < k)
            return partition(a, j + 1, high, k);
        else
            return partition(a, low, j - 1, k);
    }
    int findKth(vector<int> a, int n, int K) {
        return partition(a, 0, n - 1, K);
    }
};
```
**Python代码实现：**
```Python
import random
class Solution:
    def partition(self, a: List[int], low: int, high: int, k: int) -> int:
        #随机快排划分
        x = random.randint(0, 10000)
        a[low], a[x % (high - low + 1) + low] = a[x % (high - low + 1) + low], a[low]
        v = a[low]
        i = low + 1
        j = high
        while True:
            #小于标杆的在右
            while j >= low + 1 and a[j] < v:
                j -= 1
            #大于标杆的在左
            while i <= high and a[i] > v:
                i += 1
            if i > j:
                break
            a[i], a[j] = a[j], a[i]
            i += 1
            j -= 1
        a[low], a[j] = a[j], a[low]
        #从0开始，所以为第j+1大
        if j + 1 == k:
            return a[j]
        #继续划分
        elif j + 1 < k:
            return self.partition(a, j + 1, high, k)
        else:
            return self.partition(a, low, j - 1, k)
    def findKth(self , a: List[int], n: int, K: int) -> int:
        return self.partition(a, 0, n - 1, K)
```

**复杂度分析：**
- 时间复杂度：$O(n)$，利用二分法缩短了时间——$T(2/N)+T(N/4)+T(N/8)+……=T(N)$
- 空间复杂度：$O(n)$，递归栈最大深度