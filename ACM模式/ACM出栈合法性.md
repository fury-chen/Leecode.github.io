# [出栈合法性](https://kamacoder.com/problempage.php?pid=1016)

- 本题的意思是按照自然数顺序来判断是否是合理的入栈顺序，即如果和输入对应则出栈，判断出栈顺序是否正确

```C++
#include<iostream>
#include<vector>
#include<stack>
using namespace std;
int main(){
    int N;
    int nums[105];
    while (cin >> N){
        if (N == 0) break;
        for (int index = 0; index < N; index++){
            cin >> nums[index];
        }
        stack<int> st;
        int index = 0;
        for (int i =  1; i <= N; i++){
            st.push(i);
            while (!st.empty() && st.top() == nums[index]){
                st.pop();
                index++;
            }
        }
        if (st.empty() && index == N) cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    return 0;
}

```

