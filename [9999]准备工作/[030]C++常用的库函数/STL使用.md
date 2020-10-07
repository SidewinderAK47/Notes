

# 容器（containers）

- 序列式容器（sequence containers）：元素都是可序（ordered），但未必是有序（sorted）

- 关联式容器（associattive containers)



## array

- 用于存储固定数量元素序列的标准容器

- 以内置数组为基础实现

- 支持随机访问迭代器

- array 提供的接口（成员函数）
  迭代器：`begin()`,` end()`, `cbegin()`, `cend()`,` rbegin()`,` rend()`, `crbegin()`, `crend()`
  `size()`, `max_size()`, `empty()`
  `operator[]`, `at()`, `front()`, `back()`
  `data()`
  `fill()`, `swap()`
  array 支持的操作（非成员函数）
  `operator==`, `operator!=`, `operator<`, `operator<=`, `operator>`, `operator>=`

  `swap()`
  `get()`

- 

### size()

返回数组容器中元素的数量。
constexpr size_type size（）noexcept;

```c++
#include <iostream>
#include <array>
int main()
{
    std::array<int, 5> myints;
    std::cout << "size of myints:" << myints.size() << std::endl;
    std::cout << "sizeof(myints):" << sizeof(myints) << std::endl;
    return 0;
}
//输出
//size of myints:5
//sizeof(myints):20
```

### max_size()

返回数组容器可容纳的最大元素数。数组对象的max_size与其size一样，始终等于用于实例化数组模板类的第二个模板参数。
constexpr size_type max_size() noexcept;

```c++
#include <iostream>
 #include <array>
  int main () { 
    std::array<int,10> myints; 
    std::cout << "size of myints: " << myints.size() << '\n';
    std::cout << "max_size of myints: " << myints.max_size() << '\n';
    return 0;
}
//输出
// size of myints: 10
// max_size of myints: 10
```

### empty（）

返回一个布尔值，指示数组容器是否为空，即它的size()是否为0。

`constexpr bool empty() noexcept;`

```c++
#include <iostream> 
#include <array>
int main()
{
    std::array<int, 0> first;
    std::array<int, 5> second;
    std::cout << "first " << (first.empty() ? "is empty" : "is not empty") << '\n';
    std::cout << "second " << (second.empty() ? "is empty" : "is not empty") << '\n';
    return 0;
}
//输出
// first is empty
// second is not empty
```

### operator[]

返回数组中第n个位置的元素的引用。与array::at相似，但array::at会检查数组边界并通过抛出一个outofrange异常来判断n是否超出范围，而array::operator[]不检查边界。

`reference operator[] (size_type n);`

` const_reference operator[] (size_type n) const;`

```c++
#include <iostream>
#include <array>
int main () {
    std::array<int, 10> myarray;
    unsigned int i; // assign some values:
    for(i=0; i<10; i++)
        myarray[i] = i; 
    // print content 
    std::cout << "myarray contains:"; 
    for(i=0; i<10; i++) 
        std::cout << ' ' << myarray[i]; 
    
    std::cout << '\n';
    std::cout <<myarray[10];
     return 0;
}
//输出
// myarray contains: 0 1 2 3 4 5 6 7 8 9
// 59 没有检查边界，乱输出
```

at()

返回数组中第n个位置的元素的引用。与array::operator[]相似，但array::at会检查数组边界并通过抛出一个outofrange异常来判断n是否超出范围，而array::operator[]不检查边界。

`reference at ( size_type n ); `

`const_reference at ( size_type n ) const;`

```c++
#include <iostream>
#include <array>
int main () {
    std::array<int, 10> myarray;
    unsigned int i; // assign some values:
    for(i=0; i<10; i++)
        myarray[i] = i; 
    // print content 
    std::cout << "myarray contains:"; 
    for(i=0; i<10; i++) 
        std::cout << ' ' << myarray.at(i); 
    std::cout << '\n';
    myarray.at(0) = 3;
    for(i=0; i<10; i++) 
        std::cout << ' ' << myarray.at(i); 
    std::cout << '\n';
    std::cout <<myarray.at(10);
     return 0;
}
//输出
// myarray contains: 0 1 2 3 4 5 6 7 8 9
//  3 1 2 3 4 5 6 7 8 9
// terminate called after throwing an instance of 'std::out_of_range'
//   what():  array::at: __n (which is 10) >= _Nm (which is 10)
```

### front()

返回对数组容器中第一个元素的引用。array::begin返回的是迭代器，array::front返回的是直接引用。 在空容器上调用此函数会导致未定义的行为。

`reference front();`

