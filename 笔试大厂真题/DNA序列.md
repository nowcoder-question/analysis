## 题目
[题目链接](https://www.nowcoder.com/practice/ab900f183e054c6d8769f2df977223b5?tpId=182&tqId=137991&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

# DNA序列

## 题目难度：中等

## 知识点：数学逻辑、宽度优先遍历

## 方法一
采用宽度优先遍历，得到长度为i的子串的所有排列，比较字符串中是否存在全部子串，若全部存在，则继续下一个长度的子串的遍历，否则，i即为字符串中没有包含所有该长度的子串，输出i，循环结束。
```
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            String str = sc.next();
            //i表示子串的长度
            for(int i = 1; i &amp;amp;lt; str.length()+1; i++) {
                String strArr[] = getAllPermutation(i);
                boolean flag = true;
                for(int index = 0; index &amp;amp;lt; strArr.length; index++) {
                    //判断字符串中是否存在该子串
                    if(str.indexOf(strArr[index]) == -1) {
                        flag = false;
                        break;
                    }
                }
                if(!flag) {
                    System.out.println(i);
                    break;
                }
            }
        }  
    } 
    //宽度优先遍历
    public static String[] getAllPermutation(int n) {
        int arr[] = new int[n + 1];  
        String strArr[] = new String[(int)Math.pow(4, n)];
        int index = 0;
        while(arr[0] == 0) {
            String curStr = &amp;amp;quot;&amp;amp;quot;;
            for(int i = 1; i &amp;amp;lt; n + 1; i++) {
                if(arr[i] == 0) curStr += &amp;amp;quot;A&amp;amp;quot;;
                else if(arr[i] == 1) curStr += &amp;amp;quot;C&amp;amp;quot;;
                else if(arr[i] == 2) curStr += &amp;amp;quot;G&amp;amp;quot;;
                else if(arr[i] == 3) curStr += &amp;amp;quot;T&amp;amp;quot;;
            }
            strArr[index] = curStr;
            index++;
            arr[n] = arr[n] + 1;
            for(int i = n; i &amp;amp;gt;= 0; i--) {
                if(arr[i] == 4) {
                    arr[i] = 0;
                    arr[i - 1] += 1;
                }
            }
        }
        return strArr;
    }
}  
```

## 方法二
按照子串的数量来判断是否全部包含。子串中，每一种长度i都会有4的i次方个不同的子串。定义一个set容器，将长度为i的子串依次加入到set容器中去，set容器会自动除去重复的元素，因此set的大小就是所有子串的数量，当set小于4的i次方时，即字符串中没有包含所有该长度的子串。

```
import java.util.*;
public class Main{    
    public static void main(String[] args){        
        Scanner in = new Scanner(System.in);        
        String s = in.nextLine();                 
        //i表示子串的长度
        for(int i=1;i&amp;amp;lt;=s.length();i++){            
            HashSet&amp;amp;lt;String&amp;amp;gt; set= new HashSet&amp;amp;lt;String&amp;amp;gt;();            
            //遍历字符串，将所有长度为i的子串加入set中           
            for(int j=0;j&amp;amp;lt;=s.length()-i;j++){                
                set.add(s.substring(j,j+i));
            }
            //如果长度为i子串的个数是否小于4^i
            if(set.size() &amp;amp;lt; Math.pow(4,i)){                
                System.out.println(i);                
                break;            
            }        
        }    
    }
}
```