

# [剑指 Offer 05. 替换空格](https://www.acwing.com/problem/content/description/17/)

```C++
class Solution {
public:
    string replaceSpaces(string &str) {
        int len = str.size();
        int count = 0;
        
        for (int i = 0; i < len; i++){
            if (str[i] == ' ') count++;
        }
        str.resize(len + 2 * count);
        int newLength = str.size();
        
        //cout << len << endl;
        //cout << newLength << endl;
        
        for (int i = newLength, j = len; j < i; i--, j--){
            if (str[j] == ' '){
                str[i] = '0';
                str[i - 1] = '2';
                str[i - 2] = '%';
                i -= 2;
            }
            else {
                str[i] = str[j];
            }
        }
        return str;
    }
};
```

- 注意替换空格开的新长度是2*count，空格原本也占一个位置，只需要多开两个位置就行

- 本题和替换数字是同一个模板

  

