



## 解题思路分析：来自柳神

[https://github.com/liuchuo/PAT/blob/master/AdvancedLevel_C%2B%2B/1032.%20Sharing%20(25).cpp](https://github.com/liuchuo/PAT/blob/master/AdvancedLevel_C%2B%2B/1032. Sharing (25).cpp)

解题思路：

每个单词都单独存储在一个内存空间之中。

如果第一个链表和第二个链表指向同一个内存空间，那么他们后面都应该是一样的。

所以思路比较简单，先遍历第一个链表所有的元素，并且给所有访问过得元素块添加访问标记；

遍历第二条链表的时候，如果访问过程中存在访问过得元素，那么直接输出当前元素的地址。

否则，输出-1



## 代码实现：

```C++
#include <cstdio>
#include <vector>
#include <stack>
using namespace std;
struct node{
    char letter;
    int next;
    bool flag;
};
vector<node> nodes(99999);
int main(){
    int s1,s2,n;
    scanf("%d %d %d",&s1,&s2,&n);

    int addr,next;
    char c;
    for(int i=0; i<n; i++){
        scanf("%d %c %d",&addr,&c,&next);
        nodes[addr]={c,next,false};
    }
    for(int i=s1; i!=-1; i=nodes[i].next){
        nodes[i].flag = true;
    }
    for(int i=s2; i!=-1; i=nodes[i].next){
        if(nodes[i].flag== true){
            printf("%05d",i);
            return 0;
        }
    }
    printf("-1");


    return 0;
}
```

