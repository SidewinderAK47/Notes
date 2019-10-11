## 667 Beautiful Arrangement II 优美的排列

Medium

## 题目描述：

Given two integers `n` and `k`, you need to construct a list which contains `n` different positive integers ranging from `1` to `n` and obeys the following requirement: 
Suppose this list is [a1, a2, a3, ... , an], then the list [|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] has exactly `k` distinct integers.

If there are multiple answers, print any of them.

**Example 1:**

```
Input: n = 3, k = 1
Output: [1, 2, 3]
Explanation: The [1, 2, 3] has three different positive integers ranging from 1 to 3, and the [1, 1] has exactly 1 distinct integer: 1.
```



**Example 2:**

```
Input: n = 3, k = 2
Output: [1, 3, 2]
Explanation: The [1, 3, 2] has three different positive integers ranging from 1 to 3, and the [2, 1] has exactly 2 distinct integers: 1 and 2.
```



**Note:**

1. The `n` and `k` are in the range 1 <= k < n <= 104.



## 代码实现与分析

```c++
#include <vector>
#include <set>
#include <cstdio>
#include <cmath>
using namespace std;
/*
    再来分析一遍题目：1-n是n个数，两个之间一个间距：间距个数为k .     ( 1 <= k < n < 10^4 )

    leecode上解题思路：
    (1).当 k = n-1的时候，例如 n = 9, k=8的时候。
    这时候start from i = 1 ,j = n;
          分别取  j,i  ; 
          然后 j++,i-- ;
    => 9   8   7   6   5           
         1   2   3   4 
    => 9 1 8 2 7 3 6 4 5
diff=   8 7 6 5 4 3 2 1

    (2)当k属于 [1,n-1)的时候，这时候我们可以先列出k-1个（j,i）
    比如 n = 9 , k=5时，
    => 9 1 8 2 7 [3] [4] [5] [6]  wrong Answer   :(
    => 1 9 2 8 3 [4] [5] [6] [7]  corrent Answer :) 
    当 n=9, k=4的时候
    => 9 1 8 2 [3] [4] [5] [6] [7]  corrent Answer :) 

    所以我们需要让最后一个元素是i，然后后续为(i,j)所有元素升序，所有差值均为1
    最后一个元素就是第k个元素，从i中获取

    AB AB AB A  || BA BA BA
    第一个的位置可以使用 k%2 定位  ==1 则从i开始，否则从j开始。 使用k--,刚好交替迭代；

*/

class Solution {
public:
    vector<int> constructArray(int n, int k) {
        //printf("n=%d,k=%d\n",n,k);
        vector<int> ans;
        for(int i=1,j=n; i<=j;){
            if(k>0){
                if(k%2)
                    ans.push_back(i++);
                else
                    ans.push_back(j--);
                k--;
            }
            else
            {
                 ans.push_back(i++);
            }
            
        }

        return ans;
    }
    vector<int> constructArray_1(int n, int k) {
        vector<int> ans;
        vector<int> pailei,duilei;
        for(int i=1; i<=n; i++)
            duilei.push_back(i);
        quanpailei(pailei,duilei,n,k,ans);
        return ans;
    }

   void quanpailei(vector<int> pailei,vector<int> duilei,int n,int k,vector<int> &ans){
        if(pailei.size()==n){
            set<int> Set;
            for(int i=0; i<n-1;i++){
                //printf("%d ",pailei[i]);
                Set.insert(abs(pailei[i+1]-pailei[i]));
            }
            //printf("\n");
            if(Set.size()==k){
                ans=pailei;
                
            }
            return;
        }
        for(int i=0; i<duilei.size(); i++){
            int z=duilei[i];
            pailei.push_back(z);
            duilei.erase(duilei.begin()+i);
            quanpailei(pailei,duilei,n,k,ans);
            duilei.insert(duilei.begin()+i,z);
            pailei.erase(pailei.end()-1);
        }
    }
};

int main(){
    Solution s;
    vector<int> ans=s.constructArray(3,2);
    for(int i=0; i<ans.size(); i++){
        printf("%d ",ans[i]);
    }
    printf("\n");
    return 0;
}
```

