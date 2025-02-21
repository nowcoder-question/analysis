## 题目
[题目链接](https://www.nowcoder.com/practice/c76408782512486d91eea181107293b6?tpId=295&tqId=1008753&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息:
- 在一个$n*n$的棋盘上要摆放$n$个皇后，求摆的方案数，不同位置就是不同方案数
- 摆放要求：任何两个皇后不同行，不同列也不在同一条斜线上

##### 举一反三：

学习完本题的思路你可以解决如下题目：

[BM55. 没有重复项数字的全排列](https://www.nowcoder.com/practice/4bcf3081067a4d028f95acee3ddcd2b1?tpId=295&sfm=html&channel=nowcoder)

[BM56. 有重复项数字的全排列](https://www.nowcoder.com/practice/a43a2b986ef34843ac4fdd9159b69863?tpId=295&tqId=700)

[BM58. 字符串的排列](https://www.nowcoder.com/practice/fe6b651b66ae47d7acce78ffdd9a96c7?tpId=295&sfm=html&channel=nowcoder)


##### 方法：递归（推荐使用）

**知识点：递归与回溯**

递归是一个过程或函数在其定义或说明中有直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。因此递归过程，最重要的就是查看能不能讲原本的问题分解为更小的子问题，这是使用递归的关键。

如果是线型递归，子问题直接回到父问题不需要回溯，但是如果是树型递归，父问题有很多分支，我需要从子问题回到父问题，进入另一个子问题。因此回溯是指在递归过程中，从某一分支的子问题回到父问题进入父问题的另一子问题分支，因为有时候进入第一个子问题的时候修改过一些变量，因此回溯的时候会要求改回父问题时的样子才能进入第二子问题分支。


n个皇后，不同行不同列，那么肯定棋盘每行都会有一个皇后，每列都会有一个皇后。如果我们确定了第一个皇后的行号与列号，则相当于接下来在$n-1$行中查找$n-1$个皇后，这就是一个子问题，因此使用递归：

- **终止条件：** 当最后一行都被选择了位置，说明n个皇后位置齐了，增加一种方案数返回。
- **返回值：** 每一级要将选中的位置及方案数返回。
- **本级任务：** 每一级其实就是在该行选择一列作为该行皇后的位置，遍历所有的列选择一个符合条件的位置加入数组，然后进入下一级。

**具体做法：**

- step 1：对于第一行，皇后可能出现在该行的任意一列，我们用一个数组pos记录皇后出现的位置，下标为行号，元素值为列号。
- step 2：如果皇后出现在第一列，那么第一行的皇后位置就确定了，接下来递归地在剩余的$n-1$行中找$n-1$个皇后的位置。
- step 3：每个子问题检查是否符合条件，我们可以对比所有已经记录的行，对其记录的列号查看与当前行列号的关系：即是否同行、同列或是同一对角线。

**图示：**

![alt](https://uploadfiles.nowcoder.com/images/20220220/397721558_1645324836478/CAAF9B6E5081EEAD4FFF714ED2F8CBA5)

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    private int res;
    //判断皇后是否符合条件
    public boolean isValid(int[] pos, int row, int col){ 
        //遍历所有已经记录的行
        for(int i = 0; i < row; i++){ 
            //不能同行同列同一斜线
            if(row == i || col == pos[i] || Math.abs(row - i) == Math.abs(col - pos[i])) 
                return false;
        }
        return true;
    }
    
    //递归查找皇后种类
    public void recursion(int n, int row, int[] pos){ 
        //完成全部行都选择了位置
        if(row == n){ 
            res++; 
            return;
        }
        //遍历所有列
        for(int i = 0; i < n; i++){ 
            //检查该位置是否符合条件
            if(isValid(pos, row, i)){ 
                //加入位置
                pos[row] = i; 
                //递归继续查找
                recursion(n, row + 1, pos); 
            }
        }
    }
    public int Nqueen (int n) {
        res = 0;
        //下标为行号，元素为列号，记录皇后位置
        int[] pos = new int[n]; 
        Arrays.fill(pos, 0);
        //递归
        recursion(n, 0, pos); 
        return res; 
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    //判断皇后是否符合条件
    bool isValid(vector<int> &pos, int row, int col){ 
        //遍历所有已经记录的行
        for(int i = 0; i < row; i++){ 
            //不能同行同列同一斜线
            if(row == i || col == pos[i] || abs(row - i) == abs(col - pos[i])) 
                return false;
        }
        return true;
    }
    
    //递归查找皇后种类
    void recursion(int n, int row, vector<int> & pos, int &res){ 
        //完成全部行都选择了位置
        if(row == n){ 
            res++; 
            return;
        }
        //遍历所有列
        for(int i = 0; i < n; i++){ 
            //检查该位置是否符合条件
            if(isValid(pos, row, i)){ 
                //加入位置
                pos[row] = i; 
                //递归继续查找
                recursion(n, row + 1, pos, res); 
            }
        }
    }
    
    int Nqueen(int n) {
        int res = 0;
        //下标为行号，元素为列号，记录皇后位置
        vector<int> pos(n, 0); 
        //递归
        recursion(n, 0, pos, res); 
        return res;
    }
};
```

**Python实现代码：**
```python
class Solution:
    #判断皇后是否符合条件
    def isValid(self, pos: List[int], row:int, col:int): 
        #遍历所有已经记录的行
        for i in range(row): 
            #不能同行同列同一斜线
            if row == i or col == pos[i] or abs(row - i) == abs(col - pos[i]): 
                return False
        return True
    
    #递归查找皇后种类
    def recursion(self, n:int, row:int, pos:List[int], res:int): 
        #完成全部行都选择了位置
        if row == n: 
            res += 1
            return int(res)
        #遍历所有列
        for i in range(n): 
            #检查该位置是否符合条件
            if self.isValid(pos, row, i):  
                #加入位置
                pos[row] = i 
                #递归继续查找
                res = self.recursion(n, row + 1, pos, res) 
        return res

    def Nqueen(self , n: int) -> int:
        res = 0
        #下标为行号，元素为列号，记录皇后位置
        pos = [0] * n 
        #递归
        result = self.recursion(n, 0, pos, res) 
        return result
```

**复杂度分析：**
- 时间复杂度：$O(n*n!)$，isValid函数每次检查复杂度为$O(n)$，递归过程相当于对长度为$n$的数组求全排列，复杂度为$O(n!)$
- 空间复杂度：$O(n)$，辅助数组和栈空间最大为$O(n)$