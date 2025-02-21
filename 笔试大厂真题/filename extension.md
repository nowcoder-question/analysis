## 题目
[题目链接](https://www.nowcoder.com/practice/7eb53c86e50845f6a2eafe7ea0fe9ef9?tpId=182&tqId=105627&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 基本思路：
   - 从路径字符串中找到最后一个点号位置
   - 提取点号后面的所有字符作为扩展名
   - 处理特殊情况（无扩展名）

2. 实现方法：
   - 使用字符串的lastIndexOf或rfind方法
   - 使用字符串切片获取扩展名
   - 处理边界条件

---

## 代码

```cpp []
#include<bits/stdc++.h>
using namespace std;
class Solution {
public:
    string getExtension(string path) {
        int dotPos = path.rfind('.');
        if (dotPos == string::npos || dotPos == path.length() - 1) {
            return "null";
        }
        return path.substr(dotPos + 1);
    }
};

int main() {
    string path;
    getline(cin, path);
    
    Solution solution;
    cout << solution.getExtension(path) << endl;
    
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    public String getExtension(String path) {
        int dotPos = path.lastIndexOf('.');
        if (dotPos == -1 || dotPos == path.length() - 1) {
            return "null";
        }
        return path.substring(dotPos + 1);
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String path = sc.nextLine();
        
        Main solution = new Main();
        System.out.println(solution.getExtension(path));
    }
}
```

```python []
class Solution:
    def getExtension(self, path):
        dotPos = path.rfind('.')
        if dotPos == -1 or dotPos == len(path) - 1:
            return "null"
        return path[dotPos + 1:]

if __name__ == "__main__":
    path = input().strip()
    solution = Solution()
    print(solution.getExtension(path))
```

---

## 算法及复杂度
- 算法：字符串处理
- 时间复杂度：$\mathcal{O}(n)$，其中 $n$为字符串长度
- 空间复杂度：$\mathcal{O}(1)$，只使用常数额外空间