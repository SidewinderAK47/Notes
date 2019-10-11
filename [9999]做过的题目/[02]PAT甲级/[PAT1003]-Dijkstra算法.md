# [PAT1003]-迪杰斯特拉算法

## 算法原理

迪杰斯特拉算法主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止。

> 网友的理解：好比从s节点一个环，首次碰到最近的就扩散。
>
> 网友理解理解：

自己的理解：

1. 首先，引入了一个辅助向量 `dist`, 每个分量`dist[i]`用来估计到起始点s距离。注：初始化的时候，只有一个小圆圈，`dist[s]=0` 其它为无穷。在算法执行过程中，`dist`的值是不断在逼近最终结果。（初始化阶段）；
2. 每次循环，从未确定最短路径的节点中`dist` 中选择一个最短路径，作为当前确定的最短路径的节点v[k]。
3. 确定v[k]的最短路径之后，更新估计的最短路径距离`dist[v]=max(dis[v],dist[u]+e[u][v])`。

>  求最短路径步骤
>
> 算法步骤如下：
>
> G={V,E}
>
> \1. 初始时令 S={V0},T=V-S={其余顶点}，T中顶点对应的距离值
>
> 若存在<V0,Vi>，d(V0,Vi)为<V0,Vi>弧上的权值
>
> 若不存在<V0,Vi>，d(V0,Vi)为∞
>
> \2. 从T中选取一个与S中顶点有关联边且权值最小的顶点W，加入到S中
>
> \3. 对其余T中顶点的距离值进行修改：若加进W作中间顶点，从V0到Vi的距离值缩短，则修改此距离值
>
> 重复上述步骤2、3，直到S中包含所有顶点，即W=Vi为止

## 题目描述

As an emergency rescue team leader of a city, you are given a special map of your country. The map shows several scattered cities connected by some roads. Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map. When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains 4 positive integers: *N* (≤500) - the number of cities (and the cities are numbered from 0 to *N*−1), *M* - the number of roads, *C*1 and *C*2 - the cities that you are currently in and that you must save, respectively. The next line contains *N* integers, where the *i*-th integer is the number of rescue teams in the *i*-th city. Then *M* lines follow, each describes a road with three integers *c*1, *c*2 and *L*, which are the pair of cities connected by a road and the length of that road, respectively. It is guaranteed that there exists at least one path from *C*1 to *C*2.

### Output Specification:

For each test case, print in one line two numbers: the number of different shortest paths between *C*1 and *C*2, and the maximum amount of rescue teams you can possibly gather. All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

### Sample Input:

```in
5 6 0 2
1 2 1 5 3
0 1 1
0 2 2
0 3 1
1 2 1
2 4 1
3 4 1
```

### Sample Output:

```out
2 4
```

## 题目分析

题目概要：作为一个应急救援队队长，你有一张你们国家的地图。题图上有多座城市，城市之间有一些路相连接。你的目标是尽快从你的城市赶到你需要救援的城市，以及在尽快的前提下，沿路尽可能多的叫一些在路上经过城市的救援队。

这确实是一个最短路径问题



## 代码实现

```c++
#include <stdio.h>
#include <algorithm>

#define INF 1000000
using namespace std;
int main(){
    int N,M,C1,C2; //N<=500
    int dist[510]; //起点到终点距离
    int e[510][510]; //边权重，表示路径长度
    int num[510] ;   //最短路径条数目
    int weight[510];  //点的权重，表示该城市救援队数目
    int visited[510]={false}; //表示该点是否被访问过。
    int w[510];     //起点到终点的救援队数目。
    int prev[510];
    fill(dist,dist+510,INF);
    //
    fill(e[0],e[0]+510*510,INF);

    scanf("%d %d %d %d",&N,&M,&C1,&C2);
    for(int i=0; i< N; i++){
        scanf("%d",&weight[i]);
    }
    int a,b,c;
    for(int i=0; i<M; i++){
        scanf("%d %d %d",&a,&b,&c);
        e[a][b]=e[b][a]=c;
    }
    dist[C1]=0; //起点到终点i的距离
    w[C1]=weight[C1]; //起点到终点的i的救援队数目，点权重和
    num[C1]=1;  //起点到终点i的最短路径数目
    prev[C1]=-1;

    for(int i=0; i<N; i++){
        //u获取，当前更新后【未标记】估计距离最小的点，为s到当前点的最短距离
        int u=-1, minn=INF; 
        for(int j=0; j<N; j++){
            if(visited[j]==false&& dist[j]<minn){
                u=j;
                minn=dist[j];
            }
        }
        if(u==-1) break;  //无法继续拓展，结束
        visited[u] = true;
        
        for(int v=0; v<N; v++){
            //printf("%d ",e[u][v]);
            if(visited[v]==false &&e[u][v]!=INF){
                if(dist[u]+e[u][v]<dist[v]){
                    dist[v]=dist[u]+e[u][v];
                    w[v]=w[u]+weight[v];
                    num[v]=num[u];
                    prev[v]=u;
                }
                else if(dist[u]+e[u][v]==dist[v]){
                    num[v]=num[v]+num[u];      //原来路径数+现在路径数
                    //prev[v]=u;
                    if(w[u]+weight[v]>w[v]){
                        w[v]=w[u]+weight[v];
                    }
                }
            }
        }
    }

    printf("%d %d\n",num[C2],w[C2]);

    while(C2!=-1){
        printf("%d ",C2);
        C2=prev[C2];
    }

    return 0;
}

```

