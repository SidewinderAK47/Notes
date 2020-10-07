1036 Boys vs Girls (25 分)

This time you are asked to tell the difference between the lowest grade of all the male students and the highest grade of all the female students.

### Input Specification:

Each input file contains one test case. Each case contains a positive integer *N*, followed by *N* lines of student information. Each line contains a student's `name`, `gender`, `ID` and `grade`, separated by a space, where `name` and `ID` are strings of no more than 10 characters with no space, `gender` is either `F` (female) or `M` (male), and `grade` is an integer between 0 and 100. It is guaranteed that all the grades are distinct.

### Output Specification:

For each test case, output in 3 lines. The first line gives the name and ID of the female student with the highest grade, and the second line gives that of the male student with the lowest grade. The third line gives the difference *g**r**a**d**e**F*−*g**r**a**d**e**M*. If one such kind of student is missing, output `Absent` in the corresponding line, and output `NA` in the third line instead.

### Sample Input 1:

```in
3
Joe M Math990112 89
Mike M CS991301 100
Mary F EE990830 95
```

### Sample Output 1:

```out
Mary EE990830
Joe Math990112
6
```

### Sample Input 2:

```in
1
Jean M AA980920 60
```

### Sample Output 2:

```out
Absent
Jean AA980920
NA
```





代码实现：

```
#include <iostream>
using namespace std;
int main(){
    int grades[2]={200,-1},n;
    string names[2];
    string ids[2];
    cin>>n;
    string name,ID;
    char sex;
    int grade;
    for(int i=0; i<n; i++){
        cin>>name>>sex>>ID>>grade;
        if(sex=='M'){
            if(grade<grades[0]){
                grades[0]=grade;
                ids[0]=ID;
                names[0]=name;
            }
        } else if(sex=='F'){
            if(grade>grades[1]){
                grades[1] = grade;
                ids[1]=ID;
                names[1]= name;
            }
        }
    }

    if(grades[1]!=-1)
        cout<<names[1]<<" "<<ids[1]<<"\n";
    else
        cout<<"Absent\n";
    if(grades[0]!=200)
        cout<<names[0]<<" "<<ids[0]<<"\n";
    else
        cout<<"Absent\n";
    if(grades[0]==200||grades[1]==-1)
        cout<<"NA";
    else
        cout<<grades[1]-grades[0];

    return 0;
}

```

