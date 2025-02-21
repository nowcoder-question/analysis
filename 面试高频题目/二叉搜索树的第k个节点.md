## 题目
[题目链接](https://www.nowcoder.com/practice/57aa0bab91884a10b5136ca2c087f8ff?tpId=196&tqId=2305268&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

##### 题目主要信息：
- 给定一棵节点数为n二叉搜索树，需要其中的第k小的节点值
- 返回第k小的节点值即可
- 不能查找的情况，如二叉树为空，则返回-1，或者k大于n等等，也返回-1
- 保证n个节点的值不一样

##### 举一反三：
学习完本题的思路你可以解决如下题目：

[JZ68. 二叉搜索树的最近公共祖先](https://www.nowcoder.com/practice/d9820119321945f588ed6a26f0a6991f?tpId=13&tqId=2290592)

[JZ8. 二叉树的下一个节点](https://www.nowcoder.com/practice/9023a0c988684a53960365b889ceaf5e?tpId=13&tqId=23451)

##### 方法一：递归中序遍历（推荐使用）

**知识点：二叉树递归**

递归是一个过程或函数在其定义或说明中有直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。因此递归过程，最重要的就是查看能不能讲原本的问题分解为更小的子问题，这是使用递归的关键。

而二叉树的递归，则是将某个节点的左子树、右子树看成一颗完整的树，那么对于子树的访问或者操作就是对于原树的访问或者操作的子问题，因此可以自我调用函数不断进入子树。

**思路：**

根据二叉搜索树的性质，左子树的元素都小于根节点，右子树的元素都大于根节点。因此它的中序遍历（左中右）序列正好是由小到大的次序，因此我们可以尝试递归中序遍历，也就是从最小的一个节点开始，找到k个就是我们要找的目标。

**具体做法：**

- step 1：设置全局变量count记录遍历了多少个节点，res记录第k个节点。
- step 2：另写一函数进行递归中序遍历，当节点为空或者超过k时，结束递归，返回。
- step 3：优先访问左子树，再访问根节点，访问时统计数字，等于k则找到。
- step 4：最后访问右子树。

**图示：**

![图片说明](https://uploadfiles.nowcoder.com/images/20210715/397721558_1626280110464/AD840828372A5B112A2F2C26B88D4243 "图片标题") 

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    //记录返回的节点
    private TreeNode res = null;
    //记录中序遍历了多少个
    private int count = 0;
    public void midOrder(TreeNode root, int k){
        //当遍历到节点为空或者超过k时，返回
        if(root == null || count > k) 
            return;
        midOrder(root.left, k);
        count++;
        //只记录第k个
        if(count == k)  
            res = root;
        midOrder(root.right, k);
    }
    public int KthNode (TreeNode proot, int k) {
        midOrder(proot, k);
        if(res != null)
            return res.val;
        //二叉树为空，或是找不到
        else 
            return -1;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    //记录返回的节点
    TreeNode* res = NULL;
    //记录中序遍历了多少个
    int count = 0;
    //当遍历到节点为空或者超过k时，返回
    void midOrder(TreeNode* root, int k){
        if(root == NULL || count > k) 
            return;
        midOrder(root->left, k);
        count++;
        //只记录第k个
        if(count == k)  
            res = root;
        midOrder(root->right, k);
    }
    
    int KthNode(TreeNode* proot, int k) {
        midOrder(proot, k);
        if(res)
            return res->val;
        //二叉树为空，或是找不到
        else 
            return -1;
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def __init__(self):
        #记录返回的节点
        self.res = None 
        #记录中序遍历了多少个
        self.count = 0 
        
    def midOrder(self, root, k):
        #当遍历到节点为空或者超过k时，返回
        if not root or self.count > k: 
            return
        self.midOrder(root.left, k)
        self.count += 1
        #只记录第k个
        if self.count == k:  
            self.res = root
        self.midOrder(root.right, k)

    def KthNode(self , proot: TreeNode, k: int) -> int:
        self.midOrder(proot, k)
        if self.res:
            return self.res.val
        #二叉树为空，或是找不到
        else: 
            return -1
```
**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为二叉树的节点数，所有节点遍历一遍
- 空间复杂度：$O(n)$，栈空间最大深度为二叉树退化为链表，长度为$n$

##### 方法二：非递归中序遍历
**知识点：栈**

栈是一种仅支持在表尾进行插入和删除操作的线性表，这一端被称为栈顶，另一端被称为栈底。元素入栈指的是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；元素出栈指的是从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。

**思路：**

递归实际上就是一种先进后出的栈结构，因此能用递归进行的中序遍历，非递归（栈）也可以实现，还是需要记录遍历到第k位即截止。

**具体做法：**

- step 1：用栈记录当前节点，不断往左深入，直到左边子树为空。
- step 2：再弹出栈顶（即为当前子树的父节点），访问该节点，同时计数。
- step 3：然后再访问其右子树，其中每棵子树都遵循左中右的次序。
- step 4：直到第k个节点返回，如果遍历结束也没找到，则返回-1.

**Java实现代码：**
```java
import java.util.*;
public class Solution {
    public int KthNode (TreeNode proot, int k) {
        if(proot == null)
            return -1;
        //记录遍历了多少个数
        int count = 0;  
        TreeNode p = null;
        //用栈辅助建立中序
        Stack<TreeNode> s = new Stack<TreeNode>();   
        while(!s.isEmpty() || proot != null){
            while (proot != null) {
                s.push(proot);
                //中序遍历每棵子树从最左开始
                proot = proot.left;   
            }
            p = s.pop();
            count++;
            //第k个直接返回
            if(count == k) 
                return p.val;
            proot = p.right;
        }
        //没有找到
        return -1;
    }
}
```
**C++实现代码：**
```cpp
class Solution {
public:
    int KthNode(TreeNode* proot, int k) {
        if(proot == NULL)
            return -1;
        //记录遍历了多少个数
        int count = 0;  
        TreeNode* p = NULL;
        //用栈辅助建立中序
        stack<TreeNode*> s;   
        while(!s.empty() || proot != NULL){
            while (proot != NULL) {
                s.push(proot);
                //中序遍历每棵子树从最左开始
                proot = proot->left;   
            }
            p = s.top();
            s.pop();
            count++;
            //第k个直接返回
            if(count == k)
                return p->val;
            proot = p->right;
        }
        //没有找到
        return -1;
    }
};
```
**Python代码实现：**
```Python
class Solution:
    def KthNode(self , proot: TreeNode, k: int) -> int:
        if not proot:
            return -1
        #记录遍历了多少个数
        count = 0  
        p = None
        #用栈辅助建立中序
        s = []  
        while (len(s) != 0) or proot is not None:
            while proot is not None:
                s.append(proot)
                #中序遍历每棵子树从最左开始
                proot = proot.left   
            p = s[-1]
            s.pop()
            count += 1
            #第k个直接返回
            if count == k:  
                return p.val
            proot = p.right
        #没有找到
        return -1 
```

**复杂度分析：**
- 时间复杂度：$O(n)$，其中$n$为二叉树的节点数，所有节点遍历一遍
- 空间复杂度：$O(n)$，栈空间最大值为二叉树退化为链表