## 题目
[题目链接](https://www.nowcoder.com/practice/f0f1015579904ebc92974f7c92764797?tpId=182&tqId=325921&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

# 题解

## 难度：中等

## 知识点：map、查找、字符分割
解题剖析：涉及到数据库，主要就是考察添加、删除和查找的知识。因为添加\删除\查找都对应了不同的字符串首字母，所以只要在分割字符串的过程后就可以对应出相应的处理。因为添加和删除都是相同的输出，所以主要需要考虑的是怎么查找。这里可以直接使用一个map<string,bool>来对应出这个ip是否存在，以便在查找时做出不同的输出。
## 解题
```
#include <bits stdc++.h>
using namespace std;
int main(){
    string s;
    //使用一个map来判断查找的ip是否存在
    map<string,bool> m;
    while(cin&gt;&gt;s){
        if(s=="end")
            break;
        string ip = s.substr(2);
        char op = s[0];
        if(op=='i'){
            m[ip] = true;
            cout&lt;&lt;"ok"&lt;</string,bool></bits></string,bool>