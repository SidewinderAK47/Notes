选择一个数后，随机返回一个该数对应的下表。

["Solution","pick"]     =>    [[[1,2,3,3,3]],[3]]

返回 2 或3 或4

我的解决方式是最简单的：

​	将相同元素放入一个map之内。：

```C++
#include <cstdio>
#include <map>
#include <vector>
#include <random>
using namespace std;

class Solution {
    map<int,vector<int> > Map;
public:
    Solution(vector<int>& nums) {
        for(int i=0; i<nums.size(); i++){
            Map[nums[i]].push_back(i);
        }
    }
    
    int pick(int target) {
        return Map[target][rand()%Map[target].size()];
    }
};
```

