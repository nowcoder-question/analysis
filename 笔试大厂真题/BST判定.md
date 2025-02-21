## 题目
[题目链接](https://www.nowcoder.com/practice/d7a2a2bc00544dbfa71cdef591354c9a?tpId=182&tqId=314241&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

1. 基本思路：
   - 使用递归方法遍历二叉树，检查每个节点是否满足二分查找树的性质。
   - 对于每个节点，确保其左子树的所有节点值小于该节点值，右子树的所有节点值大于该节点值。

2. 实现方法：
   - 解析输入构建二叉树。
   - 使用辅助函数进行递归检查，传递当前节点的值范围。

---

## 代码

```cpp []
#include <iostream>
#include <unordered_map>
#include <sstream>
#include <limits>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

bool isBST(TreeNode* node, int minVal, int maxVal) {
    if (!node) return true;
    if (node->val <= minVal || node->val >= maxVal) return false;
    return isBST(node->left, minVal, node->val) && isBST(node->right, node->val, maxVal);
}

int main() {
    int rootVal;
    cin >> rootVal;

    unordered_map<int, TreeNode*> nodes;
    TreeNode* root = new TreeNode(rootVal);
    nodes[rootVal] = root;

    string line;
    cin.ignore(); // 忽略换行符
    while (getline(cin, line)) {
        int parentVal, leftVal, rightVal;
        char colon, pipe;
        stringstream ss(line);
        ss >> parentVal >> colon >> leftVal >> pipe >> rightVal;

        if (nodes.find(parentVal) == nodes.end()) {
            nodes[parentVal] = new TreeNode(parentVal);
        }
        TreeNode* parent = nodes[parentVal];

        if (leftVal != -1) {
            parent->left = new TreeNode(leftVal);
            nodes[leftVal] = parent->left;
        }
        if (rightVal != -1) {
            parent->right = new TreeNode(rightVal);
            nodes[rightVal] = parent->right;
        }
    }

    // 检查是否为二分查找树
    if (isBST(root, numeric_limits<int>::min(), numeric_limits<int>::max())) {
        cout << 1 << endl; // 是二分查找树
    } else {
        cout << 0 << endl; // 不是二分查找树
    }

    return 0;
}
```

```java []
import java.util.*;

public class Main {
    static class TreeNode {
        int val;
        TreeNode left, right;
        TreeNode(int x) {
            val = x;
        }
    }

    static boolean isBST(TreeNode node, int minVal, int maxVal) {
        if (node == null) return true;
        if (node.val <= minVal || node.val >= maxVal) return false;
        return isBST(node.left, minVal, node.val) && isBST(node.right, node.val, maxVal);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int rootVal = sc.nextInt();
        sc.nextLine(); // 读取换行符

        Map<Integer, TreeNode> nodes = new HashMap<>();
        TreeNode root = new TreeNode(rootVal);
        nodes.put(rootVal, root);

        while (sc.hasNextLine()) {
            String line = sc.nextLine();
            String[] parts = line.split(":|\\|");
            int parentVal = Integer.parseInt(parts[0]);
            int leftVal = Integer.parseInt(parts[1]);
            int rightVal = Integer.parseInt(parts[2]);

            if (!nodes.containsKey(parentVal)) {
                nodes.put(parentVal, new TreeNode(parentVal));
            }
            TreeNode parent = nodes.get(parentVal);

            if (leftVal != -1) {
                parent.left = new TreeNode(leftVal);
                nodes.put(leftVal, parent.left);
            }
            if (rightVal != -1) {
                parent.right = new TreeNode(rightVal);
                nodes.put(rightVal, parent.right);
            }
        }

        // 检查是否为二分查找树
        if (isBST(root, Integer.MIN_VALUE, Integer.MAX_VALUE)) {
            System.out.println(1); // 是二分查找树
        } else {
            System.out.println(0); // 不是二分查找树
        }
    }
}
```

```python []
import sys
sys.setrecursionlimit(100000)  # 扩大递归栈深度

class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def is_bst(node, min_val, max_val):
    if not node:
        return True
    if node.val <= min_val or node.val >= max_val:
        return False
    return is_bst(node.left, min_val, node.val) and is_bst(node.right, node.val, max_val)

def main():
    root_val = int(input())
    
    nodes = {}
    root = TreeNode(root_val)
    nodes[root_val] = root
    
    try:
        while True:
            line = input().strip()
            if not line:
                break
                
            # 解析输入行
            parts = line.split(':')
            parent_val = int(parts[0])
            left_val, right_val = map(int, parts[1].split('|'))
            
            # 如果父节点不存在，创建父节点
            if parent_val not in nodes:
                nodes[parent_val] = TreeNode(parent_val)
            parent = nodes[parent_val]
            
            # 创建左右子节点
            if left_val != -1:
                parent.left = TreeNode(left_val)
                nodes[left_val] = parent.left
            if right_val != -1:
                parent.right = TreeNode(right_val)
                nodes[right_val] = parent.right
                
    except EOFError:
        pass
    
    # 检查是否为二分查找树
    if is_bst(root, float('-inf'), float('inf')):
        print(1)  # 是二分查找树
    else:
        print(0)  # 不是二分查找树

if __name__ == "__main__":
    main()

```

---

## 算法及复杂度
- 算法：二叉树遍历
- 时间复杂度：$\mathcal{O(n)}$，其中 $n$ 为节点数量
- 空间复杂度：$\mathcal{O(n)}$，用于存储树的节点