## 题目
[题目链接](https://www.nowcoder.com/practice/673454422d1b4a168aed31e449d87c00?tpId=182&tqId=298693&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

#题解
该问题为排序问题，给出几种常用方法：

##方法（一）
使用vector，用库函数sort进行排序。

```
#include<vector>
#include<iostream>
#include<algorithm>
using namespace std;
bool compare(const int &amp;a,const int &amp;b){
   	return a&gt;b;
   }
int main(){
   int num; 
   vector<int> v; 
//将字符串中的数字放入vector中 
  while(getchar()!=']')
    {
        cin&gt;&gt;num;
        v.push_back(num);   
    }
//sort排序,默认为由小到大，需要由大到小自己写compare函数 
   sort(v.begin(),v.end(),compare);
//输出第三大的数字
   cout&lt;<v[2]; return 0; } ``` ##方法（二） 冒泡排序 #include<vector>
#include<iostream>
using namespace std;
int main(){
   int num; 
   vector<int> v; 
//将字符串中的数字放入vector中 
  while(getchar()!=']')
    {
        cin&gt;&gt;num;
        v.push_back(num);   
    }
//冒泡排序 
  for(int i = 0 ;i<v.size();i++){ 第i趟比较 for(int j="0" ;j<v.size()-i-1;j++){ 开始进行比较，如果arr[j]比arr[j+1]的值小，那就交换位置 if(v[j]<v[j+1]){ int temp="v[j];" v[j]="v[j+1];" v[j+1]="temp;" } 输出第三大的数字 cout<<v[2]; return 0; ``` ##方法（三） 快速排序 #include<vector>
#include<iostream>
using namespace std;
//快排
    void swap(vector<int> &amp;arr, int i, int j) {
        if (arr.size()==0 || i &gt;= arr.size() || j &gt;= arr.size() || i &lt; 0 || j &lt; 0) {
            return;
        }
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    int partion(vector<int> &amp;data, int begin, int end) {
        int temp = data[begin];
        while (begin &lt; end) {
            while (begin &lt; end &amp;&amp; data[end] &lt;= temp) {
                end--;
            }
            swap(data, begin, end);
            while (begin &lt; end &amp;&amp; data[begin] &gt; temp) {
                begin++;
            }
            swap(data, begin, end);
        }
        return begin;
    }
    int findKthNum(vector<int> &amp;data, int k) {
        int begin = 0, end = data.size() - 1;
        int pos = 0;
        while (begin &lt;= end) {
            pos = partion(data, begin, end);
            if (pos == k - 1) {
                return data[pos];
            } else if (pos &gt; k - 1) {
                end = pos - 1;
            } else {
                begin = pos + 1;
            }
        }
        return -1;
    } 
int main(){
   int num; 
   vector<int> v; 
//将字符串中的数字放入vector中 
  while(getchar()!=']')
    {
        cin&gt;&gt;num;
        v.push_back(num);   
    }
//调用快排输出第三大的数 
   cout&lt;<findkthnum(v,3); return 0; } ``` ##方法（四） 求第n个最大的数先建立最小堆把海量数据向最小堆添加n个数，堆的内部会自动排序，不用管它的内部是什么操作。然后我们往最小堆进行插入操作，因为最小堆的特点就是堆顶元素最小。若插入元素小于堆顶元素 ，则它小于堆中任何一个元素， 所以摒弃该元素遍历下一个元素。若插入元素大于堆顶元素则进行插入， 插入后堆内自动排序，堆顶元素又成为该堆的最小元素 ，原堆顶元素出堆。持续遍历完得到的就是所有元素中第n个最大数。 #include<iostream>
#include<queue>
#include<vector>
using namespace std;
int main(){
	//在最短时间内找到所有整数中最大/最小的n个数并且打印；
	//找top最大的用小根堆；找top最小的用大根堆
	//时间复杂度 O(n)*log2 n
    vector<int> v;//保存字符串中所有出现的数 
    int n=0;
    int num;
    while(getchar()!=']') {
        cin&gt;&gt;num;
        n++;
        v.push_back(num);   
    }
	priority_queue<int, vector<int>, greater<int> &gt; minHeap; //建立最小堆
	//注意：最小堆要加上greater<int>
	int j = 3;
	for (int i = 0; i &lt; j; i++)	{
		minHeap.push(v[i]);
	}
	for (int i = j; i &lt; n; ++i){
	    //遍历判断容器中剩余的每个数与堆顶元素的大小
	    //若大于堆顶元素 则把堆顶元素出堆 把该元素入堆
		if (v[i] &gt; minHeap.top())
		{
			minHeap.pop();
			minHeap.push(v[i]);
		}
	}
		cout &lt;&lt; minHeap.top() &lt;&lt; " ";
	cout &lt;&lt; endl;
	return 0;
}
```
</int></int></int,></int></vector></queue></findkthnum(v,3);></int></int></int></int></iostream></v.size();i++){></int></iostream></v[2];></int></algorithm></iostream></vector>