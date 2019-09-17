199. Binary Tree Right Side View

Medium

Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

**Example:**

```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <
```

## 我的解决方式：

思路比较简单，就是使用队列进行层次遍历，输出每层的最后一个元素。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        queue<TreeNode*> Q;
        if(root==NULL){
            return ans;
        }
        Q.push(root);
        Q.push(NULL);
        TreeNode *prev,*cur=NULL;
        while(!Q.empty()){
            prev=cur;
            cur=Q.front();
            Q.pop();
            while(cur!=NULL){
                if(cur->left!=NULL) Q.push(cur->left);
                if(cur->right!=NULL) Q.push(cur->right);
                prev=cur;
                cur=Q.front();
                Q.pop();
            }
            //cout<<prev->val<<endl;
            ans.push_back(prev->val);
            if(!Q.empty()){
                Q.push(NULL);
            }
        }
        return ans;
    }
};
```

我使用NULL作为每一层的分割。

遍历到NULL的时候，说明当前层所有元素遍历结束了。然后将它们的子元素添加到队列末尾，再在队列末尾添加一个NULL标记。

``` C++
[1,2,3,NULL,4,5]
```

添加到队列中不删除的时候：

```C++
1  mark  2   3  mark 4 mark  5   mark
```

就是遍历当前层，添加新层的时候都会在新层最后添加一个mark标记最为新层结束标记。

但是如果遍历完当前层，没有新元素加进来。也就是，下一层已经没有元素了的时候，最后就不许再添加元素了。



## 大佬的解题思路，真的很优秀。很值得借鉴。

1.Each depth of the tree only select one node.

2. View depth is current size of result list.

Here is the code:

```java
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        rightView(root, result, 0);
        return result;
    }
    
    public void rightView(TreeNode curr, List<Integer> result, int currDepth){
        if(curr == null){
            return;
        }
        if(currDepth == result.size()){
            result.add(curr.val);
        }
        
        rightView(curr.right, result, currDepth + 1);
        rightView(curr.left, result, currDepth + 1);
        
    }
}
```

优秀的想法：使用了非主流的 中→右→左的遍历。DFS思路。同时在遍历时候带上层数参数，我们就能知道它在第几层。