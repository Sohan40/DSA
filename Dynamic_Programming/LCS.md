# 🧾 Problem: [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## 📝 Description
Given two strings text1 and text2, return the length of their longest common subsequence. A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

## 🎯 Applications
- DNA sequence alignment in bioinformatics
- File difference detection (like git diff)
- Plagiarism detection
- Spell checking and auto-correction

## 🧠 Brute Force Approach:
- Use recursion to try all possible subsequences
- For each character, we have two choices: match or skip
- If characters match: move both pointers forward and add 1 to length
- If characters don't match: try both possibilities:
  * Skip current character in text1
  * Skip current character in text2
- Time Complexity: O(2^(m+n)) - Each character has 2 choices
- Space Complexity: O(m+n) for recursion stack

### 💻 Brute Force Code:
```cpp
class Solution {
public:
    int solve(string text1, string text2, int i, int j) {
        if(i >= text1.length() || j >= text2.length()) return 0;
        
        int ans = 0;
        if(text1[i] == text2[j]) {
            ans = solve(text1, text2, i+1, j+1) + 1;
        }
        else {
            ans = max(
                solve(text1, text2, i, j+1),
                solve(text1, text2, i+1, j)
            );
        }
        return ans;
    }
    
    int longestCommonSubsequence(string text1, string text2) {
        return solve(text1, text2, 0, 0); 
    }
};
```

## 💡 Memoization Approach:
- Add memoization to avoid recalculating same subproblems
- Use 2D DP array to store results of (i,j) states
- State (i,j) represents LCS length for text1[i...] and text2[j...]
- If characters match: dp[i][j] = 1 + dp[i+1][j+1]
- If characters don't match: dp[i][j] = max(dp[i+1][j], dp[i][j+1])
- Time Complexity: O(m*n) - Each state is calculated once
- Space Complexity: O(m*n) for DP array

### 💻 Memoization Code:
```cpp
class Solution {
public:
    int solve(string text1, string text2, int i, int j, vector<vector<int>>& dp) {
        if(i >= text1.length() || j >= text2.length()) return 0;
        
        if(dp[i][j] != -1) return dp[i][j];
        
        int ans = 0;
        if(text1[i] == text2[j]) {
            ans = solve(text1, text2, i+1, j+1, dp) + 1;
        }
        else {
            ans = max(
                solve(text1, text2, i, j+1, dp),
                solve(text1, text2, i+1, j, dp)
            );
        }
        return dp[i][j] = ans;
    }
    
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.length(), vector<int>(text2.length(), -1));
        return solve(text1, text2, 0, 0, dp); 
    }
};
```

## 💡 Tabulation Approach:
- Convert recursive solution to iterative using bottom-up approach
- Fill DP table from right to left
- Base case: dp[m][j] = dp[i][n] = 0 (empty string)
- For each position (i,j):
  * If text1[i] == text2[j]: dp[i][j] = 1 + dp[i+1][j+1]
  * Else: dp[i][j] = max(dp[i+1][j], dp[i][j+1])
- Time Complexity: O(m*n) - Fill entire DP table
- Space Complexity: O(m*n) for DP table

### 💻 Tabulation Code:
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.length()+1, vector<int>(text2.length()+1, 0));
        
        for(int i = text1.length()-1; i >= 0; i--) {
            for(int j = text2.length()-1; j >= 0; j--) {
                int ans = 0;
                if(text1[i] == text2[j]) {
                    ans = dp[i+1][j+1] + 1;
                }
                else {
                    ans = max(
                        dp[i][j+1],
                        dp[i+1][j]
                    );
                }
                dp[i][j] = ans;
            }
        }
        return dp[0][0];
    }
};
```

## 💡 Space optimization Approach:
- Reduce space complexity by using only two rows
- Keep track of current and next row
- For each position:
  * If characters match: cur[j] = 1 + next[j+1]
  * Else: cur[j] = max(cur[j+1], next[j])
- After processing each row: next = cur
- Time Complexity: O(m*n) - Same as tabulation
- Space Complexity: O(n) - Only two rows needed

### 💻 Space optimization Code:
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<int> cur(text2.length()+1, 0), next(text2.length()+1, 0);
        
        for(int i = text1.length()-1; i >= 0; i--) {
            for(int j = text2.length()-1; j >= 0; j--) {
                int ans = 0;
                if(text1[i] == text2[j]) {
                    ans = next[j+1] + 1;
                }
                else {
                    ans = max(
                        cur[j+1],
                        next[j]
                    );
                }
                cur[j] = ans;
            }
            next = cur;
        }
        return cur[0];
    }
};
```

## 🔍 How It Works:
1. Brute Force:
   - Start with both pointers at beginning of strings
   - For each position, try both possibilities:
     * If characters match: move both pointers and add 1
     * If characters don't match: try skipping in either string
   - Return maximum length found
   - Problem: Recalculates same subproblems multiple times

2. Memoization:
   - Store results of subproblems in DP array
   - Before calculating any state, check if already computed
   - If computed, return stored result
   - If not computed:
     * Calculate result
     * Store in DP array
     * Return result
   - Avoids recalculating same states

3. Tabulation:
   - Start from end of strings
   - Fill DP table from right to left
   - For each cell:
     * If characters match: add 1 to diagonal value
     * If characters don't match: take maximum of right and down values
   - Final answer in dp[0][0]

4. Space Optimization:
   - Instead of full DP table, use only two rows
   - Current row depends on next row
   - After processing each row:
     * Copy current row to next row
     * Reset current row
   - Maintains same logic with less space

## ⏱️ Time & Space Complexity:
- Brute Force: O(2^(m+n)) time, O(m+n) space
  * Exponential time due to trying all possibilities
  * Linear space for recursion stack
- Memoization: O(m*n) time, O(m*n) space
  * Each state calculated once
  * Full DP table needed
- Tabulation: O(m*n) time, O(m*n) space
  * Fill entire DP table once
  * Full DP table needed
- Space Optimized: O(m*n) time, O(n) space
  * Same time as tabulation
  * Only two rows needed

## 📌 Key Takeaways:
- LCS is a classic DP problem with multiple solution approaches
- Space optimization is possible by using only necessary rows
- The problem can be solved in O(m*n) time using DP
- Understanding the state transitions is crucial for DP solutions
- The space optimized solution maintains the same time complexity with reduced space
- The problem has applications in string matching and sequence alignment
- The DP state represents the LCS length for remaining portions of both strings
- The transition depends on whether current characters match or not

## 🎯 Example:
```
Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: The longest common subsequence is "ace" and its length is 3.
Step by step:
1. Match 'a': length = 1
2. Skip 'b' in text1
3. Match 'c': length = 2
4. Skip 'd' in text1
5. Match 'e': length = 3

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
All characters match in order.

Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
No characters match between the strings.
``` 