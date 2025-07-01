# 🧾 Problem: Longest Palindromic Substring ([LeetCode 5](https://leetcode.com/problems/longest-palindromic-substring/))

## 📝 Description
Given a string `s`, find the longest palindromic substring in `s`. A palindrome is a string that reads the same backward as forward. You need to return the substring itself, not its length.

**Significance:**
- This is a classic dynamic programming problem that helps build intuition for substring problems and palindromic properties.
- It is frequently asked in coding interviews and is a foundation for more advanced string and DP problems.

---

## 🧠 Brute Force Approach:
- **Description:**
  - Check all possible substrings of `s` and verify if each is a palindrome.
  - Track the longest palindromic substring found.
- **Time Complexity:** O(n³) (n² substrings, O(n) to check each)
- **Space Complexity:** O(1) (ignoring output)

### 💻 Brute Force Code:
```cpp
class Solution {
public:
    int sp = 0;
    int maxlen = 0;
    int solve(int i , int j , string&s)
    {
        while(i<j){

            if(s[i]==s[j]){
                i++;j--;
            }
            else return false;
        }
        return true;
    }
    string longestPalindrome(string s) {
        
        for(int i =0 ; i < s.length();i++){
            for(int j = 0 ; j < s.length();j++){
                if(solve(i,j,s) && (j-i+1 > maxlen)){
                    sp = i;
                    maxlen = j-i+1;
                }
            }
        }

        return s.substr(sp,maxlen);
    }
};
```

## 💡 Memoization Approach:
- **Description:**
  - Use recursion with memoization (top-down DP) to avoid recomputation.
  - Store results of subproblems in a DP table `dp[i][j]` indicating if `s[i..j]` is a palindrome.
- **Advantages:**
  - Avoids redundant checks for the same substring.
  - Reduces time complexity compared to brute force.
  - Builds intuition for bottom-up (tabulation) DP.

### 💻 Memoization Code:
```cpp
class Solution {
public:
    int sp = 0;
    int maxlen = 0;
    int solve(int i , int j , string&s,vector<vector<int>>&dp)
    {
        if(dp[i][j]!=-1) return dp[i][j];

        if(i>=j) return dp[i][j]=1;
        else if(s[i]==s[j]) return solve(i+1,j-1,s,dp);
        else return dp[i][j] = 0;       
    }
    string longestPalindrome(string s) {
        vector<vector<int>>dp(1001,vector<int>(1001,-1));
        
        for(int i =0 ; i < s.length();i++){
            for(int j = 0 ; j < s.length();j++){
                if(s[i]==s[j] && solve(i,j,s,dp) && (j-i+1 > maxlen)){
                    sp = i;
                    maxlen = j-i+1;
                }
            }
        }

        return s.substr(sp,maxlen);
    }
};
```

## 🔍 How It Works:
1. For each possible starting and ending index `(i, j)`, check if `s[i..j]` is a palindrome using recursion.
2. Use a DP table to store results for each `(i, j)` to avoid recomputation.
3. If `s[i] == s[j]` and the substring between them is a palindrome, then `s[i..j]` is a palindrome.
4. Track the longest palindromic substring found during the process.
5. Return the substring with the maximum length.

## ⏱️ Time & Space Complexity:
- **Time Complexity:** O(n²) (n² subproblems, each checked once)
- **Space Complexity:** O(n²) (for the DP table)

---

## 💡 Tabulation Approach:
- **Description:**
  - Use bottom-up dynamic programming to fill a table `dp[i][j]` where each entry is true if `s[i..j]` is a palindrome.
  - Start with substrings of length 1, then 2, then longer.
- **Advantages:**
  - Iterative and avoids recursion stack overhead.
  - Efficient for moderate string lengths.

### 💻 Tabulation Code:
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.length();
        if (n == 0) return "";

        vector<vector<bool>> dp(n, vector<bool>(n, false));
        int start = 0, maxLen = 1;

        // All substrings of length 1
        for (int i = 0; i < n; ++i)
            dp[i][i] = true;

        // Substrings of length ≥ 2
        for (int len = 2; len <= n; ++len) {
            for (int i = 0; i + len - 1 < n; ++i) {
                int j = i + len - 1;
                if (s[i] == s[j]) {
                    if (len == 2 || dp[i + 1][j - 1]) {
                        dp[i][j] = true;
                        if (len > maxLen) {
                            start = i;
                            maxLen = len;
                        }
                    }
                }
            }
        }

        return s.substr(start, maxLen);
    }
};

```

## 🔍 How It Works:
1. Initialize a DP table `dp[n][n]` where `dp[i][j]` is true if `s[i..j]` is a palindrome.
2. All substrings of length 1 are palindromes (`dp[i][i] = true`).
3. For substrings of length 2, check if both characters are equal.
4. For longer substrings, check if the first and last characters are equal and the substring between them is a palindrome (`dp[i+1][j-1]`).
5. Track the start index and length of the longest palindrome found.
6. Return the substring from the start index with the maximum length.

## ⏱️ Time & Space Complexity:
- **Time Complexity:** O(n²) (nested loops over substring lengths and positions)
- **Space Complexity:** O(n²) (for the DP table)

---

## 🎯 Example:
```
Input: s = "babad"

All palindromic substrings:
- "b", "a", "b", "a", "d" (all length 1)
- "bab" (length 3, palindrome)
- "aba" (length 3, palindrome)

Output: "bab" or "aba"
(Any one is correct as both are longest palindromic substrings)
```
**Example result/output:**
```
"bab"
```
---

## 🎯 Example:
```
Input: s = "babad"

All palindromic substrings:
- "b", "a", "b", "a", "d" (all length 1)
- "bab" (length 3, palindrome)
- "aba" (length 3, palindrome)

Output: "bab" or "aba"
(Any one is correct as both are longest palindromic substrings)
```
**Example result/output:**
```
"bab"
``` 