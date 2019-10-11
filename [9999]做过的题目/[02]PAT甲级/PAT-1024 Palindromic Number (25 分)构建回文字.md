# 1024 Palindromic Number (25 分)

## 题目描述

A number that will be the same when it is written forwards or backwards is known as a **Palindromic Number**. For example, 1234321 is a palindromic number. All single digit numbers are palindromic numbers.

Non-palindromic numbers can be paired with palindromic ones via a series of operations. First, the non-palindromic number is reversed and the result is added to the original number. If the result is not a palindromic number, this is repeated until it gives a palindromic number. For example, if we start from 67, we can obtain a palindromic number in 2 steps: 67 + 76 = 143, and 143 + 341 = 484.

Given any positive integer *N*, you are supposed to find its paired palindromic number and the number of steps taken to find it.

### Input Specification:

Each input file contains one test case. Each case consists of two positive numbers *N* and *K*, where *N* (≤1010) is the initial numer and *K* (≤100) is the maximum number of steps. The numbers are separated by a space.

### Output Specification:

For each test case, output two numbers, one in each line. The first number is the paired palindromic number of *N*, and the second number is the number of steps taken to find the palindromic number. If the palindromic number is not found after *K* steps, just output the number obtained at the *K*th step and *K* instead.

### Sample Input 1:

```in
67 3
```

### Sample Output 1:

```out
484
2
```

### Sample Input 2:

```in
69 3
```

### Sample Output 2:

```out
1353
3
```



## 代码实现



```C++
#include <cstdio>
#include <string>
#include <iostream>
#include <algorithm>

using namespace std;
bool isPalindromic(string &s){
    int len=s.size();
    bool flag=false;
    if(len%2==0){
        for(int x=(len-1)/2,y=x+1; x>=0&&y<len; x--,y++){
            if(s[x]!=s[y])
                return false;
        }
    } else {
        for(int x=(len-1)/2 -1,y=x+2; x>=0&&y<len; x--,y++){
            if(s[x]!=s[y])
                return false;
        }
    }
    return true;
}

int main(){
    string s;
    int k;
    cin>>s>>k;

    int i=0;
    while(i<k&&!isPalindromic(s)){
        string t=s;
        reverse(t.begin(),t.end());
        int flag =0;
        for(int j=s.size()-1; j >= 0; j--){
            int temp=(s[j]-'0')+(t[j]-'0')+flag;
            flag=0;
            if(temp>=10){
                flag=1;
                temp-=10;
            }
            s[j]=temp+'0';
        }
        if(flag==1)
            s='1'+s;
        i++;
    }
    cout<<s<<"\n"<<i;
    return 0;

}

/*
    if(isPalindromic(s)){
        cout<<s<<"\n"<<i;
        return 0;
    }

    for(int i=1; i<=k; i++){
        string t=s;
        reverse(t.begin(),t.end());
        int flag =0;
        for(int j=s.size()-1; j >= 0; j--){
            int temp=(s[j]-'0')+(t[j]-'0')+flag;
            flag=0;
            if(temp>=10){
                flag=1;
                temp-=10;
            }
            s[j]=temp+'0';
        }
        if(flag==1)
            s='1'+s;
        //cout<<"s:"<<s<<endl;
    }

    cout<<s<<"\n"<<k;
    return 0;
*/

```

