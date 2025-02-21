## 题目
[题目链接](https://www.nowcoder.com/practice/a632ec91a4524773b8af8694a51109e7?tpId=182&tqId=363029&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路

这是一道链表操作题目，主要思路如下：

1. 问题分析：
   - 给定一个链表和整数 $k$
   - 每 $k$ 个节点为一组进行翻转
   - 如果最后剩余节点不足 $k$ 个，保持原有顺序
   - 要求实际交换节点，而不是仅改变值

2. 解决方案：
   - 使用虚拟头节点简化操作
   - 实现单链表翻转的辅助函数
   - 按组遍历并翻转链表
   - 维护前驱和后继节点的指向关系

3. 实现细节：
   - 使用三指针法实现链表翻转
   - 记录每组翻转的起始和结束位置
   - 处理最后一组不足 $k$ 个节点的情况

---

## 代码

```cpp []
#include <iostream>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    // 翻转单个链表
    ListNode* reverseList(ListNode* head) {
        ListNode *prev = nullptr, *curr = head, *next = nullptr;
        while (curr) {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
    
    // K组翻转
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *pre = dummy, *end = dummy;
        
        while (end->next) {
            // 找到每组的结束位置
            for (int i = 0; i < k && end; i++) {
                end = end->next;
            }
            if (!end) break;  // 不足k个，结束翻转
            
            // 保存相关节点
            ListNode* start = pre->next;
            ListNode* next = end->next;
            
            // 断开当前组
            end->next = nullptr;
            
            // 翻转当前组并连接
            pre->next = reverseList(start);
            start->next = next;
            
            // 移动指针到下一组
            pre = end = start;
        }
        
        ListNode* result = dummy->next;
        delete dummy;
        return result;
    }
};

int main() {
    // 读入链表
    ListNode* dummy = new ListNode(-1);
    ListNode* tail = dummy;
    int val;
    while (cin >> val) {
        tail->next = new ListNode(val);
        tail = tail->next;
        if (cin.get() == '\n') break;
    }
    
    // 读入k
    int k;
    cin >> k;
    
    // 处理并输出结果
    Solution solution;
    ListNode* result = solution.reverseKGroup(dummy->next, k);
    
    // 输出结果
    while (result) {
        cout << result->val;
        if (result->next) cout << " ";
        result = result->next;
    }
    cout << endl;
    
    return 0;
}
```

```java []
import java.util.*;

class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

public class Main {
    // 翻转单个链表
    public static ListNode reverseList(ListNode head) {
        ListNode prev = null, curr = head, next = null;
        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
    
    // K组翻转
    public static ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy, end = dummy;
        
        while (end.next != null) {
            // 找到每组的结束位置
            for (int i = 0; i < k && end != null; i++) {
                end = end.next;
            }
            if (end == null) break;
            
            // 保存相关节点
            ListNode start = pre.next;
            ListNode next = end.next;
            
            // 断开当前组
            end.next = null;
            
            // 翻转当前组并连接
            pre.next = reverseList(start);
            start.next = next;
            
            // 移动指针到下一组
            pre = end = start;
        }
        
        return dummy.next;
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        // 读入链表
        ListNode dummy = new ListNode(-1);
        ListNode tail = dummy;
        String[] nums = sc.nextLine().split(" ");
        for (String num : nums) {
            tail.next = new ListNode(Integer.parseInt(num));
            tail = tail.next;
        }
        
        // 读入k
        int k = sc.nextInt();
        
        // 处理并输出结果
        ListNode result = reverseKGroup(dummy.next, k);
        
        // 输出结果
        while (result != null) {
            System.out.print(result.val);
            if (result.next != null) System.out.print(" ");
            result = result.next;
        }
        System.out.println();
        
        sc.close();
    }
}
```

```python []
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def reverse_list(head):
    prev = None
    curr = head
    while curr:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node
    return prev

def reverse_k_group(head, k):
    dummy = ListNode(-1)
    dummy.next = head
    pre = end = dummy
    
    while end.next:
        # 找到每组的结束位置
        for i in range(k):
            end = end.next
            if not end:
                return dummy.next
        
        # 保存相关节点
        start = pre.next
        next_node = end.next
        
        # 断开当前组
        end.next = None
        
        # 翻转当前组并连接
        pre.next = reverse_list(start)
        start.next = next_node
        
        # 移动指针到下一组
        pre = end = start
    
    return dummy.next

def main():
    # 读入链表
    nums = list(map(int, input().split()))
    dummy = ListNode(-1)
    tail = dummy
    for num in nums:
        tail.next = ListNode(num)
        tail = tail.next
    
    # 读入k
    k = int(input())
    
    # 处理并输出结果
    result = reverse_k_group(dummy.next, k)
    
    # 输出结果
    while result:
        print(result.val, end='')
        if result.next:
            print(' ', end='')
        result = result.next
    print()

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：链表操作
- 时间复杂度：$\mathcal{O}(n)$ - 每个节点最多被访问两次
- 空间复杂度：$\mathcal{O}(1)$ - 只使用常数额外空间
