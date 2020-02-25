---
title: 'Dynamic Programming'
date: 2018-06-30 00:00:00
featured_image: '/images/demo/demo-square.jpg'
excerpt: The Blog for Dynamic Programming
---

### 516 Longest Palindromic Subsequence
注意 subsequence 并不一定连续

#### dp 含义

dp[i][j] 表示在一段字符中从i 到 j 中最长回文子串的长度（只需要上三角就够了） 初始值：当总长度为奇数时，dp[i][i] = 1。当总长度为偶数时，根据dp[i][i+1]情况判断长度为1或2。

#### 递推关系
当我们不断增加我们看子字符串的长度（从3开始直到整个字符串）我们每次比较所看长度的最边缘两个字符，如果一样，就dp[i][j] = dp[i+1][j-1] +2，如果不一样，那我们就要将两边都退一步来比较哪个更大，即，dp[i][j] = max(dp[i+1][j], dp[i][j-1]);

```C++
int longestPalindromeSubseq(string s){
        //dp[i][j] represents the length of the LPS we can make 
        //if we consider the characters from s[i] to s[j]
        vector<int> tmp(s.length(),0);
        vector<vector<int>> dp;
        for(int i = 0; i < s.length(); i++){
            dp.push_back(tmp);
            dp[i][i] = 1;//handle for odd total length of given input
        }

        int n = s.length();
        for(int i = 0; i < s.length()-1; i++){//this for loop is for even total length of input
            if(s[i] == s[i+1])
                dp[i][i+1] = 2;
            else
                dp[i][i+1] = 1;
        }
     
        for(int m = 3; m <= n; m++){//m is the length of current subsequence e.g. bbb bba bab
            for(int i = 0; i <= n - m; i++){ //i is the current subsequence's starter
                int j = i + m - 1; // j is the current subsequence's ender
                if(s[i] == s[j])
                    dp[i][j] = dp[i+1][j-1] + 2;
                else
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
            }
        }
        return dp[0][n-1];
    }
```

### 576 Out of Boundary Paths & Knight Probability in Chessboard

#### dp 含义

出界问题：dp[i][j] 表示第n步（一共k步）时，进入i，j位置的路线数 初始值：根据含义，因为我们需要更新的是每个位置的路线数，因此开始是0，起始位置是1
棋盘问题：dp[i][j] 表示第n步（一共k步）时，能到达i, j位置有几种走法 初始值：根据含义，每个位置都能从某一个位置达到因此初始值为1

#### 递推关系

出界问题：
如果从i,j位置朝着上下左右走一步后就可以出界，说明当前位置，再走一步就可以实现出界，因此就可以将现在dp[i][j]中存储的第n步的出界方式数目加到计数总和中。如果走一步后没有出界，就在走一步的那个位置上原本的计数(dp[i+1][j] + dp[i][j])加上dp[i][j]，（根据dp的含义为进入某一位置的路线数目，那反过来也就是能出去的路线数）实际操作中通常增加一个与dp相同大小的变量来不断更新。

棋盘问题：骑士从“别的地方”跳到i,j位置，如果这个“别的地方”是在界外，那么就continue忽略，如果“别的地方”是合理的，则当前位置dp[i][j]存储的走法（上一步，即n-1）加上从“别的地方”的走法就是这一点在第n步的走法。

#### 代码

##### 出界问题
```C++
    int findPaths(int m, int n, int N, int i, int j) {//m,n为格子边界，N为一共可以走的步数， i，j为起始位置
        int result = 0;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        vector<vector<int>> dicts {{1,0}, {0,1}, {-1,0}, {0,-1}};
        dp[i][j] = 1;
        for(int k = 0; k < N; k++){
            vector<vector<int>> temp(m, vector<int>(n, 0));
            for(int r = 0; r < m ; ++r){
                for(int c = 0; c < n; ++c){
                    for(auto dict : dicts){
                        int x = r + dict[0], y = c + dict[1];
                        if(x < 0 || x >= m || y < 0 || y >= n)
                            result = (result + dp[r][c]) % 1000000007;
                        else
                            temp[x][y] = (temp[x][y] + dp[r][c]) % 1000000007;//dp[r][c]表示从r,c位置出界的路线数
                    }
                }
            }
            dp = temp;
        }
        return result;
    }
```

##### 棋盘问题
```C++
    double knightProbability(int N, int K, int r, int c) {//N为方格边长，K为一共可以走的步数，r,c为起始位置
        vector<vector<double>> dp(N, vector<double>(N,1));
        vector<vector<double>> dict {{1,2}, {2,1}, {1,-2}, {-2,1}, {-1,2},{2,-1},{-1,-2},{-2,-1}};
        for(int m = 0; m < K; m++){
            vector<vector<double>> temp(N, vector<double>(N,0));
            for(int i = 0; i < N; i++){
                for(int j = 0; j < N; j++){
                    for(auto dic : dict){
                        int x = i + dic[0], y = j + dic[1];
                        if(x < 0 || x >= N || y < 0 || y >= N) continue;
                        temp[i][j] += dp[x][y];//dp[i][j] 代表走第n步时（一共走K步）有多少种方式能到i，j位置上
                    }
                }
            }
            dp = temp;
        }
        return dp[r][c] / pow(8, K);
    }
```
