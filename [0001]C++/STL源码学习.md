

# STL源码学习

## 概述

by 侯捷 

>  使用一个东西，却不明白它的原理并不高明。—— 林语堂 

`level1 `学习的目标：心中自有丘壑。

需要清除它在内存里是长什么样子，它是怎么拓展的， 当你放一百万个元素的时候，它长得是什么样子的图。

才能够决定或者判断，使用哪种容器，哪一种算法，会能够达到最好的效果。

`level2`学习目标： 良好的使用C++标准库。

两个名词解析：

C++ Standard Library `==>`C++ 标准库

Standard Template Library `==>` STL 标准模板库

```C++
#include <iostream>
#include <vector>
```

所有的新式的组件封装于namespace std;    比如vector的全名是std::vector 使用方法：

```
using namespace std;
using std::cout;
```

## 重要的网站和网页：

[www.cplusplus.com](www.cplusplus.com)

[en.cppreference.com](en.cppreference.com)

[gcc.gnu.org](gcc.gnu.org)





## STL 六大组件（Components）:

- 容器（Containers）
- 分配器（Allocators）
- 算法（Algorithm）
- 迭代器（Iterators）
- 适配器（Adapaters）
- 仿函数（Functors）

<img src="STL源码学习.assets/1570510769613.png" alt="1570510769613" style="zoom: 33%;" />



```C++
int main(){
    int ia[ 6 ] ={27, 210,12,47,109,83};
    vector<int allocator<int>> vi(ia,ia+6);
    cout << count_if(vi.begin(),vi.end(),
                    notl(bind2nd(less<int>(),40)));
	return 0;
}
//begin ,end 迭代器
//count_if 算法
//notl 不小于         function adapter适配器 negator
//bind2nd           function adapter适配器 binder    //网友说14弃用了？没有验证。
//less<int>()       function  Objec仿函数
//40               predicate 
```

常见的复杂度Complexity Big-oh：

![1570511698705](STL源码学习.assets/1570511698705.png)

> 复杂度要成立，只有工业级的数量才能成立。

## 前闭后开的区间。

![1570511921810](STL源码学习.assets/1570511921810.png)

iterator是泛华的指针。

range-based for statement.(since C++11)

```C++
for(decl:coll){
    statement;
}
for(int i:{2,3,4,5,6,7}){
	cout<< i<<endl;
}
for(auto elem : vec){
    cout << elem << "\n";
}
for(auto& elem :vec){
    elem *= 3;
}
```





Generic Programming (GP,泛型编程) 

就是使用template(模板)来。

## 关联容器

```c++
multiset
unorder_multiset
multimap 
unorder_multimap
```

RB-tree

