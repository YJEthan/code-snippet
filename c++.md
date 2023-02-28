 # 一些坑
 ## 浮点数精度
 在四舍五入的时候需要加上一个很小的小数（如0.000000000000001）
 如leetcode[这个题](https://leetcode.cn/problems/sparse-similarity-lcci/)
 代码第22行,末尾
 	`sprintf(s, "%d,%d: %.4f", i, j, 1.0 * help[i][j] / (docs[i].size() + docs[j].size() - help[i][j]) + 1e-9);`

```cpp
class Solution {
public:
    vector<string> computeSimilarities(vector<vector<int>>& docs) {
        int n = docs.size();
        unordered_map<int, vector<int>> mp;
        vector<vector<int>> help(n, vector<int>(n, 0));
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < docs[i].size(); j++) {
                int x = docs[i][j];
                for(auto& indx : mp[x]) {
                    help[indx][i] += 1;
                }
                mp[x].push_back(i);

            }
        }
        vector<string> ans;
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(help[i][j]) {
                    char s[102];
                    sprintf(s, "%d,%d: %.4f", i, j, 1.0 * help[i][j] / (docs[i].size() + docs[j].size() - help[i][j]) + 1e-9);
                    ans.push_back(s);
                }
            }
        }
        return ans;
    }
};
```

## set
### 初始化

```cpp
vector<int> a;
...
set<int> s(a.begin(), a.end());
```

## priority_queue() 
### 比较函数写法
	

```cpp
		auto cmp = [](pair<int, int>& left, pair<int, int>& right) {
            double a = 1.0 * (left.first + 1) / (left.second + 1) - 1.0 * left.first  / left.second;
            double b = 1.0 * (right.first + 1) / (right.second + 1) - 1.0 * right.first  / right.second;
            return a < b; // 从大到小，大的在堆顶
        };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> q(cmp);
```
