# Leetcode-
Leetcode字节跳动面试题，按出现次数排序
---
### 1,Leetcode1101 彼此熟识的最早时间
+ 题意：N个人，给定一些时间点，双方成为朋友。规定朋友的朋友就是自己的朋友，问N个人最早变为朋友的时间。 
+ 思路：按时间排序，并查集处理。  
+ 考点：排序二维数组，并查集。  
+ 坑点：要给fa定义大小，fa.resize(N)，不然会：
   > Line 923: Char 34: runtime error: reference binding to null pointer of type
'''
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
'''
