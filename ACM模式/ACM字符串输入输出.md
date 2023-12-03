# [ACM平均绩点](https://kamacoder.com/problempage.php?pid=1006)

```C++
#include <iostream>
using namespace std;

int main(){
    string s;
    while (getline(cin, s)){
        float sum = 0;
        int count = 0;
        int flag = 1;
        for (int i = 0; i < s.size(); i++){
            if (s[i] == 'A') {sum += 4; count++;}
            else if (s[i] == 'B') {sum += 3; count++;}
            else if (s[i] == 'C') {sum += 2; count++;}
            else if (s[i] == 'D') {sum += 1; count++;}
            else if (s[i] == 'F') {sum += 0; count++;}
            else if (s[i] == ' ') {continue;}
            else {
                flag = 0;
                cout << "Unknown" << endl;
                break;
            }
        }
        if (flag) printf("%.2f\n", sum / count);
    }
    return 0;
}
```

- 注意getline直接获取整一行数据，通过对一行中的所有数据进行枚举来进行计算

- 出现异常情况的flag
