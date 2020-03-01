# Leetcode-
Leetcode字节跳动面试题，按出现次数排序
---
### 1, Leetcode1101 彼此熟识的最早时间
+ 题意：N个人，给定一些时间点，双方成为朋友。规定朋友的朋友就是自己的朋友，问N个人最早变为朋友的时间。 
+ 思路：按时间排序，并查集处理。  
+ 考点：排序二维数组，并查集。  
+ 坑点：要给fa定义大小，即fa.resize(N)，不然会：
   > Line 923: Char 34: runtime error: reference binding to null pointer of type   
+ static：[关于static](https://blog.csdn.net/weixin_40311211/article/details/82851300)
```
class Solution {
public:
    static bool cmp(vector<int>&a,vector<int>&b) {
        return a[0]<b[0];
    }
    vector<int>fa;
    int find(int x){
        if(fa[x]==x) return x;
        return fa[x]=find(fa[x]);
    }
    int earliestAcq(vector<vector<int> >& logs, int N) {
        fa.resize(N);
        int res=-1,num=0;
        sort(logs.begin(),logs.end(),cmp);
        for(int i=0;i<N;i++) fa[i]=i;
        for(auto log:logs){
            int x=find(log[1]);
            int y=find(log[2]);
            if(x==y) continue;
            fa[x]=y;
            num++;
            if(num==N-1){
                res=log[0];
                break;
            }
        }
        return res;
    }
};
```

---
---


### 2, Leetcode440 字典序的第K小数字  
+ 题意：问1到N，按照字典序排序，第K大的是？  
+ 思路：一层一层的试探。  这个题第一次做估计还是有点难的。

---
---
### 3, 135. 分发糖果   
+ 题意：有N个人站成一排，每个人有个评分，现在要根据评分发糖果，相邻的人，如果评分高，则糖果多，问至少发多少个糖果。  
+ 思路：显然可以差分约束，但是现在只有相邻的约束，显然没必要那么麻烦。我们扫描两遍即可，左边一遍得到一个res1[]，表示至少多少可以满足左边的约束，再从右边扫一遍，在res1基础上得到res2，则res2可以满足左边和右边的约束。  
```
class Solution {
public:
    int candy(vector<int>& ratings) {
        int N=ratings.size(),sum=0;
        vector<int>res;
        res.resize(N);
        res[0]=1;
        for(int i=1;i<N;i++) {
            res[i]=ratings[i]>ratings[i-1]?res[i-1]+1:1;
        }
        for(int i=N-2;i>=0;i--){
            res[i]=ratings[i]>ratings[i+1]?max(res[i+1]+1,res[i]):res[i];
        }
        for(auto i:res) sum+=i;
        return sum;
    }
};
```





