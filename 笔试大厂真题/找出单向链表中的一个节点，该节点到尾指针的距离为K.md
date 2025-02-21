## 题目
[题目链接](https://www.nowcoder.com/practice/0cff324157a24a7a8de3da7934458e34?tpId=182&tqId=325924&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

# 题解

## 题目难度：简单

## 知识点：查找、链表
解题思路：这是一道考察链表知识点的题，要求输出倒数第k个节点的值，因为此链表结构是顺着保存数据的，所以指针无法访问倒数的值。解题关键在使用两个距离相差k的指针进行数据访问，一旦第一个指针已经到最后的节点时，此时第二个指针所在的位置刚好是倒数第k个节点。

## 题解
第一步：创建一个链表保存数据；第二步：使用两个相距k的指针同时进行访问链表，一旦第一个到达末尾，第二个即在所求节点位置。

```
#include <bits stdc++.h>
using namespace std;
struct ListNode
{
    int m_nKey;
    ListNode* m_pNext;
    ListNode(int x):m_nKey(x),m_pNext(nullptr){};
};
int main()
{
    //使用一个指针head，保存目前链表的头
    ListNode* head=new ListNode(0);
    ListNode* cur=head;
    for(int i=1;i&lt;8;i++)
    {
        ListNode* temp=new ListNode(i);
        cur-&gt;m_pNext=temp;
        cur=cur-&gt;m_pNext;
    }
    int k;
    cin&gt;&gt;k;
    cur=head;
    //将两个指针距离k
    for(int i=0;i<k;i++) { cur="cur-">m_pNext;
   }
    //两个指针同时前进，直到前面的指针走完，这时head的值就是第倒数k个的节点
    while(cur)
    {
        cur=cur-&gt;m_pNext;
        head=head-&gt;m_pNext;
   }
    cout&lt;<head->m_nKey&lt;</head-></k;i++)></bits>