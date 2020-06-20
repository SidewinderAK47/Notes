变量极限：头文件

```
#include <climits>
```



```C++
#define CHAR_BIT /* 见定义 */
#define SCHAR_MIN /* 见定义 */
#define SCHAR_MAX /* 见定义 */
#define UCHAR_MAX /* 见定义 */
#define CHAR_MIN /* 见定义 */
#define CHAR_MAX /* 见定义 */
#define MB_LEN_MAX /* 见定义 */
#define SHRT_MIN /* 见定义 */
#define SHRT_MAX /* 见定义 */
#define USHRT_MAX /* 见定义 */
#define INT_MIN /* 见定义 */
#define INT_MAX /* 见定义 */
#define UINT_MAX /* 见定义 */
#define LONG_MIN /* 见定义 */      
#define LONG_MAX /* 见定义 */
#define ULONG_MAX /* 见定义 */
#define LLONG_MIN /* 见定义 C++11*/
#define LLONG_MAX /* 见定义 C++11*/
#define ULLONG_MAX /* 见定义 C++11 */
```

```
//宏常量
CHAR_BIT              字节的位数 (宏常量) 
MB_LEN_MAX         	  多字节字符的最大字节数 (宏常量)
CHAR_MIN              char 的最小值 (宏常量)
CHAR_MAX              char 的最大值 (宏常量)   
```

| 变量类型 |   最大值   |   最小值    | 位数 |
| :------: | :--------: | :---------: | :--: |
|   int    | 2147483647 | -2147483648 |  32  |
|   long   | 2147483647 |             |      |
|          |            |             |      |
|          |            |             |      |



```c++
//最小值 (宏常量)
SCHAR_MIN	          signed char
SHRT_MIN		      short 的最小值 (宏常量
INT_MIN			      int的最小值 (宏常量
LONG_MIN		     long 的最小值 (宏常量
LLONG_MIN(C++11)      long long 的最小值 (宏常量
```

```c++
//最大值 (宏常量)
SCHAR_MAX		      signed char
SHRT_MAX			 short
INT_MAX				 int
LONG_MAX   			 long
LLONG_MAX(C++11)      long long
```



```C++
//最大值 (宏常量)
UCHAR_MAX        unsigned char
USHRT_MAX        unsigned short
UINT_MAX		unsigned int
ULONG_MAX		unsigned long
ULLONG_MAX(C++11)  unsigned long long
```

