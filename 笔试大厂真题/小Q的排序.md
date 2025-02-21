## 题目
[题目链接](https://www.nowcoder.com/practice/62a677c595fb471e832e4ed4107d3c63?tpId=182&tqId=314248&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 题目要求：
   - 有两种操作：
     1. 将前 $n-1$ 个数排为升序
     2. 将后 $n-1$ 个数排为升序
   - 求最少需要多少次操作使序列变为升序

2. 分析不同情况：
   - 如果序列已经是升序，需要0次操作
   - 如果最小值在开头或最大值在结尾，需要1次操作
   - 如果最小值在结尾且最大值在开头，需要3次操作
   - 其他情况需要2次操作

---

## 代码

```cpp []
#include <iostream>
#include <vector>
using namespace std;

int minOperations(int n, vector<int>& arr) {
    // 检查是否已经是升序
    bool isSorted = true;
    int minVal = arr[0], maxVal = arr[0];
    
    for(int i = 1; i < n; i++) {
        if(arr[i] < arr[i-1]) {
            isSorted = false;
        }
        minVal = min(minVal, arr[i]);
        maxVal = max(maxVal, arr[i]);
    }
    
    if(isSorted) return 0;
    if(minVal == arr[0] || maxVal == arr[n-1]) return 1;
    if(minVal == arr[n-1] && maxVal == arr[0]) return 3;
    return 2;
}

int main() {
    int n;
    cin >> n;
    vector<int> arr(n);
    for(int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    cout << minOperations(n, arr) << endl;
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static int minOperations(int n, int[] arr) {
        // 检查是否已经是升序
        boolean isSorted = true;
        int minVal = arr[0], maxVal = arr[0];
        
        for(int i = 1; i < n; i++) {
            if(arr[i] < arr[i-1]) {
                isSorted = false;
            }
            minVal = Math.min(minVal, arr[i]);
            maxVal = Math.max(maxVal, arr[i]);
        }
        
        if(isSorted) return 0;
        if(minVal == arr[0] || maxVal == arr[n-1]) return 1;
        if(minVal == arr[n-1] && maxVal == arr[0]) return 3;
        return 2;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }
        System.out.println(minOperations(n, arr));
    }
}
```

```python []
def min_operations(n, arr):
    # 检查是否已经是升序
    is_sorted = True
    min_val = max_val = arr[0]
    
    for i in range(1, n):
        if arr[i] < arr[i-1]:
            is_sorted = False
        min_val = min(min_val, arr[i])
        max_val = max(max_val, arr[i])
    
    if is_sorted:
        return 0
    if min_val == arr[0] or max_val == arr[n-1]:
        return 1
    if min_val == arr[n-1] and max_val == arr[0]:
        return 3
    return 2

n = int(input())
arr = list(map(int, input().split()))
print(min_operations(n, arr))
```

---

## 算法及复杂度
- 算法：贪心
- 时间复杂度：$\mathcal{O}(n)$ - 只需要遍历一次数组
- 空间复杂度：$\mathcal{O}(1)$ - 只需要常数级别的额外空间