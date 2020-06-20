## array

### `array` 的基础实现

```c++
// 数组 T[N]
template <typename T, size_t N>
struct __array_traits
{
  typedef T Type[N];

  static constexpr T& S_ref(const Type& t, size_t n) { return const_cast<T&>(t[n]); }
  static constexpr T* S_ptr(const Type& t) { return const_cast<T*>(t); }
};

// 类模板部分特例化，处理空数组
template <typename T>
struct __array_traits<T, 0>
{
  struct Type { };

  static constexpr T& S_ref(const Type&, size_t) { return *static_cast<T*>(nullptr); }
  static constexpr T* S_ptr(const Type&) { return nullptr; }
};
```

## `array` 的内部结构

```c++
template <typename T, size_t N>
struct array {
  // 类型成员
  using value_type      = T;
  using pointer         = T * ;
  using const_pointer   = const T*;
  using reference       = T & ;
  using const_reference = const T&;
  using iterator        = T * ;         // array 的迭代器即普通指针
  using const_iterator  = const T*;

  using size_type              = std::size_t;
  using difference_type        = std::ptrdiff_t;
  using reverse_iterator       = std::reverse_iterator<iterator>;
  using const_reverse_iterator = std::reverse_iterator<const_iterator>;

  typedef  __array_traits<T, N> AT_Type;
  typename AT_Type::Type        M_elems;   // 数组 T[N] 或空数组

  // ===================== 迭代器 =====================

  constexpr iterator begin() { return iterator(data()); }
  constexpr iterator end() { return iterator(data() + N); }
  constexpr const_iterator begin() const { return const_iterator(data()); }
  constexpr const_iterator end() const { return const_iterator(data() + N); }
  // 省略 cbegin(), cend(), rbegin(), rend(), crbegin(), crend()

  // ===================== 对外接口 =====================

  void fill(const value_type& val) { std::fill_n(begin(), end(), val); }
  void swap(array& other) { std::swap_ranges(begin(), end(), other.begin()); }

  constexpr size_type size() const { return N; }
  constexpr size_type max_size() const { return N; }
  constexpr bool empty() const { return size() == 0; }

  constexpr reference operator[](size_type n) { return AT_Type::S_ref(M_elems, n); }
  constexpr const_reference operator[](size_type n) const { return AT_Type::S_ref(M_elems, n); }

  constexpr reference at(size_type n)
  { return n < N ? AT_Type::S_ref(M_elems, n) : /* out_of_range */; }

  constexpr const_reference at(size_type n) const
  { return n < N ? AT_Type::S_ref(M_elems, n) : /* out_of_range */ ; }

  constexpr reference front() { return *begin(); }
  constexpr reference back() { return N ? *(end() - 1) : *end(); }
  constexpr const_reference front() const { return AT_Type::S_ref(M_elems, 0); }
  constexpr const_reference back() const { return N ? AT_Type::S_ref(M_elems, N - 1) : AT_Type::S_ref(M_elems, 0); }

  constexpr pointer data() { return AT_Type::S_ptr(M_elems); }
  constexpr const_pointer data() const { return AT_Type::_S_ptr(M_elems); }
};
```

## `array` 支持的操作（非成员函数）

```c++
// ===================== 比较操作 =====================
template <typename T, size_t N>
inline bool operator==(const array<T, N>& lhs, const array<T, N>& rhs)
{ return std::equal(lhs.begin(), lhs.end(), rhs.begin()); }

template <typename T, size_t N>
inline bool operator!=(const array<T, N>& lhs, const array<T, N>& rhs)
{ return !(lhs == rhs); }

template <typename T, size_t N>
inline bool operator<(const array<T, N>& lhs, const array<T, N>& rhs)
{ return std::lexicographical_compare(lhs.begin(), lhs.end(), rhs.begin(), rhs.end()); }

template <typename T, size_t N>
inline bool operator>(const array<T, N>& lhs, const array<T, N>& rhs)
{ return rhs < lhs; }

template <typename T, size_t N>
inline bool operator<=(const array<T, N>& lhs, const array<T, N>& rhs)
{ return !(lhs > rhs); }

template <typename T, size_t N>
inline bool operator>=(const array<T, N>& lhs, const array<T, N>& rhs)
{ return !(lhs < rhs); }

// ===================== 其他操作 =====================
template <typename T, size_t N>
inline void swap(array<T, N>& lhs, array<T, N>& rhs)
{ lhs.swap(rhs); }

template <typename T, size_t N, size_t Index>
constexpr T& get(array<T, N>& arr)
{
  static_assert(Index < N); 
  return typename __array_traits<T, N>::S_ref(arr.M_elems, Index); 
}

template <typename T, size_t N, size_t Index>
constexpr const T& get(const array<T, N>& arr)
{ 
  static_assert(Index < N);
  return typename __array_traits<T, N>::S_ref(arr.M_elems, Index); 
}

template <typename T, size_t N, size_t Index>
constexpr T&& get(array<T, N>&& arr)
{ 
  static_assert(Index < N);
  return std::move(get<Index>(arr)); 
}
```



注意：deallocate和destory的关系：

## deallocate实现的源码：

```c++
template <class T>
 inline void _deallocate(T* buffer) { 
    ::operator delete(buffer); 
    //为什么不用 delete [] ? ,operator delete 区别于 delete 
    //operator delete 是一个底层操作符
}
```

destory：

```c++
template <class T>
inline void _destory(T *ptr) {
    ptr->~T(); 
}
destory负责调用类型的析构函数，销毁相应内存上的内容（但销毁后内存地址仍保留）
deallocate负责释放内存（此时相应内存中的值在此之前应调用destory销毁，将内存地址返回给系统，代表这部分地址使用引用-1）
```