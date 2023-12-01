# ACM输入输出模式训练Day1

## A+B问题I

**问题描述**

输入包含一系列的a和b对，通过空格隔开。一对a和b占一行。

```C++
#include<iostream>
using namespace std;
int main(){
    int a, b;
    while (cin >> a >> b){
        cout << a + b << endl;
    }
}
```





## A+B问题II

**问题描述**

第一行是一个整数N，表示后面会有N行a和b，通过空格隔开。

```C++
#include<iostream>
using namespace std;
int main(){
    int n, a, b;
    while (cin >> n){
        while (n--){
            cin >> a >> b;
            cout << a + b << endl;
        }
    }
}
```

## A+B问题III

**问题描述**

输入中每行是一对a和b。其中会有一对是0和0标志着输入结束，且这一对不要计算。

```C++
#include<iostream> 
using namespace std;
int main(){
    int a, b;
    while (cin >> a >> b){
        if (a != 0 && b != 0){
            std::cout << a + b << std::endl;
        }
    }
}
```

## A+B问题IV

**问题描述**

每行的第一个数N，表示本行后面有N个数。如果N=0时，表示输入结束，且这一行不要计算。

```C++
#include<iostream>
using namespace std;
int main(){
    int n, a;
    while (cin >> n){   
    if (n == 0) break;
    int sum = 0;
    while (n--){
        cin >> a;
        sum += a;
    }
    cout << sum <<endl;
	}
}
```

##  A+B问题VII
