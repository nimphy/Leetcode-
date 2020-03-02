# Leetcode-
Leetcode字节跳动面试题，按指数排序
---
### 1, Leetcode1. 两数之和 
+ 题意：给定N个数，以及一个target，问是否有两个数的和为target。  
+ 思路：O(N),扫描一遍，以及扫到的存在map里，就OK了。  
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        unordered_map<int,int>mp;
        for(int i=0;i<nums.size();i++){
            int x=target-nums[i];
            if(mp.find(x)!=mp.end()){
                res.push_back(mp[x]);
                res.push_back(i);
                break;
            }
            mp[nums[i]]=i;
        }
        return res;
    }
};
```
---
### 2, Leetcode2. 两数相加 
+ 题意：用倒序的链表表示数字，求和。 
+ 思路：用一个数表示进位，如果两个都为null就不再加，最后特判是否还要进位。 

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *res=new ListNode(0);
        ListNode *cur=res;
        int sum=0;
        while(l1!=NULL||l2!=NULL){
            if(l1!=NULL) {
                sum+=l1->val;
                l1=l1->next;
            }
            if(l2!=NULL) {
                sum+=l2->val;
                l2=l2->next;
            }
            cur->next=new ListNode(sum%10);
            cur=cur->next;
            sum=sum/10;
        }
        if(sum!=0) cur->next=new ListNode(sum);
        return res->next;
    }
};
```
---
### 3, Leetcode3. 无重复字符的最长子串 
+ 题意： 求最长的长度，无重复出现相同字符。 
+ 思路：保存每个字符上一次出现的位置即可，map。 
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int L=s.length(),res=0,pre=-1;
        unordered_map<int,int>mp;
        for(int i=0;i<L;i++){
            if(mp.find(s[i])!=mp.end())pre=max(pre,mp[s[i]]);
            res=max(res,i-pre);
            mp[s[i]]=i;
        }
        return res;
    }
};
```
---
### 4, Leetcode5. 最长回文子串
+ 题意：求最长的回文串长度。  
+ 思路：DP，马拉车，回文自动机。 这里简单DP一下。 
```
class Solution {
public:
    string longestPalindrome(string s) {
        int dp[1010][1010];
        int L=s.length(),res=0,a,b;
        string ans="";
        for(int i=0;i<L;i++) dp[i][i]=1;
        for(int i=0;i<L-1;i++) dp[i][i+1]=(s[i]==s[i+1]);
        for(int i=L-1;i>=0;i--){
            for(int j=i+2;j<L;j++){
                dp[i][j]=dp[i+1][j-1]&&(s[i]==s[j]);
            }
        }
        for(int i=0;i<L;i++)
         for(int j=i;j<L;j++)
          if(dp[i][j]&&res<j-i+1) {
            res=j-i+1;
            a=i;b=j;
        }
        for(int i=a;i<=b;i++) ans+=s[i];
        return ans;
    }
};
```
---
### 5, Leetcode206. 反转链表 
+ 题意：你可以迭代或递归地反转链表。你能否用两种方法解决这道题？  
+ 思路: 
```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre=NULL,*prepre=NULL;
        while(head!=NULL){
            if(pre!=NULL) pre->next=prepre;
            prepre=pre;
            pre=head;
            head=head->next;
        }
        if(pre!=NULL) pre->next=prepre;
        return pre;
    }
};
```
---
### 6, Leetcode42. 接雨水 
+ 题意：给定一排柱子，对应的高度，问集雨水的容量。 
+ 思路：算出每个柱子上面能集多少雨水，分别算贡献，+=min(左边最高，右边最高)-height[i]；   
```
class Solution {
public:
    int trap(vector<int>& height) {
        int SZ=height.size(),res=0;
        if(SZ==0) return 0;
        vector<int>L,R;
        L.resize(SZ);
        R.resize(SZ);
        L[0]=height[0]; R[SZ-1]=height[SZ-1];
        for(int i=1;i<SZ;i++) L[i]=max(L[i-1],height[i]);
        for(int i=SZ-2;i>=0;i--) R[i]=max(R[i+1],height[i]);
        for(int i=0;i<SZ;i++) res+=min(R[i],L[i])-height[i];
        return res;
    }
};
```
---
### 7, Leetcode4. 寻找两个有序数组的中位数
+ 题意：两个有序数组，让你找中位数。  
+ 思路：显然是二分，但是普通的二分已经满足不了了，还得加一点小技巧---迭代逼近。假设第一个数组是A[]，第二个数组是B[]，找第K个数，那么我们各自看右移K/2个数字后谁小，那么它就右移K/2位，直到找到中位数。  

```
为什么下标要从0开始啊。我好不习惯。
```
---
### 8, Leetcode1101 彼此熟识的最早时间
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


### 9, Leetcode440 字典序的第K小数字  
+ 题意：问1到N，按照字典序排序，第K大的是？  
+ 思路：一层一层的试探。  这个题第一次做估计还是有点难的。
> 待补

---
### 10, 135. 分发糖果   
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
---
### 11,146. LRU缓存机制  
+ 题意：设计一种算法，满足：给定一个大小为N的容器，支持O1插入一个数，删去一个数，而且找到最远的插入的一个。
+ 思路：关键就在于在于要O1删去之间的某个数，普通的栈、队列高效的做到。 这里用了一个小技巧，就是记下在队列中的位置（迭代器iterator完成），那么就可以快速删去了。其他部分和普通list一样。
```
class LRUCache {
public:
    LRUCache(int capacity):
        capacity(capacity) {}
    
    int get(int key) {
        if(mp.find(key)==mp.end()) return -1;
        put(key,mp[key]->second);
        return mp[key]->second;
    }
    //关键字key，对应值为val；
    //如果存在key，删去
    //如果容量不够，删去头
    void put(int key, int value) {
        if(mp.find(key)!=mp.end()) lis.erase(mp[key]);
        else if(mp.size()==capacity){
            mp.erase(lis.back().first);
            lis.pop_back();
        }
        lis.push_front(make_pair(key,value));
        mp[key]=lis.begin();
    }
private: 
    int capacity;
    list<pair<int,int> >lis;
    unordered_map<int,list<pair<int,int> >::iterator >mp;
};
```





