# 1023 Have Fun with Numbers (20 分)

## 题目描述：

Notice that the number 123456789 is a 9-digit number consisting exactly the numbers from 1 to 9, with no duplication. Double it we will obtain 246913578, which happens to be another 9-digit number consisting exactly the numbers from 1 to 9, only in a different permutation. Check to see the result if we double it again!

Now you are suppose to check if there are more numbers with this property. That is, double a given number with *k* digits, you are to tell if the resulting number consists of only a permutation of the digits in the original number.

### Input Specification:

Each input contains one test case. Each case contains one positive integer with no more than 20 digits.

### Output Specification:

For each test case, first print in a line "Yes" if doubling the input number gives a number that consists of only a permutation of the digits in the original number, or "No" if not. Then in the next line, print the doubled number.

### Sample Input:

```in
1234567899
```

### Sample Output:

```out
Yes
2469135798
```

## 代码实现:

里面包含两个实现，一个是我的实现。

一个是柳神的实现。

```C++
#include <cstdio>
#include <iostream>
#include <cstring>

using namespace std;

int testMain(){
    string s1,s2;
    int up=0,cur;
    bool flag,exist[24]={false};
    int book[10]={0};
    cin>>s1;
    s2=s1;

    for(int i=s1.size()-1; i>=0; i--){
        int temp=s1[i]-'0';
        //book[temp]++;
        cur=temp*2+up;
        //temp=cur%10+'0';
        //book[temp]
        s2[i]=cur%10+'0';
        up=cur/10;
    }

    if(up!=0){
        char t=up+'0';
        s2=t+s2;
    }

    if(s1.size()!=s2.size()){
        cout<<"No\n"<<s2;
        return 0;
    }
    //cout<<s1.size()<<" "<<s2.size()<<endl;
    for(int i=0; i<s1.size(); i++){
        flag = false;
        for(int j = 0; j < s2.size(); j++){
            if(exist[j]==false&&s2[j]==s1[i]){
                exist[j] = true;
                flag = true;
                break;
            }
        }
        if(flag==false){
            cout<<"No\n"<<s2;
            return 0;
        }
    }

    cout<<"Yes\n";
    cout<<s2;

    return 0;
}

int main(){
    char s[24]={0};
    int book[10]={0},len;
    scanf("%s",&s);
    len=strlen(s);

    int flag = 0;
    for(int i=len-1; i >= 0; i--){
        int temp=s[i]-'0';
        book[temp] ++;
        temp=temp*2+flag;
        flag=0;
        if(temp>=10){
            flag= 1;
            temp-=10;
        }
        book[temp]--;
        s[i]=temp+'0';
    }
    bool flag1=true;
    for(int i=0; i<10; i++){
        if(book[i]!=0){
            flag1=false;
            break;
        }
    }

    printf("%s\n", (flag==1||flag1==false) ?"No":"Yes");
    if(flag==1) printf("1");
    printf("%s",s);
    return 0;
}

```

