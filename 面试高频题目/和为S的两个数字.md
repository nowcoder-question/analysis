## 题目
[题目链接](https://www.nowcoder.com/practice/390da4f7a00f44bea7c2f3d19491311b?tpId=196&tqId=23295&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目的主要信息：

- 升序数组中找到和为S的两个数字
- 若有多组，返回任意一组
- 无法找到则返回空数组

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[]()
[]()

##### 方法一：哈希表（推荐使用）
**知识点：哈希表**

哈希表是一种根据关键码（key）直接访问值（value）的一种数据结构。而这种直接访问意味着只要知道key就能在$O(1)$时间内得到value，因此哈希表常用来统计频率、快速检验某个元素是否出现过等。

**思路：**

我们能想到最直观的解法，可能就是两层遍历，将数组所有的二元组合枚举一遍，看看是否是和为目标值，但是这样太费时间了，既然加法这么复杂，我们是不是可以尝试一下减法：对于数组中出现的一个数a，如果目标值减去a的值已经出现过了，那这不就是我们要找的一对元组吗？这种时候，快速找到已经出现过的某个值，可以考虑使用哈希表快速检验某个元素是否出现过这一功能。

**具体做法：**

- step 1：构建一个哈希表，其中key值为遍历数组过程中出现过的值，value值为其相应的下标，因为我们最终要返回的是下标。
- step 2：遍历数组每个元素，如果目标值减去该元素的结果在哈希表中存在，说明我们先前遍历的时候它出现过，根据记录的下标，就可以得到结果。
- step 3：如果相减后的结果没有在哈希表中，说明先前遍历的元素中没有它对应的另一个值，那我们将它加入哈希表，等待后续它匹配的那个值出现即可。

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        //创建哈希表,两元组分别表示值、下标
        HashMap<Integer, Integer> mp = new HashMap<Integer, Integer>();
        //在哈希表中查找target-numbers[i]
        for(int i = 0; i < array.length; i++){
            int temp = sum - array[i];
            //若是没找到，将此信息计入哈希表
            if(!mp.containsKey(temp)){ 
                mp.put(array[i], i);
            }
            else{
                //取出数字添加
                res.add(temp);   
                res.add(array[i]);
                break;
            }
        }
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> res;
        //创建哈希表,两元组分别表示值、下标
        unordered_map<int, int> mp; 
        //在哈希表中查找sum-array[i]
        for(int i = 0; i < array.size(); i++){
            int temp = sum - array[i];
            //若是没找到，将此信息计入哈希表
            if(mp.find(temp) == mp.end()){ 
                mp[array[i]] = i;
            }
            else{
                //取出数字添加
                res.push_back(temp);   
                res.push_back(array[i]);
                break;
            }
        }
        return res;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def FindNumbersWithSum(self , array: List[int], sum: int) -> List[int]:
        res = []
        #创建哈希表,两元组分别表示值、下标
        mp = dict()
        #在哈希表中查找sum-array[i]
        for i in range(len(array)):
            temp = sum - array[i]
            #若是没找到，将此信息计入哈希表
            if temp not in mp: 
                mp[array[i]] = i
            else:
                #取出数字添加
                res.append(temp)   
                res.append(array[i])
                break
        return res
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为数组长度，遍历一次数组
- 空间复杂度：$O(n)$，哈希表最大空间为$n$

##### 方法二：双指针（扩展思路）

**知识点：双指针**

双指针指的是在遍历对象的过程中，不是普通的使用单个指针进行访问，而是使用两个指针（特殊情况甚至可以多个），两个指针或是同方向访问两个链表、或是同方向访问一个链表（快慢指针）、或是相反方向扫描（对撞指针），从而达到我们需要的目的。

**思路：**

这道题目还有一个条件是数组是升序序列，在方法一中没有用到。这个条件有什么用？既然数组是有序的，那我们肯定知道和找到一定程度就不找了，我们为什么要从最小的两个数开始相加呢？我们可以用二分法的思路，从中间开始找。

使用双指针指向数组第一个元素和最后一个元素，然后双指针对撞移动，如果两个指针下的和正好等于目标值sum，那我们肯定找到了，如果和小于sum，说明我们需要找到更大的，那只能增加左边的元素，如果和大于sum，说明我们需要找更小的，只能减小右边的元素。

**具体做法：**

- step 1：准备左右双指针分别指向数组首尾元素。
- step 2：如果两个指针下的和正好等于目标值sum，则找到了所求的两个元素。
- step 3：如果两个指针下的和大于目标值sum，右指针左移；如果两个指针下的和小于目标值sum，左指针右移。
- step 4：当两指针对撞时，还没有找到，就是数组没有。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220420/397721558_1650457554074/06BDB11626D2FD0CA1EBF6C2777FD95C)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        //左右双指针
        int left = 0, right = array.length - 1;
        //对撞双指针
        while(left < right){
            //相加等于sum，找到目标
            if(array[left] + array[right] == sum){
                res.add(array[left]);
                res.add(array[right]);
                break;
            //和太大，缩小右边
            }else if(array[left] + array[right] > sum)
                right--;
            //和太小，扩大左边
            else
                left++;
        }
        return res;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> res;
        //左右双指针
        int left = 0, right = array.size() - 1;
        //对撞双指针
        while(left < right){
            //相加等于sum，找到目标
            if(array[left] + array[right] == sum){
                res.push_back(array[left]);
                res.push_back(array[right]);
                break;
            //和太大，缩小右边
            }else if(array[left] + array[right] > sum)
                right--;
            //和太小，扩大左边
            else
                left++;
        }
        return res;
    }
};
```
**Python实现代码：**
```python
class Solution:
    def FindNumbersWithSum(self , array: List[int], sum: int) -> List[int]:
        res = []
        #左右双指针
        left = 0 
        right = len(array) - 1
        #对撞双指针
        while left < right:
            #相加等于sum，找到目标
            if array[left] + array[right] == sum:
                res.append(array[left])
                res.append(array[right])
                break
            #和太大，缩小右边
            elif array[left] + array[right] > sum:
                right -= 1
            #和太小，扩大左边
            else:
                left += 1
        return res
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为数组长度，左右指针共同遍历一次数组
- 空间复杂度：$O(1)$，常数个变量，无额外辅助空间