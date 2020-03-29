# list链表排序的实现。

转载来源： https://www.cnblogs.com/avota/p/5388865.html 

我自己看侯捷老师翻译的《STL源码剖析》中链表list章节中的`list::sort()`源码的时候看了很久没有看懂，应为之前的文字描述为使用了`Quick Sort`，更是把我弄的稀里糊涂的。后来去网上找找，是不是我这个问题应该其他人也遇到过了，答案是确定的，其他人也遇到了，并且我找到了这样一篇有助于我理解的博客。

## 摘录于《STL源码剖析》list::sort源码

```C++
template <class T, class Alloc>
void list<T, Alloc> :: sort(){
    // 判断链表是否为空或者只有一个元素
    //可以使用size()==0||size()==1 等价判断，虽然也可以但是比较慢。
    if(node->next == node || link_type(node->next)->next == node){
        return;
    }
   
    list<T, Alloc> carry;
    list<T, alloc> counter[64];
    int fill = 0;
    while(!empty()){
        carry.splice(carry.begin(), *this, begin());
        int i = 0;
        while(i < fill && !counter[i].empty()){
            counter[i].merge(carry);
            carry.swap(counter[i++]);
        }
        carry.swap(counter[i]);
        if(i == fill){
            ++fill;
        } 
    }
    
    for(int i = 1; i < fill; ++i){
        counter[i].merge(counter[i-1]);
    }
    swap(counter[fill-1]);
}
```

这边最关键的一点是没有在这里面明确的 比较大小 ， 而是使用了merge函数，作用：是将两个非减序数组，归并成一个数组。

## list.sort()的外部等价实现

```C++
#include <list>
#include <iostream>

using namespace std;
// list<T>.sort()等价外部实现，用到了归并排序的算法思想
void sortList(list<int> &a) {
	if (a.size() <= 1) {//空链表或者是链表只有一个元素，不用排序by wxd
		return;
	}
	
	list<int> carry;       // 辅助链表，用于从a中提取元素以及临时保存两个链表的合并结果
	list<int> counter[64]; // 保存着当前每一个归并层次的结果, i号链表保存的元素个数为2的i次方或者0
	int fill = 0;          // 表示当前最大归并排序的层次，while循环之后fill变成log2(a.size())

	while (!a.empty()) {
		carry.splice(carry.begin(), a, a.begin()); // 将链表a中的第一个元素移动至carry开头
		int i = 0;
		// 从小往大不断合并非空归并层次直至遇到空层或者到达当前最大归并层次
		while (i < fill && !counter[i].empty()) {
			counter[i].merge(carry);    // 链表合并，结果链表是有序的，必须保证合并前两个链表是有序的
			carry.swap(counter[i++]);   // 链表元素互换
		}
		carry.swap(counter[i]);
		if (i == fill) {       // i到达当前最大归并层次，说明得增加一层
			++fill;
		}
	}

	for (int i = 1; i < fill; ++i) {  // 将所有归并层次的结果合并得到最终结果counter[fill - 1]
		counter[i].merge(counter[i - 1]);
	}
	a.swap(counter[fill - 1]);
}

int main() {
	list<int> test;
	test.push_back(8);
	test.push_back(6);
	test.push_back(520);
	test.push_back(27);
	test.push_back(124);
	test.push_back(214);
	test.push_back(688);
	test.push_back(12);
	test.push_back(36);
	cout << "排序前" << endl;
	for (auto i = test.begin(); i != test.end(); i++) {
		cout << *i << " ";
	}
	cout << endl << "排序后：" << endl;
	sortList(test);
	for (auto i = test.begin(); i != test.end(); i++) {
		cout << *i << " ";
	}
	cout << endl;
	int k;
	cin >> k;
	return 0;
}
```

​    算法的巧妙之处在于外层while循环下counter链表数组的维护，下面我们就用例子a(8, 6, 520, 27, 124, 214, 688, 12, 36 )来跟踪counter的变化。事先约定，null表示list不含元素，下面所说的第i次循环之后均指外层while的。a的元素个数为9，归并层次最多到达第4层，故counter[3]之后的就不显示了, 它们的值均为null。

| 第i次循环之后 | counter[0] | counter[1] | counter[2] |        counter[3]         |
| :-----------: | :--------: | :--------: | :--------: | :-----------------------: |
|       0       |     8      |    null    |    null    |           null            |
|       1       |    null    |    6,8     |    null    |           null            |
|       2       |    520     |    6.8     |    null    |           null            |
|       3       |    null    |    null    | 6,8,27,520 |           null            |
|       4       |    124     |    null    | 6,8,27,520 |           null            |
|       5       |    null    |  124,214   | 6,8,27,520 |           null            |
|       6       |    688     |  124,214   | 6,8,27,520 |           null            |
|       7       |    null    |    null    |    null    | 6,8,12,27,124,214,520,688 |
|       8       |     36     |    null    |    null    | 6,8,12,27,124,214,520,688 |



前3次循环的具体运行过程如下：

- 第0次外层循环，carry取得a列表头元素8，i == fill == 0 无法进入内层循环，之后carry与counter[0]交换，counter[0] 变为8， fill变为1；
- 第1次外层循环， carry取得a列表头元素6，counter[0]不为空故进入内层循环，合并carry和counter[0]，内层一次循环之后counter[0]变为null， carry变为(6,8), i == fill == 1退出内层循环。然后carry与counter[1]交换，最后counter[1]变为(6, 8), fill 变为2；
- 第2次外层循环， carry取得a列表头元素520，counter[0]为空无法进入内层循环，之后carry与counter[0]交换，counter[0] 变为520， fill的值不变；
- 第3次外层循环，carry取得a列表头元素27，进入内层while循环，先是发现counter[0]不为空，故与其合并，合并之后carry变为(27, 520), counter[0]变为null，然后进入下一次内层循环发现counter[1]不为空，故与其合并，合并之后carry变为(6,8,27,520)， counter[1]变为null，i == fill == 2退出内层循环。最后carry与counter[2]互换，counter[2]变为(6,8,27,520)，fill变为3.

之后的循环过程类似，最后将counter[0]至counter[8]的结果合并即为结果。此算法的时间复杂度为O(N*logN)，空间复杂度为O(N).

## 总结

​      传统归并排序使用先二分后调用递归函数的步骤，应用对象主要是普通数组和vector数组，这两者的共同点在于可以在O(1)的时间内找到中点。但分析list数据结构可知，寻找其中点需要O(N)复杂度，故不大适合使用传统归并排序的思想。后来不知哪位牛人想到了利用二进制的进位思想，结合一个list数组保存各个归并层次的结果，最终实现了非递归版的归并排序，此想法也可以用在普通数组和vector数组上，具体实现以后有时间再写。