multimap & multiset查找元素



multimap和multiset类型---equal_range、lower_bound 和upper_bound函数



原文链接：https://blog.csdn.net/hlsdbd1990/article/details/46501391

## 简介

- multimap和multiset 类型对应的头文件也是map和set 头文件
- multimap和multiset 所支持的操作分别与map和set 操作相同，只有一个例外：multimap 不支持下标操作。不能对multimap对象使用下标操作，引用在这类容器中，某个键可能对应多个值。
- 关联容器中map 和set 的元素是按顺序存储的。multimap和multiset也一样。因此，在multimap和multiset容器中，如果某个键对应多个实例，则这些实例在容器中将相邻存放。
- count 函数求出某键出现的次数，find 操作返回一个迭代器，指向第一个拥有正在查找的键的实例
- 查找元素
  - 使用find和count操作
  - equal_range函数
  - lower_bound 和upper_bound函数

## **简介**

- map 和set 容器中，一个键只能对应一个实例，而multimap和multiset类型则允许一个键对应多个实例。
- multimap和multiset 类型对应的头文件也是map和set 头文件、
- multimap和multiset 所支持的操作分别与map和set 操作相同，只有一个例外：multimap 不支持下标操作。不能对multimap对象使用下标操作，引用在这类容器中，某个键可能对应多个值。

## **一、10.5.1 元素的添加和删除—-insert 和erase**

- insert 和erase 操作同样适用于multimap和multiset 容器，实现元素的添加和删除。

```C++
//定义一个multimap容器，将作者映射到他们所写的书的书名上。
authors.insert(make_pair(string("Barth,John"),string("Sot-weed Factor")));//这样的映射可以为一个作者存储多个条目
authors.insert(make_pair("Barth,John","Sot-weed Factor"));//也可以写成这样

authors.insert(make_pair(string("Barth,John"),string("lost in the eunhouse")));//这样的映射可以为一个作者存储多个条目12345
//带有一个键参数的erase版本将删除拥有该键的所有元素，并返回删除元素的个数。
//而带有一个或一对迭代器参数的版本只删除指定的元素，并返回void类型

multimap<string,string> authors;//定义一个multimap容器
string search_item("Kazuo Ishiguro");//定义一个string对象 
multimap<string,string>::size_type cnt=authors.erase(search_item);//multimap<string,string>类型的size_type
//删除拥有键名为Kazuo Ishiguro的元素
12345678
#include <iostream>
#include<string>
#include<map>
using namespace std;
int main()
{
  	multimap<string,string> authors1;
	string name1("hehaitao");
	authors1.insert(make_pair(name1,"jianzhioffer"));//使用insert函数来插入元素
	authors1.insert(make_pair(name1,"c++ primer"));//
	multimap<string,string>::iterator iter=authors1.find(name1);
	cout<<iter->second<<endl;
	iter++;
	cout<<iter->second<<endl;
    return 0;
}


12345678910111213141516171819
```

## **二、10.5.2 在multimap和multiset中查找元素**

- 关联容器中map 和set 的元素是按顺序存储的。multimap和multiset也一样。因此，在multimap和multiset容器中，如果某个键对应多个实例，则这些实例在容器中将相邻存放。
- 迭代器遍历multimap和multiset 容器时，可保证依次返回特定键所关联的所有元素。
- 在map和set 容器中查找一个元素很简单—-元素要么在要么不在容器中。但对于multimap和multiset，过程就复杂多：某键对应的元素可能出现多次。
- 上述问题可用三种策略解决，三种策略基于一个事实—-在multimap 中，同一个键所关联的元素必然相邻存放。

### 1.查找元素的三种策略

#### 1.1 查找元素的三种策略—–使用find和count操作

- **count 函数求出某键出现的次数，find 操作返回一个迭代器，指向第一个拥有正在查找的键的实例：**

```c++
string search_item("Alain de Botton");
typedef multimap<string,string>::size_type sz_type;
sz_type entries=authors.count(search_item);
multimap<string,string>::iterator iter=authors.find(search_item);


for(sz_type cnt=0;cnt!=entries;++cnt,++iter)
//因为键相同元素是连续存放的
cout<<iter->second<<endl;
cout<<*iter.second<<endl;
1234567891011
```

#### 1.2查找元素的三种策略—-lower_bound 和upper_bound函数

- 另一个简洁的方法是使用两个未曾见过的关联容器的操作：lower_bound 和upper_bound。
  - 下面这些操作适用于所有的关联容器，也可用于普通的map和set容器，但更常用于multimap 好multiset。

-所有这些操作都需要传递一个键，并返回一个迭代器。

![这里写图片描述](multimap和multiset类型equal_range、lower_bound 和upper_bound函数.assets/20150615134602071)

- 在同一键上调用lower_bound 和upper_bound，将产生一个迭代器范围，指示出该键所关联的所有元素。如果该键在容器中存在，则会获得两个不同的迭代器：lower_bound 返回的迭代器指向该键关联的第一个实例，而upper_bound返回的迭代器则指向最后一个实例的下一个位置。如果该键不在multimap 中，这两个操作将返回同一个迭代器，指向依据元素的排列顺序该键应该插入的位置。

  -当然，这些操作返回的也可能是容器自身的超出末端迭代器，如果所查找的元素拥有multimap容器中最大的键，那么在该键上调用upper_bound将返回超出末端迭代器。如果所查找的键不存在，而且比multimap容器中所有的键都大，则lower_bound也将返回超出末端迭代器。

  - lower_bound返回的迭代器不一定指向拥有特定键的元素。如果该键不在容器中，则lower_bound返回在保持容器元素顺序的前提下该键应被插入的第一个位置。
  - 这两个操作不会说明键是否存在，其关键之处在于返回值给出了迭代器的范围

```C++
typedef multimap<string,string>::iterator authors_it;
authors_it beg=authors.lower_bound(search_item);//定义迭代器
authors_it end=authors.upper_bound(search_item);
while(beg!=end){
    cout<<beg->second<<endl;
    ++beg;
}1234567
```

#### **1.3查找元素的三种策略—-equal_range函数**

- 解决上述问题更加直接的方法是：调用 equal_range函数来取代调用upper_bound和lower_bound函数。equal_range函数返回存储一对迭代器的pair对象。如果该值存在，则pair对象中的第一个迭代器指向该键关联的第一个实例，第二个迭代器指向该键关联的最后一个实例的下一个位置。如果找不到匹配的元素，则pair对象中的两个迭代器都将指向此键应该插入的位置。

```c++
pair<authors_it,authors_it> pos=authors.equal_range(search_item);//定义一个pair 类型，该类型存放的都是迭代器
while(pos.first!=pos.second){//pair的第一个成员不等于第二个成员，即两个迭代器不等
	cout<<pos.first->second<<endl;
    ++pos.first;
}
```