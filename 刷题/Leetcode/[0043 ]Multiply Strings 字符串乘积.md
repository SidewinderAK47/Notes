# [Leetcode-43 ]Multiply Strings 字符串乘积 

单词：**multiply**乘

问题描述：

Medium

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Note:**

1. The length of both `num1` and `num2` is < 110.
2. Both `num1` and `num2` contain only digits `0-9`.
3. Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.



比较优秀的解决思路：（来自leetcode）局局的

```C++
class Solution {
public:
    string multiply(string num1, string num2) {
        int m=num1.size(),n=num2.size();

        int ans[110*110]={0};//一共是n+m位
        int sum;
        for(int i=m-1; i>=0; i--){
            for(int j=n-1; j>=0; j--){
                sum=(num1[i]-'0')*(num2[j]-'0')+ ans[i+j+1];
                ans[i+j+1]=sum%10;
                ans[i+j]+=sum/10;
            }
        }
        string rst;
        char c;
        int k=0;
        for(; k<m+n;k++){
            if(ans[k]!='\0')
                break;
        }
        while(k<m+n){
            c=(ans[k]+'0');
            rst+=c;
            k++;
        }
        return rst==""? "0":rst;
    }
};
```

> 自己的思路：能解决但是效率&代码量都远远超过了最有解，最优解使用了动态规划算法。
>
> 主要的解题思路是：m位 * n位 总共的位数是m+n位以下， 第i位 *第j位 的结果大概在 结果的第i+j位和i+j+1位；

```C++
 string multiply(string num1, string num2) {
        if(num1=="0"||num2=="0")
            return "0";
       string result="0",t; //初始化为0
       for(int i=num2.size()-1; i>=0; i--){
           string t=num1;
           for(int j=0; j<num2.size()-1-i; j++){
               t=t+'0';
           }
           t = muliplyNum(t,num2[i]-'0');
           result=add(result,t);
         //  cout<<t<<endl;
         //  cout<<result<<endl;
       }

       return result;
    }
    string add(string num1,string num2){
        string sum;
        int up=0,cur;
        char c;
        int i=num1.size()-1, j=num2.size()-1;
        for(; i>=0&&j>=0; i--,j--){
            cur = up + (num1[i]-'0')+(num2[j]-'0');
            up=cur/10;
            cur%=10;
            c=cur+'0';
            sum=c+sum;
        }
        while(i>=0){
            cur=up + (num1[i]-'0');
            up=cur/10;
            cur%=10;
            c=cur+'0';
            sum=c+sum;
            i--;
        }
        while(j>=0){
            cur=up + (num2[j]-'0');
            up=cur/10;
            cur%=10;
            c=cur+'0';
            sum=c+sum;
            j--;
        }
        if(up){
            c=up+'0';
            sum=c+sum;
        }
       // cout<<"sum:"<<sum<<endl;
        return sum;
    }
    string muliplyNum(string num ,int k){

        string rst;
        int up=0,cur;
        char c;
        for(int i=num.size()-1; i>=0; i--){
            cur=up+(num[i]-'0')*k;
            up=cur/10;
            cur%=10;
            c=cur+'0';
            rst=c+rst;
        }
        if(up){
            c=up+'0';
            rst =c+rst;
        }
        return rst;
    }
```

