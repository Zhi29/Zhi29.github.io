---
title: 'Dynamic Programming'
date: 2020-02-25 00:00:00
featured_image: '/images/demo/demo-square.jpg'
excerpt: The Blog for Dynamic Programming
---

### 300. Longest Increasing Subsequence

#### dp 含义

dp[i]表示在 i 位置时，最长的increasing Subsequence

#### 递推关系

比较输入数组的两个位置的值，如果nums[i] > nums[j] (i > j)， 则说明increasing subsequence 可以更长了，此时dp[i] = dp[j] + 1, 但同时还要跟本身dp[i]的值比较（因为不断循环i位置之前的所有子数组），如果本身就大，就不用再更新dp[j]+1了。

```C++
int lengthOfLIS(vector<int>& nums) {
    if(nums.size() < 1) return 0;
    vector<int> dp(nums.size(), 1);
    for(int i = 1; i < nums.size(); i++){
        for(int j = i-1; j >=0 ; j--){
            if(nums[i] > nums[j]){
                dp[i] = max(dp[i], dp[j]+1);
            }
        }
    }
    
    int LIS = 0;
    for(int i = 0; i < dp.size(); i++){
        LIS = max(LIS, dp[i]);
    }
    return LIS;
}
```

### 516 Longest Palindromic Subsequence  & 相似题 647 Palindromic Substrings
注意 subsequence 并不一定连续 而 substring 则是要连续

#### dp 含义

**Longest Palindromic Subsequence:** dp[i][j] 表示在一段字符中从i 到 j 中最长回文子串的长度（只需要上三角就够了） 初始值：当总长度为奇数时，dp[i][i] = 1。当总长度为偶数时，根据dp[i][i+1]情况判断长度为1或2。

**Palindromic Substrings:** dp[i][j] 表示一段字符串中从 i 到 j 为回文串，是就标为true。 与上一个类似， dp[i][i] = true, dp[i][i+1] = true 如果两个相连的字符一样。初始化： 全部为false

#### 递推关系
**Longest Palindromic Subsequence:** 当我们不断增加我们看子字符串的长度（从3开始直到整个字符串）我们每次比较所看长度的最边缘两个字符，如果一样，就dp[i][j] = dp[i+1][j-1] +2，如果不一样，那我们就要将两边都退一步来比较哪个更大，即，dp[i][j] = max(dp[i+1][j], dp[i][j-1]);

**Palindromic Substrings:** 与上一个类似，当我们不断增加我们看子字符串的长度 （从3开始直到整个字符串）如果我们每次比较所看长度的最边缘两个字符并且出最边缘两个字符外里面的是回文串，即（dp[i+1][j-1] == true）则 dp[i][j] = true. 我们用一个变量来记录回文串的数目。

**Longest Palindromic Subsequence:**
```C++
//516 Longest Palindromic Subsequence
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
     
        for(int m = 3; m <= n; m++){//m is the length of current subsequence e.g. when m = 3 the subsequence is bbb bba bab
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

**Palindromic Substrings:**

```C++
//647 Palindromic Substrings
int countSubstrings(string s){
    vector<vector<bool>> dp(s.length(), vector<bool>(s.length(), false));
    int count = 0;
    
    for(int i = 0; i < s.length(); i++){
        dp[i][i] = true;
        count++;
    }
    
    for(int i = 0; i < s.length()-1; i++){
        if(s[i] == s[i+1]){
            dp[i][i+1] = true;
            count++;
        }     
    }
    
    for(int m = 3; m <= s.length(); m++){
        for(int i = 0; i <= s.length() - m; i++){
            int j = i + m - 1;
            if(s[i] == s[j] && dp[i+1][j-1]){
                dp[i][j] = true;
                count++;
            }
        }
    }
    return count;
}
```

### 576 Out of Boundary Paths & 688 Knight Probability in Chessboard

#### dp 含义

出界问题：dp[i][j] 表示第n步（一共k步）时，进入i，j位置的路线数 初始值：根据含义，因为我们需要更新的是每个位置的路线数，因此开始是0，起始位置是1
棋盘问题：dp[i][j] 表示第n步（一共k步）时，能到达i, j位置有几种走法 初始值：根据含义，每个位置都能从某一个位置达到因此初始值为1

#### 递推关系

**出界问题：**

如果从i,j位置朝着上下左右走一步后就可以出界，说明当前位置，再走一步就可以实现出界，因此就可以将现在dp[i][j]中存储的第n步的出界方式数目加到计数总和中。如果走一步后没有出界，就在走一步的那个位置上原本的计数(dp[i+1][j] + dp[i][j])加上dp[i][j]，（根据dp的含义为进入某一位置的路线数目，那反过来也就是能出去的路线数）实际操作中通常增加一个与dp相同大小的变量来不断更新。

**棋盘问题：**

骑士从“别的地方”跳到i,j位置，如果这个“别的地方”是在界外，那么就continue忽略，如果“别的地方”是合理的，则当前位置dp[i][j]存储的走法（上一步，即n-1）加上从“别的地方”的走法就是这一点在第n步的走法。

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

### 673 Number of Longest Increasing Subsequence

##### dp 含义

本题使用的是一维dp数组，dp[i] 的含义为在这个位置前的数组有多少个最长increasing subsequence

##### 递推关系

本题的递推关系比较复杂，感觉不属于典型类型。需要维护两个数组，第一个叫length，用来记录到 i 位置时最长的increasing subsequence。 第二个数组叫做dp，用来记录到 i 位置时有多少个最长increasing subsequence。

当我们比较给定输入数组中的两个位置的数时，如果nums[i] > nums[j] (i > j)

我们比较length[i] 和 length[j]的大小，
**当length[i] < length[j]** 
也就是说如果 i 位置来看从0-i位置的子数组中最长increasing subsequence 比 在 j （i > j） 位置小。那么首先，我们更新这个i 位置的长度。然后dp[i] = dp[j] 本身i位置的increasing subseq 就小 所以 i 位置自然就应该把之前数目最多的延续下来，也就是 j。 
**当length[i] >= length[j]时，并且 length[i] == length[j] + 1**
如果两个位置从0-i位置的子数组中最长increasing subsequence长度一样，那么我们就在dp[i] 的基础上再加上 dp[j]

```C++
int findNumberOfLIS(vector<int>& nums) {
    vector<int> length(nums.size(),0);
    vector<int> dp(nums.size(), 1);

    for(int i = 0 ; i < nums.size(); i++){
        for(int j = 0; j <= i; j++){
            if(nums[i] > nums[j]){
                if(length[i] <= length[j]){
                    length[i] = length[j] + 1;
                    dp[i] = dp[j];
                }
                else if(length[j] + 1 == length[i])
                    dp[i] += dp[j];
            }
            
        }
    }
    
    vector<int>::iterator longest = max_element(length.begin(), length.end());
    int result = 0;
    for(int i = 0; i < dp.size(); i++){
        if(length[i] == *longest)
            result += dp[i];
    }
    return result;
}
```
