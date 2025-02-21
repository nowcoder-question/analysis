## 题目
[题目链接](https://www.nowcoder.com/practice/da783089da3b4a3188d240e1b7ac4b23?tpId=372&tqId=10979417&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
后台有spj代码，能对同学们的输出数据进行校验，符合条件即可通过。  
附赠 spj 代码  

``` cpp
#include <iostream>
#include <fstream>
#include <string>
#include <cctype>
using namespace std;
bool check(string &s)
{
	if(s.length()>10)
		return false;
	if(s.front()=='0')
		return false;
	for(char &c:s)
	{
		if(c=='-')
			return false;
		else if(!isdigit(c))
			return false;
	}
	return true;
}
int main(void)
{
	// 记得重命名为 checker.cc 
	ifstream in,out,user_out;
	in.open("input");
	out.open("output");
	user_out.open("user_output");
	string s;
	long long sum=0,res;
	int n,m,i;
	in>>n>>m;
	for(i=1;i<=n;++i)
	{
		if(!(user_out>>s))
			return 1;
		if(!check(s))
			return 1;
		res=stoll(s);
		sum+=res;
	}
	if(sum!=m)
		return 1;
	if(user_out>>s)
		return 1;
	return 0;
}
```

---

## 代码

``` cpp []
#include <iostream>
using namespace std;
int main(void)
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n,m,i;
	cin>>n>>m;
	for(i=1;i<n;++i)
		cout<<"1 ";
	cout<<m-n+1;
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int n=sc.nextInt(),m=sc.nextInt(),i;
        int[] a=new int[n+5];
        for(i=1;i<n;++i)
            a[i]=1;
        a[n]=m-n+1;
        for(i=1;i<=n;++i)
            System.out.print(a[i]+" ");
    }
}
```
``` python []
n,m=map(int,input().split())

# print('1 '*(n-1)+str(m-n+1))

a=[1]*(n-1)
a.append(m-n+1)
print(' '.join(map(str,a)))
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(n)$ 。  
- 空间复杂度：$\mathcal{O}(n)$ 。