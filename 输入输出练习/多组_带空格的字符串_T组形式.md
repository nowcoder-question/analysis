## 题目
[题目链接](https://www.nowcoder.com/practice/cff28a28d7f54419a640a8bb19f4275f?tpId=372&tqId=10979388&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
特别地，对于C++，需要使用 getline(cin,s) 而非 cin>>s 的方式读入**带空格的**字符串。

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
	string s,tmp,ans;
	int t,n;
	cin>>t>>ws;
	while(t--)
	{
		cin>>n>>ws; // ws 用于去除多余的控制符（\r 和 \n）
		getline(cin,s);
		ans=tmp="";
		for(auto &e:s)
		{
			if(e==' ')
			{
				if(!tmp.empty())
				{
					ans+=tmp;
					tmp.clear();
				}
			}
			else
				tmp+=e;
		}
		ans+=tmp;
		reverse(ans.begin(),ans.end());
		cout<<ans<<'\n';
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
        int t,n;
        s=sc.nextLine();
        t=Integer.parseInt(s);
        while(t-->0){
            s=sc.nextLine();
            n=Integer.parseInt(s);
            s=sc.nextLine();
            String[] a=s.split(" ");
            String b=String.join("",a);
            b=new StringBuilder(b).reverse().toString();
            System.out.println(b);
        }
    }
}
```
``` python []
t=int(input())
for _ in range(0,t):
    n=int(input())
    s=input()
    a=list(map(str,s.split()))
    s=''.join(a)
    s=s[::-1]
    print(s)
```

---

## 算法及复杂度
- 算法：枚举。  
- 时间复杂度：$\mathcal{O}(\sum n)$ 。  
- 空间复杂度：$\mathcal{O}(\sum n)$ 。  
