## 题目
[题目链接](https://www.nowcoder.com/practice/18ecd0ecf5ef4fe9ba3f17f8d00d2d66?tpId=182&tqId=45846&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

要找到将数字 $N$ 转换为Fibonacci数所需的最少步数，我们需要：
1. 生成Fibonacci数列并与 $N$ 比较
2. 找到距离 $N$ 最近的Fibonacci数

### 关键点
1. 由于 $N$ 最大为 $1,000,000$，我们只需要生成到此范围的Fibonacci数
2. 最小步数就是 $N$ 到最近Fibonacci数的距离
3. 只需要保存最后两个Fibonacci数，不需要存储整个数列

---

## 代码
```cpp []
#include <iostream> 
#include <cmath>
using namespace std;

class Solution {
public:
    int minStepsToFib(int n) {
        // 特殊情况处理
        if (n == 0 || n == 1) return 0;
        
        // 只保存最后两个Fibonacci数
        int prev = 0, curr = 1;
        int minSteps = abs(n);  // 初始化为与0的距离
        minSteps = min(minSteps, abs(1 - n));  // 与1的距离
        
        // 生成Fibonacci数并更新最小步数
        while (curr <= n + minSteps) {
            int next = prev + curr;
            if (next > n && abs(next - n) > minSteps) {
                break;  // 提前结束
            }
            
            prev = curr;
            curr = next;
            minSteps = min(minSteps, abs(curr - n));
        }
        
        return minSteps;
    }
};

int main() {
    int n;
    cin >> n;
    
    Solution solution;
    cout << solution.minStepsToFib(n) << endl;
    
    return 0;
}
```

```java []
import java.util.Scanner;

public class Main {
    static class Solution {
        public int minStepsToFib(int n) {
            // 特殊情况处理
            if (n == 0 || n == 1) return 0;
            
            // 只保存最后两个Fibonacci数
            int prev = 0, curr = 1;
            int minSteps = Math.abs(n);  // 初始化为与0的距离
            minSteps = Math.min(minSteps, Math.abs(1 - n));  // 与1的距离
            
            // 生成Fibonacci数并更新最小步数
            while (curr <= n + minSteps) {
                int next = prev + curr;
                if (next > n && Math.abs(next - n) > minSteps) {
                    break;  // 提前结束
                }
                
                prev = curr;
                curr = next;
                minSteps = Math.min(minSteps, Math.abs(curr - n));
            }
            
            return minSteps;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        Solution solution = new Solution();
        System.out.println(solution.minStepsToFib(n));
        
        sc.close();
    }
}
```

```python []
class Solution:
    def min_steps_to_fib(self, n: int) -> int:
        # 特殊情况处理
        if n == 0 or n == 1:
            return 0
        
        # 只保存最后两个Fibonacci数
        prev, curr = 0, 1
        min_steps = abs(n)  # 初始化为与0的距离
        min_steps = min(min_steps, abs(1 - n))  # 与1的距离
        
        # 生成Fibonacci数并更新最小步数
        while curr <= n + min_steps:
            next_num = prev + curr
            if next_num > n and abs(next_num - n) > min_steps:
                break  # 提前结束
            
            prev = curr
            curr = next_num
            min_steps = min(min_steps, abs(curr - n))
        
        return min_steps

def main():
    n = int(input())
    solution = Solution()
    print(solution.min_steps_to_fib(n))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：贪心
- 时间复杂度：$\mathcal{O}(\log n)$，因为Fibonacci数列增长速度很快
- 空间复杂度：$\mathcal{O}(1)$，只使用常数额外空间