`` const_reference front() const;`

```c++
#include <iostream>
#include <array>
int main () {
     std::array<int,3> myarray = {2, 16, 77}; 
     std::cout << "front is: " << myarray.front() << std::endl;
     std::cout << "back is: " << myarray.back() << std::endl;
    myarray.front() = 100;
    std::cout << "myarray now contains:"; 
    for ( int& x : myarray )
         std::cout << ' ' << x; std::cout << '\n'; 
    return 0;
}
// front is: 2
// back is: 77
// myarray now contains: 100 16 77    
```

### back()

返回对数组容器中最后一个元素的**引用**。array::end返回的是迭代器，array::back返回的是直接引用。 在空容器上调用此函数会导致未定义的行为。
`reference back(); `

`const_reference back() const;`



### data()

返回指向数组对象中第一个元素的指针。
由于数组中的元素存储在连续的存储位置，所以检索到的指针可以偏移以访问数组中的任何元素。
`value_type* data() noexcept;` 

`const value_type* data() const noexcept;`

```c++
#include <iostream>
#include <cstring>
#include <array>
int main () {
     const char* cstr = "Test string"; 
     std::array<char,12> charray; 
     std::memcpy (charray.data(),cstr,12); 
     std::cout << charray.data() << '\n'; 
     return 0;
}
//输出
//Test string
```



### fill(T t)

用val填充数组所有元素，将val设置为数组对象中所有元素的值。

`void fill (const value_type& val);`

```c++
#include <iostream>
#include <array> 
int main () {
    std::array<int,6> myarray;
    myarray.fill(5);
    std::cout << "myarray contains:";
    for ( int& x : myarray) { std::cout << ' ' << x; }
    std::cout << '\n';
    return 0;
}
//输出
//myarray contains: 5 5 5 5 5 5
```

### swap(array<T> & x)

通过x的内容交换数组的内容，这是另一个相同类型的数组对象（包括相同的大小）。
与其他容器的交换成员函数不同，此成员函数通过在各个元素之间执行与其大小相同的单独交换操作，以线性时间运行。

`void swap (array& x) `

`noexcept(noexcept(swap(declval<value_type&>(),declval<value_type&>())));`

```c++
#include <iostream>
#include <array>
int main()
{
    std::array<int, 5> first = {10, 20, 30, 40, 50};
    std::array<int, 5> second = {11, 22, 33, 44, 55};
    first.swap(second);
    std::cout << "first:";
    for (int &x : first)
        std::cout << ' ' << x;
    std::cout << '\n';
    std::cout << "second:";
    for (int &x : second)
        std::cout << ' ' << x;
    std::cout << '\n';
    return 0;
}
//输出
// first: 11 22 33 44 55
// second: 10 20 30 40 50
```



### 各种迭代器

### begin()

### end()

### rbegin()

### rend()

### cbegin()

### cend()

### crbegin()

### crend()



### get（array）：非成员函数

形如：std::get<0>(myarray)；传入一个数组容器，返回指定位置元素的引用。

```C++
template <size_t I，class T，size_t N> T＆get（array <T，N>＆arr）noexcept; 
template <size_t I，class T，size_t N> T && get（array <T，N> && arr）noexcept; 
template <size_t I，class T，size_t N> const T＆get（const array <T，N>＆arr）noexcept;
```

```c++
#include <iostream>
#include <array>
#include <tuple>
int main () {
    std::array<int,3> myarray = {10, 20, 30};
    std::tuple<int,int,int> mytuple (10, 20, 30);
    std::tuple_element<0,decltype(myarray)>::type myelement; //获取元组中第0个元素的类型 元素默认值为 0
     // int myelement myelement = std::get<2>(myarray);
    std::get<2>(myarray) = std::get<0>(myarray);
    std::get<0>(myarray) = myelement;
    std::cout << "first element in myarray: " << std::get<0>(myarray) << "\n";
    std::cout << "first element in mytuple: " << std::get<0>(mytuple) << "\n";
    return 0;
}
//输出
// first element in myarray: 0
// first element in mytuple: 10
```

### relational operators (array)

```c++
（1） template <class T，size_T N> bool operator ==（const array <T，N>＆lhs，const array <T，N>＆rhs）; 
（2） template <class T，size_T N> bool operator！=（const array <T，N>＆lhs，const array <T，N>＆rhs）; 
（3） template <class T，size_T N> bool operator <（const array <T，N>＆lhs，const array <T，N>＆rhs）; 
（4） template <class T，size_T N> bool operator <=（const array <T，N>＆lhs，const array <T，N>＆rhs）; 
（5） template <class T，size_T N> bool operator>（const array <T，N>＆lhs，const array <T，N>＆rhs）; 
（6） template <class T，size_T N> bool operator> =（const array <T，N>＆lhs，const array <T，N>＆rhs）;
```

```c++
#include <iostream>
#include <array>
int main () {
    std::array<int,5> a = {10, 20, 30, 40, 50};
    std::array<int,5> b = {10, 20, 30, 40, 50};
    std::array<int,5> c = {50, 40, 30, 20, 10}; 
    if (a==b)
        std::cout << "a and b are equal\n";
    if (b!=c)
        std::cout << "b and c are not equal\n";
    if (b<c)
        std::cout << "b is less than c\n";
    if (c>b)
        std::cout << "c is greater than b\n";
    if (a<=b)
        std::cout << "a is less than or equal to b\n";
    if (a>=b)
        std::cout << "a is greater than or equal to b\n";
    return 0;
}
//输出
// a and b are equal
// b and c are not equal
// b is less than c
// c is greater than b
// a is less than or equal to b
// a is greater than or equal to b
```





## vector

vector 是表示可以改变大小的数组的序列容器。
就像数组一样，vector 为它们的元素使用连续的存储位置，这意味着它们的元素也可以使用到其元素的常规指针上的偏移来访问，而且和数组一样高效。但是与数组不同的是，它们的大小可以动态地改变，它们的存储由容器自动处理。
在内部，vector 使用一个动态分配的数组来存储它们的元素。这个数组可能需要重新分配，以便在插入新元素时增加大小，这意味着分配一个新数组并将所有元素移动到其中。就处理时间而言，这是一个相对昂贵的任务，因此每次将元素添加到容器时矢量都不会重新分配。
相反，vector 容器可以分配一些额外的存储以适应可能的增长，并且因此容器可以具有比严格需要包含其元素（即，其大小）的存储更大的实际容量。库可以实现不同的策略的增长到内存使用和重新分配之间的平衡，但在任何情况下，再分配应仅在对数生长的间隔发生尺寸，使得在所述载体的末端各个元件的插入可以与提供分期常量时间复杂性。
因此，与数组相比，载体消耗更多的内存来交换管理存储和以有效方式动态增长的能力。
与其他动态序列容器（deques，lists和 forward_lists ）相比，vector非常有效地访问其元素（就像数组一样），并相对有效地从元素末尾添加或移除元素。对于涉及插入或移除除了结尾之外的位置的元素的操作，它们执行比其他位置更差的操作，并且具有比列表和 forward_lists 更不一致的迭代器和引用。

针对 vector 的各种常见操作的复杂度（效率）如下：
• 随机访问 - 常数 O(1)
• 在尾部增删元素 - 平摊（amortized）常数 O(1)}}
• 增删元素 - 至 vector 尾部的线性距离 O(n)}}

`template < class T, class Alloc = allocator<T> > class vector;`

### vector（）

构造函数：

vector<int> vec(n); 默认为0

vector<int> （n, value）;

```c++
#include <iostream>
 #include <vector>
  int main () { 
      // constructors used in the same order as described above: 
      std::vector<int> first(30); //默认为0
       for(std::vector<int>::iterator it = first.begin(); it != first.end(); ++it) 
            std::cout << ' ' << *it; 
        std::cout << '\n';
       // empty vector of ints 
       std::vector<int> second(4, 100); 
       // four ints with value 100 
       std::vector<int> third(second.begin(), second.end());
       // iterating through second 
       std::vector<int> fourth(third); // a copy of third 
       // the iterator constructor can also be used to construct from arrays: 
       int myints[] = {16,2,77,29};
        std::vector<int> fifth(myints, myints + sizeof(myints) / sizeof(int));
        std::cout << "The contents of fifth are:"; 
        for(std::vector<int>::iterator it = fifth.begin(); it != fifth.end(); ++it) 
            std::cout << ' ' << *it; 
        std::cout << '\n';
        return 0;
        
}
```



### ~vector 自动调用

销毁容器对象。这将在每个包含的元素上调用allocator_traits::destroy，并使用其分配器释放由矢量分配的所有存储容量。
~vector();

### vector::operator=

将新内容分配给容器，替换其当前内容，并相应地修改其大小。

```c++
copy (1) 
vector& operator= (const vector& x); 
move (2) 
vector& operator= (vector&& x); 
initializer list (3) 
vector& operator= (initializer_list<value_type> il);
```

```c++
#include <iostream>
#include <vector> 
int main () { 
    std::vector<int> foo (3,0);
    std::vector<int> bar (5,0);
    bar = foo; 
    foo = std::vector<int>(); 
    std::cout << "Size of foo: " << int(foo.size()) << '\n'; 
    std::cout << "Size of bar: " << int(bar.size()) << '\n';
    return 0;
}
//Size of foo: 0 
//Size of bar: 3
```

### vector::size()

返回vector中元素的数量。
这是vector中保存的实际对象的数量，不一定等于其存储容量。

`size_type size() const noexcept;`

```c++
#include <iostream>
#include <vector> 
int main () { 
    std::vector<int> myints;
    std::cout << "0. size: " << myints.size() << '\n';
    for (int i=0; i<10; i++) 
        myints.push_back(i); 
    std::cout << "1. size: " << myints.size() << '\n';
    myints.insert (myints.end(),10,100);
    std::cout << "2. size: " << myints.size() << '\n';
    for(auto &k :myints)
        std::cout<<k<<" ";
    std::cout<<"\n";
    myints.pop_back();
    std::cout << "3. size: " << myints.size() << '\n';
    return 0;
}
//输出
//0. size: 0
//1. size: 10
//2. size: 20
//0 1 2 3 4 5 6 7 8 9 100 100 100 100 100 100 100 100 100 100
//3. size: 19
```

### vector::max_size()

返回该vector可容纳的元素的最大数量。由于已知的系统或库实现限制，
这是容器可以达到的最大潜在大小，但容器无法保证能够达到该大小：在达到该大小之前的任何时间，仍然无法分配存储。
`size_type max_size() const noexcept;`

```c++
#include <iostream>
#include <vector>
int main () { 
    std::vector<int> myvector;
    // set some content in the vector:
    for (int i=0; i<100; i++)
        myvector.push_back(i);
    std::cout << "size: " << myvector.size() << "\n";
    std::cout << "capacity: " << myvector.capacity() << "\n";
    std::cout << "max_size: " << myvector.max_size() << "\n"; 
    return 0; 
}
//输出
// size: 100
// capacity: 128
// max_size: 4611686018427387903
```

### vector::resize(int)

`void resize (size_type n); `

`void resize (size_type n, const value_type& val);`

调整容器的大小，使其包含n个元素。
如果n小于当前的容器size，内容将被缩小到前n个元素，将其删除（并销毁它们）。
如果n大于当前容器size，则通过在末尾插入尽可能多的元素以达到大小n来扩展内容。如果指定了val，则新元素将初始化为val的副本，否则将进行值初始化。
如果n也大于当前的容器的capacity（容量），分配的存储空间将自动重新分配。
注意这个函数通过插入或者删除元素的内容来改变容器的实际内容。

```c++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> myvector;
    // set some initial content:
    for (int i = 1; i < 10; i++)
        myvector.push_back(i);
    myvector.resize(5);
    myvector.resize(8, 100);
    myvector.resize(12);
    std::cout << "myvector contains:";
    for (int i = 0; i < myvector.size(); i++)
        std::cout << ' ' << myvector[i];
    std::cout << '\n';
    return 0;
}
//输出
//myvector contains: 1 2 3 4 5 100 100 100 0 0 0 0
```

### vector::capacity()

返回当前为vector分配的存储空间的大小，用元素表示。这个capacity(容量)不一定等于vector的size。它可以相等或更大，额外的空间允许适应增长，而不需要重新分配每个插入。
`size_type capacity() const noexcept;`

### vector::empty()

返回vector是否为空（即，它的size是否为0）
`bool empty() const noexcept;`

### vector::reserve(n)

请求vector容量至少足以包含n个元素。(调整容量)
如果n大于当前vector容量，则该函数使容器重新分配其存储容量，从而将其容量增加到n（或更大）。
在所有其他情况下，函数调用不会导致重新分配，并且vector容量不受影响。
这个函数对vector大小没有影响，也不能改变它的元素。
void reserve (size_type n);

### vector::shrinktofit（）

要求容器减小其capacity(容量)以适应其尺寸。
该请求是非绑定的，并且容器实现可以自由地进行优化，并且保持capacity大于其size的vector。 这可能导致重新分配，但对矢量大小没有影响，并且不能改变其元素。
`void shrink_to_fit();`

### vector::operator[]

### vector::at

### vector::front

### vector::back

### vector::data

### vector::assign

将新内容分配给vector，替换其当前内容，并相应地修改其大小。
在范围版本（1）中，新内容是从第一个和最后一个范围内的每个元素按相同顺序构造的元素。
在填充版本（2）中，新内容是n个元素，每个元素都被初始化为一个val的副本。
在初始化列表版本（3）中，新内容是以相同顺序作为初始化列表传递的值的副本。
所述内部分配器被用于（通过其性状），以分配和解除分配存储器如果重新分配发生。它也习惯于摧毁所有现有的元素，并构建新的元素。

```c++
range (1) 
template <class InputIterator> void assign (InputIterator first, InputIterator last); 
fill (2) 
void assign (size_type n, const value_type& val);
initializer list (3)
void assign (initializer_list<value_type> il);
```



```c++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> first;
    std::vector<int> second;
    std::vector<int> third;
    first.assign(7, 100); // 7 ints with a value of 100
    std::vector<int>::iterator it;
    it = first.begin() + 1;
    second.assign(it, first.end() - 1);
    // the 5 central values of first
    int myints[] = {1776, 7, 4};
    third.assign(myints+1, myints + 3);
    // assigning from array.
    std::cout << "Size of first: " << int(first.size()) << '\n';
    std::cout << "Size of second: " << int(second.size()) << '\n';
    std::cout << "Size of third: " << int(third.size()) << '\n';
    return 0;
}
// Size of first: 7
// Size of second: 5
// Size of third: 2
```

### vector::push_back()

在vector的最后一个元素之后添加一个新元素。val的内容被复制（或移动）到新的元素。
这有效地将容器size增加了一个，如果新的矢量size超过了当前vector的capacity，则导致所分配的存储空间自动重新分配。

`void push_back (const value_type& val); `

`void push_back (value_type&& val);`

### vector::pop_back()

删除vector中的最后一个元素，有效地将容器size减少一个。
这破坏了被删除的元素。
`void pop_back();`



### vector::insert()

通过在指定位置的元素之前插入新元素来扩展该vector，通过插入元素的数量有效地增加容器大小。 这会导致分配的存储空间自动重新分配，只有在新的vector的size超过当前的vector的capacity的情况下。

由于vector使用数组作为其基础存储，因此除了将元素插入到vector末尾之后，或vector的begin之前，其他位置会导致容器重新定位位置之后的所有元素到他们的新位置。与其他种类的序列容器（例如list或forward_list）执行相同操作的操作相比，这通常是低效的操作。

```c++
single element (1) 
iterator insert (const_iterator position, const value_type& val); 

