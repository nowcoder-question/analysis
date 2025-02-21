## 题目
[题目链接](https://www.nowcoder.com/practice/4dffedb2a0734a0e9ce759f2eaeeda65?tpId=372&tqId=10979403&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
后台有spj代码，能对同学们的输出数据进行校验，符合条件即可通过。  
附赠 spj 代码  

``` cpp
#include <iostream>
#include <fstream>
#include <string>
#include <cctype>
#include <algorithm>
using namespace std;
int main(void)
{
	ifstream in,out,user_out;
	in.open("input");
	out.open("output");
	user_out.open("user_output");
	string s,t;
	if(!(user_out>>s))
		return 1;
	out>>t;
	if(s.length()>5)
		return 1;
	transform(s.begin(),s.end(),s.begin(),::toupper);
	transform(t.begin(),t.end(),t.begin(),::toupper);
	if(s!=t)
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
	int n;
	cin>>n;
	if(n%2==1)
		cout<<"YES";
	else
		cout<<"NO";
	return 0;
}
```
``` java []
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        int n=sc.nextInt();
        if(n%2==1)
            System.out.print("YES");
        else
            System.out.print("NO");
    }
}
```
``` python []
n=int(input())
print('YES' if n%2==1 else 'NO')
```

---

## 算法及复杂度
- 算法：无。  
- 时间复杂度：$\mathcal{O}(1)$ 。  
- 空间复杂度：$\mathcal{O}(1)$ 。