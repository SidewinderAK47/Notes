51. N-Queens 回溯 ，矩阵对角线表示

Hard

96944Share

The *n*-queens puzzle is the problem of placing *n* queens on an *n*×*n* chessboard such that no two queens attack each other.

![img]([0051] N-Queens.assets/8-queens.png)

Given an integer *n*, return all distinct solutions to the *n*-queens puzzle.

Each solution contains a distinct board configuration of the *n*-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

**Example:**

```
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

分析：n皇后问题，是在n*n的棋局上放置n个Queen，每个Queen会攻击同一行，列对角线上的元素。在棋局上放n个皇后，使得n个皇后不会相互攻击。

主要解题思路，n*n的棋局上，有n行，n列，2n-1条lup,2n-1条rup 也就是说每条线上只能有一个皇后。

行：0~n-1   列 0~n-1

主对角线 `rup: i+j `  反对角线：`lup` 可以表示成： `i-j+n-1` 

还有就是使用了回溯算法，回溯就是结合递归和for循环，进行深度优先搜索，搜索完毕的时候，进行恢复原样。注意进行调用回溯函数的时候，传递引用和传值得合适性。

```C++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<int> col(n);
        vector<int> rup(2*n-1);
        vector<int> lup(2*n-1);
        vector<int> index(n);
        vector<vector<string>> ans;
        trackback(col,rup,lup,index,0,ans,n);
        return ans;
    }
    void trackback(vector<int> &col,vector<int> &rup,vector<int> &lup, 
      vector<int> &index,int j,
      vector<vector<string>>  &ans,int &n){
        if(j==n){
            vector<string> tmp;
            for(int i=0; i<n; i++){
                string arr(n,'.');
                arr[index[i]]='Q';
                tmp.push_back(arr);
            }
            ans.push_back(tmp);
        } else{
            for(int i=0; i<n; i++){
                if(col[i]!=1&&rup[i+j]!=1&&lup[i-j+n-1]!=1){
                    col[i]=1;
                    rup[i+j]=1;
                    lup[i-j+n-1]=1;
                    index[j]=i;
                    trackback(col,rup,lup,index,j+1,ans,n);
                    col[i]=0;
                    rup[i+j]=0;
                    lup[i-j+n-1]=0;
                }
            }
        }
    }
};
```

> 这边j为行，i为列。最好改一下。