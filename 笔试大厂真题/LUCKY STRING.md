## 题目
[题目链接](https://www.nowcoder.com/practice/9ef1046e746248fe93751e37126bb9e0?tpId=182&tqId=23746&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

要找出所有幸运子串，需要：
1. 生成所有可能的子串
2. 统计每个子串中不同字符的个数
3. 判断不同字符个数是否为斐波那契数
4. 按字典序排序并去重

关键步骤：
1. **预处理斐波那契数列**：
   - 由于字符串长度不超过100，只需要生成较小的斐波那契数
   - 使用set存储便于快速查找

2. **生成子串**：
   - 使用双重循环生成所有可能的子串
   - 使用 $\text{set}$ 去重并保持字典序

3. **判断幸运子串**：
   - 统计子串中不同字符的个数
   - 检查是否为斐波那契数

---

## 代码

```c++ []
#include <iostream>
#include <string>
#include <set>
#include <vector>
#include <unordered_set>
using namespace std;

class Solution {
private:
    set<int> fib;  // 存储斐波那契数
    
    void generateFib() {
        // 生成斐波那契数列
        int a = 1, b = 1;
        fib.insert(a);
        while (b <= 26) {  // 最多26个不同字符
            fib.insert(b);
            int c = a + b;
            a = b;
            b = c;
        }
    }
    
    bool isLucky(const string& s) {
        // 统计不同字符个数
        unordered_set<char> chars(s.begin(), s.end());
        return fib.count(chars.size()) > 0;
    }
    
public:
    vector<string> findLuckySubstrings(string s) {
        generateFib();
        set<string> result;  // 使用set自动去重和排序
        
        // 生成所有子串
        for (int i = 0; i < s.length(); i++) {
            for (int len = 1; len <= s.length() - i; len++) {
                string sub = s.substr(i, len);
                if (isLucky(sub)) {
                    result.insert(sub);
                }
            }
        }
        
        return vector<string>(result.begin(), result.end());
    }
};

int main() {
    string s;
    cin >> s;
    
    Solution solution;
    vector<string> result = solution.findLuckySubstrings(s);
    
    for (const string& str : result) {
        cout << str << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Solution {
        private Set<Integer> fib = new HashSet<>();
        
        private void generateFib() {
            // 生成斐波那契数列
            int a = 1, b = 1;
            fib.add(a);
            while (b <= 26) {  // 最多26个不同字符
                fib.add(b);
                int c = a + b;
                a = b;
                b = c;
            }
        }
        
        private boolean isLucky(String s) {
            // 统计不同字符个数
            Set<Character> chars = new HashSet<>();
            for (char c : s.toCharArray()) {
                chars.add(c);
            }
            return fib.contains(chars.size());
        }
        
        public List<String> findLuckySubstrings(String s) {
            generateFib();
            TreeSet<String> result = new TreeSet<>();  // 使用TreeSet自动排序和去重
            
            // 生成所有子串
            for (int i = 0; i < s.length(); i++) {
                for (int len = 1; len <= s.length() - i; len++) {
                    String sub = s.substring(i, i + len);
                    if (isLucky(sub)) {
                        result.add(sub);
                    }
                }
            }
            
            return new ArrayList<>(result);
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        
        Solution solution = new Solution();
        List<String> result = solution.findLuckySubstrings(s);
        
        for (String str : result) {
            System.out.println(str);
        }
    }
}
```

```python []
def generate_fib():
    """生成斐波那契数列"""
    fib = {1}  # 从1开始
    a, b = 1, 1
    while b <= 26:  # 最多26个不同字符
        fib.add(b)
        a, b = b, a + b
    return fib

def is_lucky(s: str, fib: set) -> bool:
    """判断字符串是否幸运"""
    return len(set(s)) in fib

def find_lucky_substrings(s: str) -> list:
    """找出所有幸运子串"""
    fib = generate_fib()
    result = set()  # 使用set去重
    
    # 生成所有子串
    for i in range(len(s)):
        for j in range(i + 1, len(s) + 1):
            sub = s[i:j]
            if is_lucky(sub, fib):
                result.add(sub)
    
    return sorted(result)  # 按字典序排序

if __name__ == "__main__":
    s = input()
    result = find_lucky_substrings(s)
    for sub in result:
        print(sub)
```

---

## 算法及复杂度
- 算法：枚举 + 集合
- 时间复杂度：$\mathcal{O}(n^2 \cdot l)$，其中 $n$ 是字符串长度，$l$ 是子串的平均长度
- 空间复杂度：$\mathcal{O}(n^2)$，用于存储所有可能的子串