fill (2) 
iterator insert (const_iterator position, size_type n, const value_type& val); 

range (3) 
template <class InputIterator> iterator insert (const_iterator position, InputIterator first, InputIterator last);

move (4) 
iterator insert (const_iterator position, value_type&& val); 

initializer list (5) 
iterator insert (const_iterator position, initializer_list<value_type> il);
```

```c++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> myvector(3, 100);
    std::vector<int>::iterator it;
    it = myvector.begin();
    it = myvector.insert(it, 200);
    myvector.insert(it, 2, 300);
     // "it" no longer valid, get a new one: 
    it = myvector.begin();
    std::vector<int> anothervector (2,400);
    myvector.insert (it+2,anothervector.begin(),anothervector.end());
    int myarray [] = { 501,502,503 }; 
    myvector.insert (myvector.begin(), myarray, myarray+3);
    std::cout << "myvector contains:";
    for (it=myvector.begin(); it<myvector.end(); it++)
        std::cout << ' ' << *it;
    std::cout << '\n'; 

    std::vector<int> vec {1,2,3};
    vec.insert(vec.end(),{4,5,6});
    
    for (it=vec.begin(); it<vec.end(); it++)
        std::cout << ' ' << *it;
    std::cout << '\n';
    return 0;
}
//输出
// myvector contains: 501 502 503 300 300 400 400 200 100 100 100
//  1 2 3 4 5 6
```

补充：insert 迭代器野指针错误：



### vector::erase（）

从vector中删除单个元素（position）或一系列元素（[first，last））。
这有效地减少了被去除的元素的数量，从而破坏了容器的大小。
由于vector使用一个数组作为其底层存储，所以删除除vector结束位置之后，或vector的begin之前的元素外，将导致容器将段被擦除后的所有元素重新定位到新的位置。与其他种类的序列容器（例如list或forward_list）执行相同操作的操作相比，这通常是低效的操作。

`iterator erase (const_iterator position); `

`iterator erase (const_iterator first, const_iterator last);`

```c++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> myvector;
    // set some values (from 1 to 10)
    for (int i = 1; i <= 10; i++)
        myvector.push_back(i);
    // erase the 6th element
    myvector.erase(myvector.begin() + 5);
    // erase the first 3 elements:
    myvector.erase(myvector.begin(), myvector.begin() + 3);
    std::cout << "myvector contains:";
    for (unsigned i = 0; i < myvector.size(); ++i)
        std::cout << ' ' << myvector[i];
    std::cout << '\n';
    return 0;
}
//Output
//myvector contains: 4 5 7 8 9 10
```

### vector::swap(x)

通过x的内容交换容器的内容，x是另一个相同类型的vector对象。尺寸可能不同。
在调用这个成员函数之后，这个容器中的元素是那些在调用之前在x中的元素，而x的元素是在这个元素中的元素。所有迭代器，引用和指针对交换对象保持有效。
请注意，非成员函数存在具有相同名称的交换，并使用与此成员函数相似的优化来重载该算法。
void swap (vector& x);



### vector::clear()

从vector中删除所有的元素（被销毁），留下size为0的容器。
**不保证重新分配**，并且由于调用此函数， vector的capacity不保证发生变化。强制重新分配的典型替代方法是使用swap：vector<T>().swap(x); // clear x reallocating
void clear() noexcept;

### vector::emplace(position,args)

通过在position位置处插入新元素args来扩展容器。这个新元素是用args作为构建的参数来构建的。
这有效地增加了一个容器的大小。
分配存储空间的自动重新分配发生在新的vector的size超过当前向量容量的情况下。
由于vector使用数组作为其基础存储，因此除了将元素插入到vector末尾之后，或vector的begin之前，其他位置会导致容器重新定位位置之后的所有元素到他们的新位置。与其他种类的序列容器（例如list或forward_list）执行相同操作的操作相比，这通常是低效的操作。
该元素是通过调用allocator_traits::construct来转换args来创建的。插入一个类似的成员函数，将现有对象复制或移动到容器中。

`template <class... Args>`

` iterator emplace (const_iterator position, Args&&... args);`

```c++
#include <iostream>
#include <vector>
struct student{
    student(int id,double money){
        this->id = id;
        this->money =money;
    }
    // 拷贝构造函数
    // student(student & stu){
    //     this->id = stu.id;
    //     this->money =stu.money;
    // }
    /* 移动构造函数*/
    // student(student && stu){
    //     this->id = stu.id;
    //     this->money =stu.money;
    // }
    /*重载赋值运算符*/ 
    // student & operator=(student & stu){
    //     id = stu.id;
    //     money = stu.money;
    //     return *this;
    // }
    int id;
    double money;
};
int main()
{
    std::vector<int> myvector;
    // set some values (from 1 to 10)
    for (int i = 1; i <= 10; i++)
        myvector.push_back(i);
    // erase the 6th element
    myvector.erase(myvector.begin() + 5);
    // erase the first 3 elements:
    myvector.erase(myvector.begin(), myvector.begin() + 3);
    std::cout << "myvector contains:";
    for (unsigned i = 0; i < myvector.size(); ++i)
        std::cout << ' ' << myvector[i];
    std::cout << '\n';

    std::vector<student> vec;
    vec.emplace(vec.begin(), 1, 0.2d);
    vec.emplace_back(2, 0.3d); 
    vec.emplace_back(21, 0.31d);  
    for(auto &k:vec){
        std::cout<<k.id<<"-"<<k.money<<"  ";
    }
    std::cout << '\n';
    return 0;
}
//Output
// myvector contains: 4 5 7 8 9 10
// 1-0.2  2-0.3  21-0.31
```

### vector::emplace_back(position,args)

在vector的末尾插入一个新的元素，紧跟在当前的最后一个元素之后。这个新元素是用args作为构造函数的参数来构造的。

这有效地将容器大小增加了一个，如果新的矢量大小超过了当前的vector容量，则导致所分配的存储空间自动重新分配。
该元素是通过调用allocator_traits :: construct来转换args来创建的。
与push_back相比，emplace_back可以避免额外的复制和移动操作。

`template <class... Args> `

`void emplace_back (Args&&... args);`

### vector::get_allocator

返回与vector关联的构造器对象的副本。

```c++
#include <iostream>
#include <vector>
int main () {
    std::vector<int> myvector;
    int * p;
    unsigned int i;
    // allocate an array with space for 5 elements using vector's allocator: 
    p = myvector.get_allocator().allocate(5); 
    // construct values in-place on the array: 
    for (i=0; i<5; i++) 
        myvector.get_allocator().construct(&p[i],i);
    std::cout << "The allocated array contains:";
    for (i=0; i<5; i++) 
        std::cout << ' ' << p[i]; std::cout << '\n';
    // destroy and deallocate:
    for (i=0; i<5; i++)
        myvector.get_allocator().destroy(&p[i]);
    myvector.get_allocator().deallocate(p,5);
     return 0;
}
//output
// The allocated array contains: 0 1 2 3 4
```



### 各种迭代器

vector::begin
vector::end
vector::rbegin
vector::rend
vector::cbegin
vector::cend
vector::rcbegin
vector::rcend



## list 容器

list 容器是由双向链表实现的，因此**不能使用下标运算符 `[]` 访问其中的元素**。在链表的任一位置进行元素的插入、删除操作都是快速的。

(**带dummy节点的双向循环队列**)头节点的前驱元素指针域保存的是链表中尾元素的首地址，list的尾节点的后继元素指针域则保存了头节点的首地址，这样，list实际上就构成了一个双向循环链。

由于list元素节点并不要求在一段连续的内存中，显然在list中是不支持快速随机存取的，因此对于迭代器，只能通过“++”或“--”操作将迭代器移动到后继/前驱节点元素处。而不能对迭代器进行+n或-n的操作，这点，是与vector等不同的地方。

list 提供的接口（成员函数）
迭代器：`begin()`,` end()`, `cbegin()`, `cend()`,` rbegin()`,` rend()`, `crbegin()`, `crend()`
`size()`, `max_size()`, `empty()`
`pop_front()`, `pop_back()`, `front()`, `back()`
`reverse()` `merge()` 
`fill()`, `swap()`

通用函数：push_back(), pop_back(), empty(), clear(), swap(), insert(), erase(), reverse()

特有操作：push_front(), pop_front(), merge(), remove(), remove_if()

array 支持的操作（非成员函数）
`operator==`, `operator!=`, `operator<`, `operator<=`, `operator>`, `operator>=`

`swap()`
`get()`

## list() 构造函数

list() 声明一个空列表；

list(n) 声明一个有n个元素的列表，每个元素都是由其默认构造函数T()构造出来的

list(n,val) 声明一个由n个元素的列表，每个元素都是由其复制构造函数T(val)得来的

list(n,val) 声明一个和上面一样的列表

list(first,last) 声明一个列表，其元素的初始值来源于由区间所指定的序列中的元素

初始化列表

### list::push_front()

双向链表头插一个节点

### list::push_back()

双向链表尾插一个节点

序列必须不为空，如果当list为空的时候调用pop_back()和pop_front()会使程序崩掉。

### list::empty()

判断链表是否我空

### list::resize(n)

如果调用resize(n)将list的长度改为只容纳n个元素，超出的元素将被删除，如果需要扩展那么调用默认构造函数T()将元素加到list末端。如果调用resize(n,val)，则扩展元素要调用构造函数T(val)函数进行元素构造，其余部分相同。

### list::clear()

 清空list中的所有元素。

### list::front()

### list::back()

 通过front()可以获得list容器中的头部元素，通过back()可以获得list容器的最后一个元素。但是有一点要注意，就是list中元素是空的时候，这时候调用front()和back()会发生什么呢？实际上会发生不能正常读取数据的情况，但是这并不报错，那我们编程序时就要注意了，个人觉得在使用之前最好先调用empty()函数判断list是否为空。



### list::erase()

原型：`iterator erase ( iterator position );`

 `  iterator erase ( iterator first, iterator last );`

功能：清除链表中position 处或者[first,last)范围内的元素。会减少list的size值。

返回值：清除的最后一个元素的**下一个位置(迭代器)**

### list::assign()：

具体和vector中的操作类似，也是有两种情况，第一种是：l1.assign(n,val)将 l1中元素变为n个T(val）。第二种情况是：l1.assign(l2.begin(),l2.end())将l2中的从l2.begin()到l2.end()之间的数值赋值给l1。

### list::swap(list &l)

原型：`void swap ( list<T,Allocator>& lst)`

功能：将两个lsit交换`

