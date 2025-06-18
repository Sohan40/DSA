# 🧾 Problem: Edit Distance ([Leetcode 72](https://leetcode.com/problems/edit-distance/))

## 📝 Description
Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2. You may perform the following operations on a word:
- Insert a character
- Delete a character
- Replace a character

This is a classic problem in dynamic programming and string manipulation, often called the Levenshtein Distance.


## 🧠 Brute Force Approach:
- Try all possible ways to convert word1 to word2 by recursively considering all three operations (insert, delete, replace) at each step.
- Time Complexity: O(3^(m+n)), where m and n are the lengths of the two words (very inefficient for large strings)
- Space Complexity: O(m+n) (due to recursion stack)

### 💻 Brute Force Code:
```cpp
class Solution {
public:
    int solve(string word1,string word2,int i , int j){

        if(i>=word1.length()){
            return word2.length()-j;
        }
        if(j>=word2.length()){
            return word1.length()-i;
        }

        if(word1[i]==word2[j]) return solve(word1,word2,i+1,j+1) ;
    
        else return min({solve(word1,word2,i+1,j+1)+1,solve(word1,word2,i,j+1)+1,solve(word1,word2,i+1,j)+1});
    }
    int minDistance(string word1, string word2) {
        if(word1==word2) return 0;
        return solve(word1,word2,0,0);
    }
};
```

## 🔍 How It Works (Brute Force):
1. Start with two pointers, i for word1 and j for word2, both at 0.
2. If either pointer reaches the end of its string:
   - If i reaches the end of word1, the remaining characters in word2 (from j onwards) must all be inserted, so return word2.length() - j.
   - If j reaches the end of word2, the remaining characters in word1 (from i onwards) must all be deleted, so return word1.length() - i.
3. If word1[i] == word2[j], no operation is needed for this character. Move both pointers forward (i+1, j+1).
4. If word1[i] != word2[j], try all three operations:
   - Insert: Insert word2[j] into word1 at position i, then move j forward (i, j+1). (We move only j because we've matched word2[j] by inserting it, so we need to match the next character in word2 with the same character in word1.)
   - Delete: Delete word1[i], then move i forward (i+1, j). (We move only i because we've removed word1[i], so we need to try matching the next character in word1 with the same character in word2.)
   - Replace: Replace word1[i] with word2[j], then move both pointers forward (i+1, j+1). (Both characters are now matched, so we move both forward.)
5. For each operation, add 1 to the result (since an operation is performed), and recursively compute the minimum among the three options.
6. The answer is the minimum number of operations needed to make the substrings word1[i:] and word2[j:] equal.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(3^(m+n)) - Each step branches into three possibilities.
- Space Complexity: O(m+n) - Maximum depth of recursion.


-----------------------

## 💡 Memoization Approach:
- Use a 2D DP array to store results of subproblems to avoid redundant calculations.
- Advantages:
  - Avoids recalculating the same subproblems
  - Much faster than brute force
  - Easy to implement with recursion

### 💻 memoization Code:
```cpp
class Solution {
public:
    int solve(string word1,string word2,int i , int j,vector<vector<int>>&dp){

        if(i>=word1.length()){
            return word2.length()-j;
        }
        if(j>=word2.length()){
            return word1.length()-i;
        }

        if(dp[i][j]!=-1) return dp[i][j];

        if(word1[i]==word2[j]) return solve(word1,word2,i+1,j+1,dp) ;
    
        else 
        return 
        dp[i][j]=min({solve(word1,word2,i+1,j+1,dp)+1,solve(word1,word2,i,j+1,dp)+1,solve(word1,word2,i+1,j,dp)+1});
    }
    int minDistance(string word1, string word2) {
        if(word1==word2) return 0;
        vector<vector<int>>dp(word1.length()+1,vector<int>(word2.length()+1,-1));
        return solve(word1,word2,0,0,dp);
    }
};
```

## 🔍 How It Works:
1. Use a DP table dp[i][j] to store the minimum operations needed to convert word1[i:] to word2[j:].
2. If the result is already computed, return it.
3. Otherwise, compute as in brute force, but store and reuse results.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(m*n) - Each subproblem is solved once.
- Space Complexity: O(m*n) - For the DP table.


-----------------------

## 💡 Tabulation Approach:
- Build the solution bottom-up using a 2D DP table.
- Advantages:
  - No recursion stack (iterative)
  - Easy to visualize and debug
  - Efficient for moderate string lengths

### 💻 tabulation Code:
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        if(word1==word2) return 0;
        vector<vector<int>>dp(word1.length()+1,vector<int>(word2.length()+1,0));
        int m = word1.length();
        int n = word2.length();
        for(int j = 0 ; j  <= word2.length(); j++){
            dp[m][j] = n-j;
        }
        for(int i = 0 ; i  <= word1.length(); i++){
            dp[i][n] = m-i;
        }

        for(int i = m-1 ; i>=0 ; i--){

            for(int j = n-1 ; j>=0 ; j--){

                if(word1[i]==word2[j]) dp[i][j] = dp[i+1][j+1] ;
    
                else dp[i][j]=min({dp[i+1][j+1]+1,dp[i][j+1]+1,dp[i+1][j]+1});
            }
        }
        return dp[0][0];
    }
};
```

## 🔍 How It Works:
1. Initialize the last row and column of the DP table for base cases (empty substrings).
2. Fill the table from the bottom-right to the top-left.
3. At each cell, if characters match, copy the diagonal value; otherwise, take 1 + min of three possible operations.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(m*n) - Each cell is filled once.
- Space Complexity: O(m*n) - For the DP table.


-----------------------

## 💡 Space optimization Approach:
- Use only two 1D arrays instead of a full 2D table, since only the current and next rows are needed at any time.
- Advantages:
  - Reduces space from O(m*n) to O(n)
  - Still O(m*n) time
  - Suitable for large strings

### 💻 space optimization Code:
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        if(word1==word2) return 0;

        vector<int>cur(word2.length()+1,0),next(word2.length()+1,0);
        int m = word1.length();
        int n = word2.length();

        for(int j = 0 ; j  <= word2.length(); j++){
            next[j] = n-j;
        }
       
        for(int i = m-1 ; i>=0 ; i--){
            cur[n] = m-i;
            for(int j = n-1 ; j>=0 ; j--){

                if(word1[i]==word2[j]) cur[j] = next[j+1] ;
    
                else cur[j]=min({next[j+1]+1,cur[j+1]+1,next[j]+1});
            }

            next = cur;
        }
        return next[0];
    }
};
```

## 🔍 How It Works:
1. Use two arrays: one for the current row, one for the next row.
2. Update the arrays as you iterate through the string.
3. At the end of each row, swap the arrays.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(m*n) - Each cell is computed once.
- Space Complexity: O(n) - Only two rows are stored at a time.


-----------------------

## 📌 Key Takeaways:

- In the space optimized approach, you should always return next[0] (not cur[0]), because after processing each row, next[] holds the results for the previous row, which is what the final answer depends on.


## 🎯 Example:
```
Input: word1 = "horse", word2 = "ros"

DP Table (Tabulation):
   h  o  r  s  e  
r  5  4  3  3  2  3
o  5  4  3  2  2  2
s  5  4  3  2  1  1
   5  4  3  2  1  0

Output: 3
Explanation: horse -> rorse (replace 'h' with 'r'), rorse -> rose (remove 'o'), rose -> ros (remove 'e')
```
Result: 3 