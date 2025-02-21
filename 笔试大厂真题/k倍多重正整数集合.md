## 题目
[题目链接](https://www.nowcoder.com/practice/10f2fab3ceb043088d2a5cca339d4413?tpId=182&tqId=340266&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

**题解：**
**题目难度：三星**

**考察点：** 哈希，思维

**易错点：**

很多同学在枚举倍数的时候会漏掉当$k$的值为$1$的情况，事实上，这种情况需要单独讨论，$k$的值为1时，集合最大可容纳的元素个数就是集合中元素的种数，也就是让集合中不同的元素只能出现1次。

**题解：**

这题的数据范围为$10^5$，因此如果对于每个位置暴力去探寻它的$k$倍是否存在与序列种显然是不可行的，因为这样复杂度为$O(n^2)$，$1s$的时间内承受不住这么高的复杂度。可以通过空间换时间的方法来进行处理，通过哈希表来处理，这里建议选用$map$，$map$的底层是颗红黑树，当作哈希表使用查询的复杂度是$O(log_2n)$的，同时$map$是有序的，可以保证枚举的时候是从小到大进行枚举的。

对于$k$为$1$的情况，可以直接参照易错点中所述，输出元素的类数即可。对于$k$不为$1$的情况，用$map$统计出每个数出现的次数，然后对$map$中的数从小到大进行枚举，如果当前数的$k$倍在序列中，且当前数的个数不为0(因为是从小到大枚举的，很可能当前数被更小的数删除过)。则在序列中二者不能同时存在，选取二者中的出现次数更小的，将其从序列中删除，最后删完所有数，序列中余下的就是所求，复杂度$O(nlog_2n)$

```cpp
#include "bits/stdc++.h"
using namespace std;
const int maxn=1e5+100;
typedef long long LL;
map<ll,int>mp;
int n;
LL k,a[maxn];
int main()
{
    scanf("%d%lld",&amp;n,&amp;k);
    for(int i=1;i&lt;=n;i++){
        scanf("%lld",&amp;a[i]);
        mp[a[i]]++;
    }
    if(k==1){
        printf("%d\n",mp.size());
    }else{
        for(auto &amp;x:mp){
            if(mp.find(x.first*k)==mp.end()) continue;
            int mi=min(x.second,mp[x.first*k]);
            n-=mi;
            x.second-=mi;
            mp[x.first*k]-=mi;
        }
        printf("%d\n",n);
    }
    return 0;
}
```

</ll,int>