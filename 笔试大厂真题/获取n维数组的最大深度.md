## 题目
[题目链接](https://www.nowcoder.com/practice/08707b9b484f4ca4943f108c709dab96?tpId=182&tqId=325943&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

#题解

###题目难度：简单题目
###知识点：字符串、数学逻辑

##思路：
用两个变量left和ans。其中当遇到“[”时，left的值增加一，当遇到“]”时，left的值减少一。再这个变化过程中，left能达到的最大值为最大的层数,将其保存再ans中。

![图片说明](https://uploadfiles.nowcoder.com/images/20200420/735510_1587372566989_B0062C387EA68EBCF025BFAE5206105C) 


```


#include<iostream>
using namespace std;
int main() {
	char ch;
    int left = 0;
    int ans = 0;
    while (cin &gt;&gt; ch) {
        if (ch == '[') {
            ++left;
            ans = max(ans, left);
        } else if (ch == ']') {
            --left;
        }  
    }
    cout &lt;&lt; ans &lt;&lt; endl;
    return 0;
}
```
</iostream>