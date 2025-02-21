## 题目
[题目链接](https://www.nowcoder.com/practice/a41ac365553e4646aa84fd9b7dad92f9?tpId=182&tqId=327563&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

验证 $\text{UTF-8}$ 编码的有效性，需要按照以下规则检查：

1. **字节长度规则**：
   - 1字节：首位为0
   - 2字节：首字节以110开头，后续1个字节以10开头
   - 3字节：首字节以1110开头，后续2个字节以10开头
   - 4字节：首字节以11110开头，后续3个字节以10开头

2. **检查步骤**：
   - 检查首字节确定字符长度
   - 验证后续字节是否都以10开头
   - 确保数组长度足够包含完整字符

---

## 代码

```c++ []
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
using namespace std;

class Solution {
public:
    bool validUtf8(vector<int>& data) {
        int n = data.size();
        int i = 0;
        
        while (i < n) {
            int bytes = getCharLength(data[i]);
            if (bytes == 0) return false;
            
            // 检查是否有足够的后续字节
            if (i + bytes > n) return false;
            
            // 验证后续字节
            for (int j = 1; j < bytes; j++) {
                if (!isValidContinuation(data[i + j])) {
                    return false;
                }
            }
            
            i += bytes;
        }
        
        return true;
    }
    
private:
    int getCharLength(int byte) {
        if ((byte & 0x80) == 0) return 1;
        else if ((byte & 0xE0) == 0xC0) return 2;
        else if ((byte & 0xF0) == 0xE0) return 3;
        else if ((byte & 0xF8) == 0xF0) return 4;
        return 0;  // 无效的首字节
    }
    
    bool isValidContinuation(int byte) {
        return (byte & 0xC0) == 0x80;
    }
};

int main() {
    string line;
    while (getline(cin, line)) {
        vector<int> data;
        stringstream ss(line);
        string num;
        
        while (getline(ss, num, ',')) {
            data.push_back(stoi(num));
        }
        
        Solution solution;
        cout << (solution.validUtf8(data) ? "true" : "false") << endl;
    }
    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class Solution {
        public boolean validUtf8(int[] data) {
            int n = data.length;
            int i = 0;
            
            while (i < n) {
                int bytes = getCharLength(data[i]);
                if (bytes == 0) return false;
                
                // 检查是否有足够的后续字节
                if (i + bytes > n) return false;
                
                // 验证后续字节
                for (int j = 1; j < bytes; j++) {
                    if (!isValidContinuation(data[i + j])) {
                        return false;
                    }
                }
                
                i += bytes;
            }
            
            return true;
        }
        
        private int getCharLength(int byte1) {
            if ((byte1 & 0x80) == 0) return 1;
            else if ((byte1 & 0xE0) == 0xC0) return 2;
            else if ((byte1 & 0xF0) == 0xE0) return 3;
            else if ((byte1 & 0xF8) == 0xF0) return 4;
            return 0;  // 无效的首字节
        }
        
        private boolean isValidContinuation(int byte1) {
            return (byte1 & 0xC0) == 0x80;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextLine()) {
            String[] nums = sc.nextLine().split(",");
            int[] data = new int[nums.length];
            for (int i = 0; i < nums.length; i++) {
                data[i] = Integer.parseInt(nums[i]);
            }
            
            Solution solution = new Solution();
            System.out.println(solution.validUtf8(data) ? "true" : "false");
        }
    }
}
```

```python []
def get_char_length(byte: int) -> int:
    """获取UTF-8字符的字节长度"""
    if (byte & 0x80) == 0:
        return 1
    elif (byte & 0xE0) == 0xC0:
        return 2
    elif (byte & 0xF0) == 0xE0:
        return 3
    elif (byte & 0xF8) == 0xF0:
        return 4
    return 0  # 无效的首字节

def is_valid_continuation(byte: int) -> bool:
    """检查是否是有效的后续字节"""
    return (byte & 0xC0) == 0x80

def valid_utf8(data: list) -> bool:
    """验证UTF-8编码的有效性"""
    n = len(data)
    i = 0
    
    while i < n:
        bytes_len = get_char_length(data[i])
        if bytes_len == 0:
            return False
        
        # 检查是否有足够的后续字节
        if i + bytes_len > n:
            return False
        
        # 验证后续字节
        for j in range(1, bytes_len):
            if not is_valid_continuation(data[i + j]):
                return False
        
        i += bytes_len
    
    return True

if __name__ == "__main__":
    while True:
        try:
            data = list(map(int, input().split(',')))
            print("true" if valid_utf8(data) else "false")
        except EOFError:
            break
```

---

## 算法及复杂度
- 算法：位运算 + 模拟
- 时间复杂度：$\mathcal{O}(n)$，其中 $n$为数组长度
- 空间复杂度：$\mathcal{O}(1)$，只使用常数额外空间