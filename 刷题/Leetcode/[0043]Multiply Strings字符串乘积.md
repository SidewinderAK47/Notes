46. Permutations

Medium

Given a collection of **distinct** integers, return all possible permutations.

**Example:**

```C++
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

其实这题目就是全排列，用代码实现的过程。

思考：全排列有一个性质：一组元素的全排列 =  选择一个元素+剩余元素的全排列的过程。

实现：是一个回溯算法，每次深层递归结束后，会恢复现场。

```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> arr;
        function(nums,arr,ans);
        return ans;
    }
    
    void function(vector<int> nums,vector<int> &arr, vector<vector<int>>& answer){
        if(nums.empty()){
            vector<int> t(arr);
            answer.push_back(t);
        }
        for(int i=0; i<nums.size(); i++){
            int k=nums[i];
            arr.push_back(k);
            
            nums.erase(nums.begin()+i);
            function(nums,arr,answer);
            //arr & answer are reference ,but nums is ord.
            // for the reason,inside the loop ,nums will not influence the outside nums.
            arr.pop_back();
            nums.insert(nums.begin()+i,k);
        }
    }
    
};
```





