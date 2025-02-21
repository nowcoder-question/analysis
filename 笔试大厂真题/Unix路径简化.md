## 题目
[题目链接](https://www.nowcoder.com/practice/590296f71dba4ba4ae3fc8e375faf689?tpId=182&tqId=161915&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

使用栈来处理路径：
1. 按"/"分割路径
2. 遍历分割后的路径：
   - 遇到".."，弹出栈顶
   - 遇到"."或空字符串，跳过
   - 其他情况，压入栈中
3. 最后用"/"连接栈中元素

---

## 代码
```cpp []
#include <iostream>
#include <string>
#include <vector>
#include <sstream>
using namespace std;

string simplifyPath(string path) {
    // 按"/"分割路径
    vector<string> stack;
    stringstream ss(path);
    string part;
    
    // 使用getline按"/"分割
    while (getline(ss, part, '/')) {
        // 跳过空字符串和"."
        if (part.empty() || part == ".") {
            continue;
        }
        // 遇到".."，弹出栈顶
        else if (part == "..") {
            if (!stack.empty()) {
                stack.pop_back();
            }
        }
        // 其他情况，压入栈中
        else {
            stack.push_back(part);
        }
    }
    
    // 构造结果
    if (stack.empty()) {
        return "/";
    }
    
    string result;
    for (const string& s : stack) {
        result += "/" + s;
    }
    
    return result;
}

int main() {
    string path;
    getline(cin, path);
    cout << simplifyPath(path) << endl;
    return 0;
}
```
```java []
import java.util.*;

public class Main {
    public static String simplifyPath(String path) {
        // 按"/"分割路径
        String[] parts = path.split("/");
        Deque<String> stack = new ArrayDeque<>();
        
        // 处理每个部分
        for (String part : parts) {
            // 跳过空字符串和"."
            if (part.isEmpty() || part.equals(".")) {
                continue;
            }
            // 遇到".."，弹出栈顶
            else if (part.equals("..")) {
                if (!stack.isEmpty()) {
                    stack.pollLast();
                }
            }
            // 其他情况，压入栈中
            else {
                stack.offerLast(part);
            }
        }
        
        // 构造结果
        if (stack.isEmpty()) {
            return "/";
        }
        
        StringBuilder result = new StringBuilder();
        while (!stack.isEmpty()) {
            result.append("/").append(stack.pollFirst());
        }
        
        return result.toString();
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String path = sc.nextLine();
        System.out.println(simplifyPath(path));
    }
}
```


```python []
def simplify_path(path):
    # 按"/"分割路径
    parts = path.split('/')
    stack = []
    
    # 处理每个部分
    for part in parts:
        # 跳过空字符串和"."
        if not part or part == '.':
            continue
        # 遇到".."，弹出栈顶
        elif part == '..':
            if stack:
                stack.pop()
        # 其他情况，压入栈中
        else:
            stack.append(part)
    
    # 构造结果
    return '/' + '/'.join(stack)

def main():
    path = input().strip()
    print(simplify_path(path))

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：栈
- 时间复杂度：$\mathcal{O}(n)$，其中 $n$ 是路径长度
- 空间复杂度：$\mathcal{O}(n)$，用于存储栈