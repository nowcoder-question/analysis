## 题目
[题目链接](https://www.nowcoder.com/practice/a386fd3a5080435dad3252bac76950a7?tpId=182&tqId=25667&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 基本步骤：
   - 统计每个字符串中"Coder"出现的次数
   - 创建包含字符串和次数的数据结构
   - 按照次数降序排序
   - 保持相同次数的相对顺序（稳定排序）

2. 关键点：
   - 不区分大小写的查找
   - 保持原始顺序
   - 返回只包含"Coder"的字符串

---

## 代码

```cpp []
class Coder {
public:
    vector<string> findCoder(vector<string>& A, int n) {
        vector<pair<string, pair<int, int>>> data;  // 字符串，<次数，原始位置>
        
        // 统计每个字符串中"Coder"出现的次数
        for(int i = 0; i < n; i++) {
            int count = countCoder(A[i]);
            if(count > 0) {
                data.push_back({A[i], {count, i}});
            }
        }
        
        // 稳定排序
        stable_sort(data.begin(), data.end(), 
            [](const auto& a, const auto& b) {
                return a.second.first > b.second.first;
            });
        
        // 提取结果
        vector<string> result;
        for(const auto& item : data) {
            result.push_back(item.first);
        }
        
        return result;
    }
    
private:
    int countCoder(const string& s) {
        string str = s;
        // 转换为小写
        transform(str.begin(), str.end(), str.begin(), ::tolower);
        
        int count = 0;
        size_t pos = 0;
        while((pos = str.find("coder", pos)) != string::npos) {
            count++;
            pos++;
        }
        return count;
    }
};
```

```java []
import java.util.*;

public class Coder {
    public String[] findCoder(String[] A, int n) {
        // 创建数组存储字符串和出现次数
        class StringCount implements Comparable<StringCount> {
            String str;
            int count;
            int index;
            
            StringCount(String s, int c, int i) {
                str = s;
                count = c;
                index = i;
            }
            
            @Override
            public int compareTo(StringCount other) {
                if(this.count != other.count) {
                    return other.count - this.count;  // 降序
                }
                return this.index - other.index;  // 保持原顺序
            }
        }
        
        List<StringCount> list = new ArrayList<>();
        
        // 统计每个字符串中"Coder"出现的次数
        for(int i = 0; i < n; i++) {
            int count = countCoder(A[i]);
            if(count > 0) {
                list.add(new StringCount(A[i], count, i));
            }
        }
        
        // 排序
        Collections.sort(list);
        
        // 提取结果
        String[] result = new String[list.size()];
        for(int i = 0; i < list.size(); i++) {
            result[i] = list.get(i).str;
        }
        
        return result;
    }
    
    private int countCoder(String s) {
        String str = s.toLowerCase();
        int count = 0;
        int index = 0;
        
        while((index = str.indexOf("coder", index)) != -1) {
            count++;
            index++;
        }
        
        return count;
    }
}
```

```python []
class Coder:
    def findCoder(self, A, n):
        # Count "Coder" occurrences and store with original index
        counts = []
        for i, s in enumerate(A):
            count = self.countCoder(s)
            if count > 0:
                counts.append((s, count, i))
        
        # Sort by count (descending) and original index
        counts.sort(key=lambda x: (-x[1], x[2]))
        
        # Extract result
        return [x[0] for x in counts]
    
    def countCoder(self, s):
        s = s.lower()
        count = 0
        i = 0
        while i < len(s):
            i = s.find('coder', i)
            if i == -1:
                break
            count += 1
            i += 1
        return count
```

---

## 算法及复杂度
- 算法：字符串匹配 + 稳定排序
- 时间复杂度：$\mathcal{O}(n \cdot m \cdot \log n)$，其中 $n$ 为字符串数量，$m$ 为字符串平均长度
- 空间复杂度：$\mathcal{O}(n)$，用于存储结果数组
