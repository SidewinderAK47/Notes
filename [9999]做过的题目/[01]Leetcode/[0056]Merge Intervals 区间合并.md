## 56 Merge Intervals 区间合并   【区间合并】

Medium

## 题目描述

Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

解题思路：

区间合并的思路：给出一堆区间，区间之间有覆盖，包含关系。

首先根据区间的头，进行排序。获取第一个区间。

然后遍历后续的区间：

后续区间开始 `> `  上一个区间末尾 `=>` 区间没有重叠区域，为新区间。

后续区间开始 `<=` 上一个区间末尾 `=>` 两个区间有重叠，重新计算上一个区间结尾。

> 通过排序，就把一个复杂的问题简单化了。

## 代码实现

``` C++
bool myCmp(vector<int> &a,vector<int> &b)
{
    return a[0]<b[0];
}
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int> > ans;
        if(intervals.empty()) return ans;
        sort(intervals.begin(),intervals.end(),myCmp);
        ans.push_back(intervals[0]);
        for(int i=1; i<intervals.size(); i++){
            if(ans.back()[1]<intervals[i][0])
                ans.push_back(intervals[i]);
            else
                ans.back()[1]=max(ans.back()[1],intervals[i][1]);
        }
        return ans;
    }
};
```