### list::reverse()

通过reverse()完成list的逆置。

### list::remove()

原型：`void remove ( const T& value );`

功能：清除链表中特定的值value，lsit的size会相应减少。

返回值：无

### list::remove_if

原型：`template <class Predicate>`

`  void remove_if ( Predicate pred );`

功能：在满足Predicate pred返回true值时，移除元素。pred可以是一个返回bool类型的函数，还可以是一个重写operator函数的类。  例如：

```c++
// a predicate implemented as a function:
bool single_digit (const int& value) { 
    return (value<10); 
}
// a predicate implemented as a class:
class is_odd{
public:
    bool operator() (const int& value) {return (value%2)==1; }
};
```



### list::merge(list &l)

合并两个链表并使之默认升序(也可改)，l1.merge(l2，greater<int>()); 调用结束后l2变为空，l1中元素包含原来l1 和 l2中的元素，并且排好序，升序。其实默认是升序，greater<int>()可以省略，另外greater<int>()是可以变的，也可以不按升序排列。

### list.splice()

原型：设list2调用了splice函数

`void splice ( iterator position, list<T,Allocator>& x );`将list x中的所有元素插入到调用该函数的list2的position处。List x会被清空。

`void splice ( iterator position, list<T,Allocator>& x, iterator i )`;将x中指向i的位置处的元素插入到list2的position处。X会将i位置处的值删除。

`void splice ( iterator position, list<T,Allocator>& x, iterator first, iterator last );` 将x中[first,last)位置处的元素插入到list2的position处。

### list.sort()

原型：`void sort ( );`

`template <class Compare>`

 `void sort ( Compare comp );`

功能：排序

返回值：

### list::unique()

原型：`void unique ( );`

`template <class BinaryPredicate>`

` void unique ( BinaryPredicate binary_pred )`;按照规则binary_pred消除重复值。例如：

```C++
bool same_integral_part (double first, double second)
{ return ( int(first)==int(second) ); }
 
// a binary predicate implemented as a class:
class is_near
{
public:
  bool operator() (double first, double second)
  { return (fabs(first-second)<5.0); }
};
//调用：mylist.unique (same_integral_part); 
```

 mylist.unique (is_near());

功能：消除list中的重复元素

返回值：





## deque 双端队列：容器

