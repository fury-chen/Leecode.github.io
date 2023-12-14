# [16. 位置互换](https://kamacoder.com/problempage.php?pid=1015)

## 题解思路

- 简单的互换，但是要注意函数参数中要使用引用/指针才能达到在原地修改的目的
- 注意循环里是i+=2

## 示例代码

```C++
#include <iostream>
#include <string>
using namespace std;

void swap(char &a, char &b){
    char tmp = a;
    a = b;
    b = tmp;
}
int main(){
    int n;
    cin >> n;
    string s;
    while (n--){
        cin >> s;
        for (int i = 0; i < s.size() - 1; i += 2){
            swap(s[i], s[i + 1]);
        }
        std::cout << s << std::endl;
    }
}
```

