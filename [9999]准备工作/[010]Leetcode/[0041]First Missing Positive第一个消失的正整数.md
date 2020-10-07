# 41 First Missing Positive

Hard

## 1 题目

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Note:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

## 2 分析：

要求：N的时间复杂度，固定的空间。

给定的数组是一个无序的，找到缺少的最小的正整数。

解题思路来源leecode most voted  solution.

> 主要思路是：将满足条件`1<=element<=n`的元素，放置到其对应的位置。每个位置的元素需要不停交换，直到所有的元素都交换到其位置。注意结束条件，当前元素，当前位置就是其位置的条件，也是结束条件。

```C++
#include <cstdio>
#include <vector>
using namespace std;
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n=nums.size();
        //题目：求最小的不存在的正整数
        //使用原来向量组的空间，不额外的开辟空间
        //将每个元素如果在向量能够表示的空间内，
        for(int i = 0; i < n; ++i){
            //一直交换到所有的元素都满足要求为止。
            while(nums[i]>0&&nums[i]<=n && nums[nums[i]-1]!=nums[i]){
                swap(nums[nums[i]-1],nums[i]);
            }
        }
        for(int i=0; i<n; i++){
            if(nums[i]!=i+1)
                return i+1;
        }
        return n+1;
    }
};
```



