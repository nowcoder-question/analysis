## 题目
[题目链接](https://www.nowcoder.com/practice/79358002fe7c48a6a74f96c0dc734fa1?tpId=182&tqId=161721&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

正确思路：
1. 先处理序列开头部分：
   - 先放入 $ivec[0]$ 个 $ivec[0]$
   - 如果 $ivec[0]=1$，还要放入 $ivec[1]$ 个 $ivec[1]$
2. 然后根据已生成序列中的数字继续构造：
   - 读取序列中的数字作为重复次数
   - 使用ivec中循环的数字进行填充

---

## 代码

```c++ []
#include <iostream>
#include <vector>
using namespace std;

vector<int> generateKolakoski(int n, int m, const vector<int>& nums) {
    // 结果序列
    vector<int> result;
    int readPos = 1;    // 读取位置
    int numPos = 1;     // nums数组的当前位置
    
    // 1. 处理开头特殊情况
    // 先添加nums[0]个nums[0]
    for (int i = 0; i < nums[0]; i++) {
        result.push_back(nums[0]);
        // 如果第一个数是1，需要特殊处理
        if (nums[0] == 1) {
            // 添加nums[1]个nums[1]
            for (int j = 0; j < nums[1]; j++) {
                result.push_back(nums[1]);
            }
            numPos = (numPos + 1) % m;
            readPos++;
        }
    }
    
    // 2. 继续生成序列直到达到所需长度
    while (result.size() < n) {
        // 获取当前组应重复的次数
        int repeatCount = result[readPos];
        // 添加指定次数的当前数字
        for (int i = 0; i < repeatCount && result.size() < n; i++) {
            result.push_back(nums[numPos]);
        }
        // 更新位置
        numPos = (numPos + 1) % m;
        readPos++;
    }
    
    return result;
}

int main() {
    // 读取输入
    int n, m;
    cin >> n >> m;
    vector<int> nums(m);
    for (int i = 0; i < m; i++) {
        cin >> nums[i];
    }
    
    // 生成序列
    vector<int> result = generateKolakoski(n, m, nums);
    
    // 输出结果
    for (int i = 0; i < n; i++) {
        cout << result[i] << endl;
    }
    
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    private static List<Integer> generateKolakoski(int n, int m, int[] nums) {
        // 结果序列
        List<Integer> result = new ArrayList<>();
        int readPos = 1;    // 读取位置
        int numPos = 1;     // nums数组的当前位置
        
        // 1. 处理开头特殊情况
        // 先添加nums[0]个nums[0]
        for (int i = 0; i < nums[0]; i++) {
            result.add(nums[0]);
            // 如果第一个数是1，需要特殊处理
            if (nums[0] == 1) {
                // 添加nums[1]个nums[1]
                for (int j = 0; j < nums[1]; j++) {
                    result.add(nums[1]);
                }
                numPos = (numPos + 1) % m;
                readPos++;
            }
        }
        
        // 2. 继续生成序列直到达到所需长度
        while (result.size() < n) {
            // 获取当前组应重复的次数
            int repeatCount = result.get(readPos);
            // 添加指定次数的当前数字
            for (int i = 0; i < repeatCount && result.size() < n; i++) {
                result.add(nums[numPos]);
            }
            // 更新位置
            numPos = (numPos + 1) % m;
            readPos++;
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        // 读取输入
        int n = sc.nextInt();
        int m = sc.nextInt();
        int[] nums = new int[m];
        for (int i = 0; i < m; i++) {
            nums[i] = sc.nextInt();
        }
        
        // 生成序列
        List<Integer> result = generateKolakoski(n, m, nums);
        
        // 输出结果
        for (int i = 0; i < n; i++) {
            System.out.println(result.get(i));
        }
    }
}
```
```python []
def generate_kolakoski(n, m, nums):
    # 结果序列
    result = []
    read_pos = 1    # 读取位置
    num_pos = 1     # nums数组的当前位置
    
    # 1. 处理开头特殊情况
    # 先添加nums[0]个nums[0]
    for _ in range(nums[0]):
        result.append(nums[0])
        # 如果第一个数是1，需要特殊处理
        if nums[0] == 1:
            # 添加nums[1]个nums[1]
            for _ in range(nums[1]):
                result.append(nums[1])
            num_pos = (num_pos + 1) % m
            read_pos += 1
    
    # 2. 继续生成序列直到达到所需长度
    while len(result) < n:
        # 获取当前组应重复的次数
        repeat_count = result[read_pos]
        # 添加指定次数的当前数字
        for _ in range(repeat_count):
            if len(result) < n:
                result.append(nums[num_pos])
        # 更新位置
        num_pos = (num_pos + 1) % m
        read_pos += 1
    
    return result

def main():
    # 读取输入
    n, m = map(int, input().split())
    nums = list(map(int, input().split()))
    
    # 生成序列
    result = generate_kolakoski(n, m, nums)
    
    # 输出结果
    for num in result[:n]:
        print(num)

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：模拟
- 时间复杂度：$\mathcal{O}(n)$，其中 $n$ 是需要生成的序列长度
- 空间复杂度：$\mathcal{O}(n)$，用于存储生成的序列
