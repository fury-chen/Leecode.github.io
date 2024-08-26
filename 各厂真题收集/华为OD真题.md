# 华为OD真题



## 模拟

- [P2500 - 【模拟】2023B-查字典 - 欧弟算法 (algomooc.com)](https://oj.algomooc.com/problem.php?id=2500)

  ```C++
  #include <iostream>
  #include <string>
  #include <vector>
  
  using namespace std;
  vector<string> findWordWithPrefix(const string& prefix, const vector<string>& dict){
  	vector<string> res;
  	for (const auto& word : dict){
  		if (word.find(prefix) == 0){
  			res.push_back(word);
  		}
  	}
  	return res;
  }
  
  int main(){
  	string prefix;
  	int dict_len;
  	cin >> prefix;
  	cin >> dict_len;
  	
  	vector<string> dict(dict_len);
  	//cout << "input string of dict:\n" << endl;
  	for (int i = 0; i < dict_len; i++){
  		cin >> dict[i];
  	} 
  	vector<string> res = findWordWithPrefix(prefix, dict);
  	if (res.size() == 0) cout << -1 << endl;
  	for (auto& s : res){
  		cout << s << endl;
  	}
  	return 0;
  } 
  ```

  
