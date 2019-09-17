## [0153]Find Minimum in Rotated Sorted Array[旋转数组中找到最小值]

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example 1:**

```
Input: [3,4,5,1,2] 
Output: 1
```

**Example 2:**

```
Input: [4,5,6,7,0,1,2]
Output: 0
```

数组升序`[1,2,3,4,5,6,7]`  ，然后绕着某空闲旋转得到 `[5,6,7,1,2,3,4]`

可以看成是 放在一个顺时针的盘子里面，然后盘子旋转之后，再次顺时针取出来的结果。

## 实现代码：

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left=0,right=nums.size()-1;
        while(left<=right){
            if(nums[left]<=nums[right]) return nums[left];
            else{
                int mid=(left+right)/2;
                if(nums[left]>nums[mid]){
                    right=mid;
                    left++;
                }
                else{
                    left=mid+1;
                }
            }
        }
        return 0;
    }
};
```

这边存在的一个主要问题是：当元素为一个的时候，如何直接返回。



下面思路比较给力：

终止条件是二分搜索只有一个元素的时候。

`num[mid]>=num[start]`  当最大值是第一个元素或者在第一次上升元素中的时候；且只有一个上升元素的时候。

```C++
int findMin(vector<int> &num) {
        int start=0,end=num.size()-1;
        while (start<end) {
            if (num[start]<num[end])
                return num[start];
            
            int mid = (start+end)/2;
            
            if (num[mid]>=num[start]) {
                start = mid+1;
            } else {
                end = mid;
            }
        }
        return num[start];
    }
```