deque（['dek]）（双端队列）是double-ended queue 的一个不规则缩写。deque是具有动态大小的序列容器，可以在两端（前端或后端）扩展或收缩。
特定的库可以以不同的方式实现deques，通常作为某种形式的动态数组。但是在任何情况下，它们都允许通过随机访问迭代器直接访问各个元素，通过根据需要扩展和收缩容器来自动处理存储。
因此，它们提供了类似于vector的功能，但是在序列的开始部分也可以高效地插入和删除元素，而不仅仅是在结尾。但是，与vector不同，deques并不保证将其所有元素存储在连续的存储位置：deque通过偏移指向另一个元素的指针访问元素会导致未定义的行为。
两个vector和deques提供了一个非常相似的接口，可以用于类似的目的，但内部工作方式完全不同：虽然vector使用单个数组需要偶尔重新分配以增长，但是deque的元素可以分散在不同的块的容器，容器在内部保存必要的信息以提供对其任何元素的持续时间和统一的顺序接口（通过迭代器）的直接访问。因此，deques在内部比vector更复杂一点，但是这使得他们在某些情况下更有效地增长，尤其是在重新分配变得更加昂贵的很长序列的情况下。
对于频繁插入或删除开始或结束位置以外的元素的操作，deques表现得更差，并且与列表和转发列表相比，迭代器和引用的一致性更低。
deque上常见操作的复杂性（效率）如下：
• 随机访问 - 常数O(1)
• 在结尾或开头插入或移除元素 - 摊销不变O(1)
• 插入或移除元素 - 线性O(n)

`template < class T, class Alloc = allocator<T> > class deque;`

### deque::deque()  

构造一个deque容器对象，根据所使用的构造函数版本初始化它的内容：

```c++
#include <deque>
#include <iostream>

int main()
{
    unsigned int i;
    // constructors used in the same order as described above:
    std::deque<int> first;
    // empty deque of ints
    std::deque<int> second(4, 100);
    // four ints with value 100
    std::deque<int> third(second.begin(), second.end());
    // iterating through second
    std::deque<int> fourth(third);
    // a copy of third // the iterator constructor can be used to copy arrays:
    int myints[] = {16, 2, 77, 29};
    std::deque<int> fifth(myints, myints + sizeof(myints) / sizeof(int));
    std::cout << "The contents of fifth are:";
    for (std::deque<int>::iterator it = fifth.begin(); it != fifth.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';
    return 0;
}
//ouput
// The contents of fifth are: 16 2 77 29
```

### deque::push_back(T value)

在当前的最后一个元素之后 ，在deque容器的末尾添加一个新元素。val的内容被复制（或移动）到新的元素。
这有效地增加了一个容器的大小。
`void push_back (const value_type& val);``

` void push_back (value_type&& val);`

### deque::push_front（T value）

在deque容器的开始位置插入一个新的元素，位于当前的第一个元素之前。val的内容被复制（或移动）到插入的元素。
这有效地增加了一个容器的大小。
void push_front (const value_type& val); void push_front (value_type&& val);

### deque::pop_back()

删除deque容器中的最后一个元素，有效地将容器大小减少一个。
这破坏了被删除的元素。
`void pop_back();`

### deque::pop_front()

删除deque容器中的第一个元素，有效地减小其大小。
这破坏了被删除的元素。
`void pop_front();`

### deque::emplace_front(args)

在deque的开头插入一个新的元素，就在其当前的第一个元素之前。这个新的元素是用args作为构建的参数来构建的。
这有效地增加了一个容器的大小。
该元素是通过调用allocator_traits::construct来转换args来创建的。
存在一个类似的成员函数push_front，它可以将现有对象复制或移动到容器中。

`template <class... Args> `

`void emplace_front (Args&&... args);`



### deque::emplace_back(args)

在deque的末尾插入一个新的元素，紧跟在当前的最后一个元素之后。这个新的元素是用args作为构建的参数来构建的。
这有效地增加了一个容器的大小。
该元素是通过调用allocator_traits::construct来转换args来创建的。
存在一个类似的成员函数push_back，它可以将现有对象复制或移动到容器中
`template <class... Args> `

`void emplace_back (Args&&... args);`

```c++
#include <deque>
#include <iostream>

int main()
{
    std::deque<int> mydeque = {10, 20, 30};
    mydeque.emplace_back(100);
    mydeque.emplace_back(200);
    std::cout << "mydeque contains:";
    //ouput
    // The contents of fifth are: 16 2 77 29
    for (auto& x: mydeque)
         std::cout << ' ' << x; std::cout << '\n'; 
    return 0; 
}
//mydeque contains: 10 20 30 100 200
```





## stack 适配器

根据不同的实现stack底层有多种实现方式，stl中默认的是deque

`std::stack<T,Container>::stack`

### Member types

| Member type       | Definition                   |
| ----------------- | ---------------------------- |
| `container_type`  | `Container`                  |
| `value_type`      | `Container::value_type`      |
| `size_type`       | `Container::size_type`       |
| `reference`       | `Container::reference`       |
| `const_reference` | `Container::const_reference` |

### stack::

`reference top();`
`const_reference top() const;top()`

返回栈顶元素的引用

### stack::empty()

`bool empty() const;`

checks whether the underlying container is empty
(public member function)

### stack::size()

返回栈中元素个数

`size_type size() const;`

### stack::push(value)

将元素压住栈顶

`void push( const value_type& value );`
`void push( value_type&& value );`

### list::emplace()

可以减少入栈过程中，构建对象过程

`template< class... Args >`
`void emplace( Args&&... args );`

### list::pop()

删除栈顶元素；当栈非空的时候；

`void pop();` 相当于容器中调用pop_back();

### list::swap( stack& other ) 

`void swap( stack& other ) noexcept(*/\* see below \*/*);`

### stack::size()

`size_type size() const`

返回栈中元素数量

### 非成员函数 operator

std::swap()







## queue (适配器)

队列： 先进先出；底层容器是deque

### Member types

| Member type       | Definition                   |
| ----------------- | ---------------------------- |
| `container_type`  | `Container`                  |
| `value_type`      | `Container::value_type`      |
| `size_type`       | `Container::size_type`       |
| `reference`       | `Container::reference`       |
| `const_reference` | `Container::const_reference` |

### queue ::front()

返回队首的引用

`reference front();
const_reference front() const;`

### queue ::back()

返回队尾的引用

`reference back();
const_reference back() const;`

### queue::empty()

判断队列是否为空

`bool empty() const;`

### queue ::size()

返回队列中元素个数

`size_type size() const`

### queue ::push()

入队进入队尾

`void push( const value_type& value );
void push( value_type&& value );`

### queue ::pop()

队首出队

`void pop();`

### queue ::swap()

交换队列

`void swap( queue& other ) noexcept(*/\* see below \*/*);`



## priority_queue 适配器

优先队列，底层默认是一个最大heap，可以更具不同的设定改变；

### Member types

| Member type             | Definition                   |
| ----------------------- | ---------------------------- |
| `container_type`        | `Container`                  |
| `value_compare` (C++17) | `Compare`                    |
| `value_type`            | `Container::value_type`      |
| `size_type`             | `Container::size_type`       |
| `reference`             | `Container::reference`       |
| `const_reference`       | `Container::const_reference` |

成员函数类似于queue

### 构造函数

priority_queue<<T,Container,Compare>

T 就是数据类型，

Container 就是容器类型（Container必须是用数组实现的容器，比如vector,deque等等，但不能用 list。STL里面默认用的是vector），

Compare 就是比较的方式，当需要用自定义的数据类型时才需要传入这三个参数，使用基本数据类型时，只需要传入数据类型，默认是大顶堆


```C++
//升序队列
priority_queue <int,vector<int>,greater<int> > q;
//降序队列
priority_queue <int,vector<int>,less<int> >q;

//greater和less是std实现的两个仿函数（就是使一个类的使用看上去像一个函数。
//其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了）

```

### 基础类型

对于基础类型默认构建最大堆

```c++
#include<iostream>
#include <queue>
using namespace std;
int main() 
{
    //对于基础类型 默认是大顶堆
    priority_queue<int> a; 
    //等同于 priority_queue<int, vector<int>, less<int> > a;
    
    //             这里一定要有空格，不然成了右移运算符↓
    priority_queue<int, vector<int>, greater<int> > c;  //这样就是小顶堆
    priority_queue<string> b;

    for (int i = 0; i < 5; i++) 
    {
        a.push(i);
        c.push(i);
    }
    while (!a.empty()) 
    {
        cout << a.top() << ' ';
        a.pop();
    } 
    cout << endl;

    while (!c.empty()) 
    {
        cout << c.top() << ' ';
        c.pop();
    }
    cout << endl;

    b.push("abc");
    b.push("abcd");
    b.push("cbd");
    while (!b.empty()) 
    {
        cout << b.top() << ' ';
        b.pop();
    } 
    cout << endl;
    return 0;
}
//4 3 2 1 0
//0 1 2 3 4
//cbd abcd abc
```

### pair类型

先比较第一个元素，第一个相等比较第二个

```c++
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
int main() 
{
    priority_queue<pair<int, int> > a;
    pair<int, int> b(1, 2);
    pair<int, int> c(1, 3);
    pair<int, int> d(2, 5);
    a.push(d);
    a.push(c);
    a.push(b);
    while (!a.empty()) 
    {
        cout << a.top().first << ' ' << a.top().second << '\n';
        a.pop();
    }
}
//2 5
//1 3
//1 2
```

### 自定义类型：

```c++
#include <iostream>
#include <queue>
using namespace std;

//方法1
struct tmp1 //运算符重载<
{
    int x;
    tmp1(int a) {x = a;}
    bool operator<(const tmp1& a) const
    {
        return x < a.x; //大顶堆
    }
};

//方法2
struct tmp2 //重写仿函数
{
    bool operator() (tmp1 a, tmp1 b) 
    {
        return a.x < b.x; //大顶堆
    }
};

int main() 
{
    tmp1 a(1);
    tmp1 b(2);
    tmp1 c(3);
    priority_queue<tmp1> d;
    d.push(b);
    d.push(c);
    d.push(a);
    while (!d.empty()) 
    {
        cout << d.top().x << '\n';
        d.pop();
    }
    cout << endl;

    priority_queue<tmp1, vector<tmp1>, tmp2> f;
    f.push(c);
    f.push(b);
    f.push(a);
    while (!f.empty()) 
    {
        cout << f.top().x << '\n';
        f.pop();
    }
}
//3
//2
//1/
//
//3
//2
//1
```



## map  (有序的map) 约等于TreeMap

其中map和 mutlimap在 map头文件中。



map 是关联容器，按照特定顺序存储由 key value (键值) 和 mapped value (映射值) 组合形成的元素。
在映射中，键值通常用于对元素进行排序和唯一标识，而映射的值存储与此键关联的内容。该类型的键和映射的值可能不同，并且在部件类型被分组在一起VALUE_TYPE，这是一种对类型结合两种：
`typedef pair<const Key, T> value_type;`
在内部，映射中的元素总是按照由其内部比较对象（比较类型）指示的特定的严格弱排序标准按键排序。映射容器通常比unordered_map容器慢，以通过它们的键来访问各个元素，但是它们允许基于它们的顺序对子集进行直接迭代。 在该映射值地图可以直接通过使用其相应的键来访问括号运算符（（操作符[] ）。 映射通常如实施

```
template < class Key,						 // map::key_type
    class T,								 // map::mapped_type 
    class Compare = less<Key>,  			// map::key_compare 
    class Alloc = allocator<pair<const Key,T>   // map::allocator_type > 
> class map;
```

### map::map()

构造一个映射容器对象，根据所使用的构造器版本初始化其内容：
（1）空容器构造函数（默认构造函数）
构造一个空的容器，没有元素。
（2）范围构造函数 构造具有一样多的元素的范围内的容器[第一，最后一个），其中每个元件布设构造的从在该范围内它的相应的元件。
（3）复制构造函数（并用分配器复制）
使用x中的每个元素的副本构造一个容器。
（4）移动构造函数（并与分配器一起移动）
构造一个获取x元素的容器。 如果指定了alloc并且与x的分配器不同，那么元素将被移动。否则，没有构建元素（他们的所有权直接转移）。 x保持未指定但有效的状态。
（5）初始化列表构造函数
用il中的每个元素的副本构造一个容器。

```c++
empty (1) explicit map (const key_compare& comp = key_compare(),
                        const allocator_type& alloc = allocator_type());
explicit map (const allocator_type& alloc); 

range (2) 
template <class InputIterator> map (InputIterator first, InputIterator last, const key_compare& comp = key_compare(), const allocator_type& = allocator_type());

copy (3)
map (const map& x); 
map (const map& x, const allocator_type& alloc); 

move (4) 
map (map&& x); map (map&& x, const allocator_type& alloc);

initializer list (5)
map (initializer_list<value_type> il, const key_compare& comp = key_compare(), const allocator_type& alloc = allocator_type());
```

示例：

```c++
#include <iostream>
#include <map>
bool fncomp(char lhs, char rhs) { return lhs < rhs; }
struct classcomp
{
    bool operator()(const char &lhs, const char &rhs) const { return lhs < rhs; }
};
int main()
{
    std::map<char, int> first;
    first['a'] = 10;
    first['b'] = 30;
    first['c'] = 50;
    first['d'] = 70;
    std::map<char, int> second(first.begin(), first.end());
    std::map<char, int> third(second);
    std::map<char, int, classcomp> fourth;
    bool (*fn_pt)(char, char) = fncomp;
    std::map<char, int, bool (*)(char, char)> fifth(fn_pt);
    // function pointer as Compare
    
    map<string, string> authors { {"Joyce", "James"}, {"Austen", "Jane"}, {"Dickens", "Charles"} };
    return 0;
}
```

### map::begin（）

返回引用map容器中第一个元素的迭代器。
由于map容器始终保持其元素的顺序，所以开始指向遵循容器排序标准的元素。
如果容器是空的，则返回的迭代器值不应被解除引用。

`iterator begin() noexcept; `

`const_iterator begin() const noexcept;`

### map::key_comp()

返回容器用于比较键的比较对象的副本。

`key_compare key_comp() const;`

```c++
#include <map>
#include <iostream>
int main()
{
    std::map<char, int> mymap;
    std::map<char, int>::key_compare mycomp = mymap.key_comp();
    mymap['a'] = 100;
    mymap['b'] = 200;
    mymap['c'] = 300;
    std::cout << "mymap contains:\n";
    char highest = mymap.rbegin()->first; // key value of last element
    std::map<char, int>::iterator it = mymap.begin();
    do
    {
        std::cout << it->first << " => " << it->second << '\n';
    } while (mycomp((*it++).first, highest));
    std::cout << '\n';
    return 0;
}
//output
// mymap contains:
// a => 100
// b => 200
// c => 300
```

### map::value_comp()

返回可用于比较两个元素的比较对象，以获取第一个元素的键是否在第二个元素之前。
`value_compare value_comp() const;`

```c++
#include <iostream>
#include <map>
int main()
{
    std::map<char, int> mymap;
    mymap['x'] = 1001;
    mymap['y'] = 2002;
    mymap['z'] = 3003;
    std::cout << "mymap contains:\n";
    std::pair<char, int> highest = *mymap.rbegin(); // last element
    std::map<char, int>::iterator it = mymap.begin();
    do
    {
        std::cout << it->first << " => " << it->second << '\n';
    } while (mymap.value_comp()(*it++, highest));
    return 0;
}
```

### map::find(key)

在容器中搜索具有等于k的键的元素，如果找到则返回一个迭代器，否则返回map::end的迭代器。
如果容器的比较对象自反地返回假（即，不管元素作为参数传递的顺序），则两个key被认为是等同的。
另一个成员函数map::count可以用来检查一个特定的键是否存在。

`iterator find (const key_type& k); `

`const_iterator find (const key_type& k) const;`

### map::count(key)

在容器中搜索具有等于k的键的元素，并返回匹配的数量。
由于地图容器中的所有元素都是唯一的，因此该函数只能返回1（如果找到该元素）或返回零（否则）。
如果容器的比较对象自反地返回错误（即，不管按键作为参数传递的顺序），则两个键被认为是等同的。
`size_type count (const key_type& k) const;`

### map::lower_bound()

将迭代器返回到下限
返回指向容器中第一个元素的迭代器，该元素的键不会在k之前出现（即，它是等价的或者在其后）。
该函数使用其内部比较对象（key_comp）来确定这一点，将迭代器返回到key_comp（element_key，k）将返回false的第一个元素。
如果map类用默认的比较类型（less）实例化，则函数返回一个迭代器到第一个元素，其键不小于k。
一个类似的成员函数upper_bound具有相同的行为lower_bound，除非映射包含一个key值等于k的元素：在这种情况下，lower_bound返回指向该元素的迭代器，而upper_bound返回指向下一个元素的迭代器。
`iterator lower_bound (const key_type& k);`

` const_iterator lower_bound (const key_type& k) const;`





### map::upper_bound()

将迭代器返回到上限
返回一个指向容器中第一个元素的迭代器，它的关键字被认为是在k之后。

该函数使用其内部比较对象（key_comp）来确定这一点，将迭代器返回到key_comp（k，element_key）将返回true的第一个元素。
如果map类用默认的比较类型（less）实例化，则函数返回一个迭代器到第一个元素，其键大于k。
类似的成员函数lower_bound具有与upper_bound相同的行为，除了map包含一个元素，其键值等于k：在这种情况下，lower_bound返回指向该元素的迭代器，而upper_bound返回指向下一个元素的迭代器。
`iterator upper_bound (const key_type& k); `

`const_iterator upper_bound (const key_type& k) const;`

```c++
#include <iostream>
#include <map>
int main()
{
    std::map<char, int> mymap;
    std::map<char, int>::iterator itlow, itup;
    mymap['a'] = 20;
    mymap['b'] = 40;
    mymap['c'] = 60;
    mymap['d'] = 80;
    mymap['e'] = 100;
    itlow = mymap.lower_bound('b');
    // itlow points to b
    itup = mymap.upper_bound('d');

    for (std::map<char, int>::iterator it = itlow; it != itup; ++it)
        std::cout << it->first << " => " << it->second << '\n';
    // itup points to e (not d!)
    std::cout<<"-------------\n";
    mymap.erase(itlow, itup);
    // erases [itlow,itup)
    // print content:
    for (std::map<char, int>::iterator it = mymap.begin(); it != mymap.end(); ++it)
        std::cout << it->first << " => " << it->second << '\n';
    return 0;
}
//output
// b => 40
// c => 60
// d => 80
// -------------
// a => 20
// e => 100
```

### map::equal_range()

获取相同元素的范围
返回包含容器中所有具有与k等价的键的元素的范围边界。 由于地图容器中的元素具有唯一键，所以返回的范围最多只包含一个元素。
如果没有找到匹配，则返回的范围具有零的长度，与两个迭代器指向具有考虑去后一个密钥对所述第一元件ķ根据容器的内部比较对象（key_comp）。如果容器的比较对象返回false，则两个键被认为是等价的。

`pair<const_iterator,const_iterator> equal_range (const key_type& k) const; pair<iterator,iterator> equal_range (const key_type& k);`

```c++
#include <iostream>
#include <map>
int main()
{
    std::map<char, int> mymap;
    mymap['a'] = 10;
    mymap['b'] = 20;
    mymap['c'] = 30;
    std::pair<std::map<char, int>::iterator, std::map<char, int>::iterator> ret;
    ret = mymap.equal_range('b');
    std::cout << "lower bound points to: ";
    std::cout << ret.first->first << " => " << ret.first->second << '\n';
    std::cout << "upper bound points to: ";
    std::cout << ret.second->first << " => " << ret.second->second << '\n';
    return 0;
}
// lower bound points to: b => 20
// upper bound points to: c => 30
```

### operator[]

`T& operator[]( const Key& key );`  存在则返回value， 不存在则创建，创建过调用默认构造函数；返回默认构建的vlaue;
`T& operator[]( Key&& key );`

```c++
#include <iostream>
#include <string>
#include <vector>
#include <map>
 
int main()
{
    std::map<char, int> letter_counts {{'a', 27}, {'b', 3}, {'c', 1}};
 
    std::cout << "initially:\n";
    for (const auto &pair : letter_counts) {
        std::cout << pair.first << ": " << pair.second << '\n';
    }
 
    letter_counts['b'] = 42;  // update an existing value
    letter_counts['x'] = 9;  // insert a new value
 
    std::cout << "after modifications:\n";
    for (const auto &pair : letter_counts) {
        std::cout << pair.first << ": " << pair.second << '\n';
    }
 
    // count the number of occurrences of each word
    // (the first call to operator[] initialized the counter with zero)
    std::map<std::string, size_t>  word_map;
    for (const auto &w : { "this", "sentence", "is", "not", "a", "sentence",
                           "this", "sentence", "is", "a", "hoax"}) {
        ++word_map[w];
    }
 
    for (const auto &pair : word_map) {
        std::cout << pair.second << " occurrences of word '" << pair.first << "'\n";
    }
}#include <iostream>
#include <string>
#include <vector>
#include <map>
 
int main()
{
    std::map<char, int> letter_counts {{'a', 27}, {'b', 3}, {'c', 1}};
 
    std::cout << "initially:\n";
    for (const auto &pair : letter_counts) {
        std::cout << pair.first << ": " << pair.second << '\n';
    }
 
    letter_counts['b'] = 42;  // update an existing value
    letter_counts['x'] = 9;  // insert a new value
 
    std::cout << "after modifications:\n";
    for (const auto &pair : letter_counts) {
        std::cout << pair.first << ": " << pair.second << '\n';
    }
 
    // count the number of occurrences of each word
    // (the first call to operator[] initialized the counter with zero)
    std::map<std::string, size_t>  word_map;
    for (const auto &w : { "this", "sentence", "is", "not", "a", "sentence",
                           "this", "sentence", "is", "a", "hoax"}) {
        ++word_map[w];
    }
 
    for (const auto &pair : word_map) {
        std::cout << pair.second << " occurrences of word '" << pair.first << "'\n";
    }
}
```



无序容器（Unorder Container） : unordered_set 、 unordered_multiset、unordered_map、

unordered_multimap

包括：
• unordered_set
• unordered_multiset
• unordered_map
• unordered_multimap
都是以哈希表实现的

### 插入数据

注意：关联容器 插入不存在 再当前指针位置插入操作；

**1用insert函数插入pair数据**

```c++
map<string, int> mapStudent;//创建map
mapStudent.insert(pair<string, int>("student_one", 22));
mapStudent.insert(pair<string, int>("student_two", 25));
mapStudent.insert(pair<string, int>("student_three", 21));
 
或者使用make_pair
map<string, int> mapStudent;
mapStudent.insert(make_pair("student_one", 22));
mapStudent.insert(make_pair("student_two", 25));
mapStudent.insert(make_pair("student_three", 21));
```

**第二种：用insert函数插入value_type数据**

```c++
map<string, int> mapStudent;//创建map
mapStudent.insert(map<string, int>::value_type("student_one", 22));
mapStudent.insert(map<string, int>::value_type("student_two", 25));
mapStudent.insert(map<string, int>::value_type("student_three", 21));
```

**第三种：用数组方式插入数据**

```c++
map<string, int> mapStudent;//创建map
mapStudent["student_one"] = 22;
mapStudent["student_two"] = 25;
mapStudent["student_three"] = 21;
```

**第四种：用emplace函数插入数据**

```c++
map<string, int> mapStudent;
mapStudent.emplace("student_one", 22);
mapStudent.emplace("student_two", 25);
mapStudent.emplace("student_three", 21);
```

### **遍历**

```
for (auto x : mapStudent){
	cout << x.first << " " << x.second << endl;
}
```

### 元素删除erase函数

`size_type erase( const key_type& key );`

`void erase( iterator pos );`

### 使用map和multimap需要注意以下几点：

1.map和multimap将key/value（键值/实值 对组）当做元素，进行管理。它们可根据key的排序准则自动将元素元素排序。multimap允许重复元素，map不允许。

2.map的第一个参数被当做元素的key，第二个参数被当做元素的value，第三个参数用它来定义排序准则。元素的次序由它们的key决定，和value无关。

3.map和multimap根据元素的key自动对元素进行排序，这么一来，根据已知的key搜寻某个元素时，就能够有很好的性能，而根据已知value搜寻元素时，性能就很糟糕。

4.“自动排序”这一性质使得map和multimap身上有了一条重要的限制：不可以直接改变元素的key，因为这会破坏正确次序。要修改元素的key，必须先移除拥有该key的元素，
然后插入拥有新的key/value的元素。从迭代器的角度来看，元素的key是常数，至于元素的value倒是可以直接修改。

5.map和multimap不支持元素直接存取，因此元素的存取通常是经由迭代器进行。不过map有个例外，map提供下标操作符，可直接存取元素。

6.在map和multimap中，key被视为常数，因此不能调用任何变动性算法，例如你不能对它们调用remove()，因为remove()算法实际上是以一个参数值覆盖被移除的元素。如果要移除map和multimap的元素，你只能使用它们所提供的成员函数。

## unordered_map  约等于 HashMap

实现方式占时没看，估计和java一样是数组+链表；

多出来的很多参数，包括 哈希桶个数 最大额哈希桶个数，加载因子，最大加载因子，扩容的时候是否重新哈希映射；



multi与否只是与operator[]能够使用有关；

## tuple

元组是一个能够容纳元素集合的对象。每个元素可以是不同的类型。
`template <class... Types> class tuple;`

```c++
#include <iostream>
// std::cout
#include <tuple>
// std::tuple, std::get, std::tie, std::ignore
int main()
{
    std::tuple<int, char> foo(10, 'x');
    auto bar = std::make_tuple("test", 3.1, 14, 'y');
    std::get<2>(bar) = 100; //bar("test", 3.1, 100, 'y')
    int myint;
    char mychar;
    std::tie(myint, mychar) = foo; //myint = 10,mychar = 'x'
    // unpack elements
    std::tie(std::ignore, std::ignore, myint, mychar) = bar; //myint = 100,mychar = 'y'
    // unpack (with ignore)
    mychar = std::get<3>(bar); //mychar ='y'
    std::get<0>(foo) = std::get<2>(bar); //foo(100,'x')
    std::get<1>(foo) = mychar;//foo(100,'y')
    std::cout << "foo contains: ";
    std::cout << std::get<0>(foo) << ' ';
    std::cout << std::get<1>(foo) << '\n';
    return 0;
}
//输出
// foo contains: 100 y
```

### tuple::tuple()

构建一个 tuple（元组）对象。
这涉及单独构建其元素，初始化取决于调用的构造函数形式：
（1）默认的构造函数
构建一个 元组对象的元素值初始化。
（2）复制/移动构造函数
该对象使用tpl的内容进行初始化 元组目的。tpl 的相应元素被传递给每个元素的构造函数。
（3）隐式转换构造函数
同上。tpl中的 所有类型都可以隐含地转换为构造中它们各自元素的类型元组 目的。
（4）初始化构造函数 用elems中的相应元素初始化每个元素。elems 的相应元素被传递给每个元素的构造函数。
（5）对转换构造函数
该对象有两个对应于pr.first和的元素pr.second。PR中的所有类型都应该隐含地转换为其中各自元素的类型元组 目的。
（6）分配器版本
和上面的版本一样，除了每个元素都是使用allocator alloc构造的

```c++
default (1)
constexpr tuple(); 

copy / move (2) 
tuple (const tuple& tpl) = default;
tuple (tuple&& tpl) = default;

implicit conversion (3) 
template <class... UTypes> tuple (const tuple<UTypes...>& tpl); 
template <class... UTypes> tuple (tuple<UTypes...>&& tpl);

initialization (4) 
explicit tuple (const Types&... elems);
template <class... UTypes> explicit tuple (UTypes&&... elems); conversion from pair
  
(5) template <class U1, class U2> 
 tuple (const pair<U1,U2>& pr);

template <class U1, class U2> 
tuple (pair<U1,U2>&& pr);

allocator (6)
template<class Alloc> 
 tuple (allocator_arg_t aa, const Alloc& alloc);

template<class Alloc>
tuple (allocator_arg_t aa, const Alloc& alloc, const tuple& tpl); 

template<class Alloc>
tuple (allocator_arg_t aa, const Alloc& alloc, tuple&& tpl);

template<class Alloc,class... UTypes> 
tuple (allocator_arg_t aa, const Alloc& alloc, const tuple<UTypes...>& tpl); 

template<class Alloc, class... UTypes> 
tuple (allocator_arg_t aa, const Alloc& alloc, tuple<UTypes...>&& tpl);

template<class Alloc>
tuple (allocator_arg_t aa, const Alloc& alloc, const Types&... elems); 

template<class Alloc, class... UTypes> 
tuple (allocator_arg_t aa, const Alloc& alloc, UTypes&&... elems); 

template<class Alloc, class U1, class U2> 
tuple (allocator_arg_t aa, const Alloc& alloc, const pair<U1,U2>& pr); 

template<class Alloc, class U1, class U2> 
tuple (allocator_arg_t aa, const Alloc& alloc, pair<U1,U2>&& pr);
```

示例

````
#include <iostream>
// std::cout
#include <utility>
// std::make_pair
#include <tuple> // std::tuple, std::make_tuple, std::get

int main()
{
    std::tuple<int, char> first;
    // default
    std::tuple<int, char> second(first);
    // copy
    std::tuple<int, char> third(std::make_tuple(20, 'b'));
    // move
    std::tuple<long, char> fourth(third);
    // implicit conversion
    std::tuple<int, char> fifth(10, 'a');
    // initialization
    std::tuple<int, char> sixth(std::make_pair(30, 'c'));
    // from pair / move
    std::cout << "sixth contains: " << std::get<0>(sixth);
    std::cout << " and " << std::get<1>(sixth) << '\n';
    return 0;
}
//sixth contains: 30 and c
````

## pair

这个类把一对值（values）结合在一起，这些值可能是不同的类型（T1 和 T2）。每个值可以被公有的成员变量first、second访问。
pair是tuple（元组）的一个特例。
pair的实现是一个结构体，主要的两个成员变量是first second 因为是使用struct不是class，所以可以直接使用pair的成员变量。
应用：
• 可以将两个类型数据组合成一个如map<key, value>
• 当某个函数需要两个返回值时
`template <class T1, class T2> struct pair;`

构建一个pair对象。

这涉及到单独构建它的两个组件对象，初始化依赖于调用的构造器形式：
（1）默认的构造函数
构建一个 对对象的元素值初始化。
（2）复制/移动构造函数（和隐式转换）
该对象被初始化为pr的内容 对目的。pr 的相应成员被传递给每个成员的构造函数。
（3）初始化构造函数
会员 第一是由一个和成员构建的第二与b。
（4）分段构造
构造成员 first 和 second 到位，传递元素first_args 作为参数的构造函数 first，和元素 second_args 到的构造函数 second.

```c++
default (1)
constexpr pair();

copy / move (2)
template<class U, class V> 
pair (const pair<U,V>& pr); 
template<class U, class V> 
pair (pair<U,V>&& pr);
pair (const pair& pr) = default;
pair (pair&& pr) = default;

initialization (3)
pair (const first_type& a, const second_type& b);
template<class U, class V> pair (U&& a, V&& b);

piecewise (4)
template <class... Args1, class... Args2> 
pair (piecewise_construct_t pwc, tuple<Args1...> first_args, tuple<Args2...> second_args);
```

```c++
#include <iostream>

int main()
{
    std::pair<std::string, double> product1;
    // default constructor
    std::pair<std::string, double> product2("tomatoes", 2.30);
    // value init
    std::pair<std::string, double> product3(product2);
    // copy constructor
    product1 = std::make_pair(std::string("lightbulbs"), 0.99);
    // using make_pair (move) product2.first = "shoes";
    // the type of first is string product2.second = 39.90;
    // the type of second is double 
    std::cout << "The price of " << product1.first << " is $" << product1.second << '\n';
    std::cout << "The price of " << product2.first << " is $" << product2.second << '\n';
    std::cout << "The price of " << product3.first << " is $" << product3.second << '\n';
    return 0;
}
// The price of lightbulbs is $0.99
// The price of tomatoes is $2.3
// The price of tomatoes is $2.3
```

## queue (适配器)

- 用于存储固定数量元素序列的标准容器

- 以内置数组为基础实现

- 支持随机访问迭代器

- array 提供的接口（成员函数）
  迭代器：`begin()`,` end()`, `cbegin()`, `cend()`,` rbegin()`,` rend()`, `crbegin()`, `crend()`
  `size()`, `max_size()`, `empty()`
  `operator[]`, `at()`, `front()`, `back()`
  `data()`
  `fill()`, `swap()`
  array 支持的操作（非成员函数）
  `operator==`, `operator!=`, `operator<`, `operator<=`, `operator>`, `operator>=`

  `swap()`
  `get()`