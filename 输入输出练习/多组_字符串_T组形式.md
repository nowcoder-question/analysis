## 题目
[题目链接](https://www.nowcoder.com/practice/35f6f4e9173943e3b3f462b697fefd8b?tpId=372&tqId=10979378&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
输入字符串，然后用库函数或者循环将其反转，最后输出。   

---

## 代码

``` cpp []
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	string s;
	int t,n;
	cin>>t;
	while(t--)
	{
		cin>>n>>s;
		reverse(s.begin(),s.end());
		cout<<s<<'\n';
	}
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        String s;
        int t=sc.nextInt(),n;
        while(t-->0){
            n=sc.nextInt();
            s=sc.next();
            s=new StringBuilder(s).reverse().toString();
            System.out.println(s);
        }
    }
}
```
``` python []
t=int(input())
for _ in range(0,t):
    n=int(input())
    s=input()
    s=s[::-1]
    print(s)
```

---

## 算法及复杂度
- 算法：枚举。  
- 时间复杂度：$\mathcal{O}(\sum n)$ 。  
- 空间复杂度：$\mathcal{O}(\sum n)$ 。