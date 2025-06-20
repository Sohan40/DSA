# 🧾 Problem: [Maximum Sum Increasing Subsequence](https://www.geeksforgeeks.org/problems/maximum-sum-increasing-subsequence4749/)

## 📝 Description
Given an array of integers `nums`, find the sum of the maximum sum increasing subsequence (MSIS). An increasing subsequence is a sequence where each element is strictly greater than the previous one. The task is to select a subsequence of `nums` such that it is strictly increasing and the sum of its elements is maximized.


## 🧠 Brute Force Approach:
- Try all possible subsequences and check if they are strictly increasing. For each valid subsequence, calculate its sum and keep track of the maximum.
- Time Complexity: O(2^n) (since each element can be included or excluded)
- Space Complexity: O(n) (for recursion stack)

### 💻 Brute Force Code:
```cpp
class Solution {
private:
    int solve(vector<int>& nums, int i, int last) {
        if(i >= nums.size()) return 0;
        
        int take = 0;
        if(last == -1 || nums[i] > nums[last]) 
            take = solve(nums, i+1, i) + nums[i];
            
        int skip = solve(nums, i+1, last);
        
        return max(take, skip);
    }
public:
    int maxSumIS(vector<int>& nums) {
        return solve(nums, 0, -1);
    }
};
```

## 🧠 Memoization Approach:
- Add memoization to avoid recalculating the same subproblems.
- Use a 2D DP array to store results of (index, last_taken_index).
- Time Complexity: O(n²)
- Space Complexity: O(n²)

### 💻 Memoization Code:
```cpp
class Solution {
private:
    int solve(vector<int>& nums, int i, int last, vector<vector<int>>& dp) {
        if(i >= nums.size()) return 0;
        
        if(dp[i][last+1] != -1) return dp[i][last+1];
        
        int take = 0;
        if(last == -1 || nums[i] > nums[last]) 
            take = solve(nums, i+1, i, dp) + nums[i];
            
        int skip = solve(nums, i+1, last, dp);
        
        return dp[i][last+1] = max(take, skip);
    }
public:
    int maxSumIS(vector<int>& nums) {
        vector<vector<int>> dp(nums.size()+1, vector<int>(nums.size()+2, -1));
        return solve(nums, 0, -1, dp);
    }
};
```
## 🔍 How It Works:
1. For each index, decide whether to include the current element in the subsequence (if it is greater than the last included element) or skip it.
2. Use recursion to explore both choices.
3. Memoize results to avoid recomputation.
4. The answer is the maximum sum obtained by either including or skipping each element.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(n²) - There are n possible values for `i` and up to n for `last`, so O(n²) subproblems.
- Space Complexity: O(n²) - For the DP table and recursion stack.




## 💡 Tabulation:
- Convert the recursive solution to an iterative one using a bottom-up approach.
- Fill the DP table from right to left, considering all possible previous elements for each index.
- Time Complexity: O(n²)
- Space Complexity: O(n²)

### 💻 Tabulation Code:
```cpp
class Solution {
public:
    int maxSumIS(vector<int>& nums) {
        vector<vector<int>> dp(nums.size()+1, vector<int>(nums.size()+2, 0));
        
        for(int last = -1; last <= nums.size(); last++) 
            dp[nums.size()][last+1] = 0;
            
        for(int i = nums.size()-1; i >= 0; i--) {
            for(int last = -1; last <= i-1; last++) {
                int take = 0;
                if(last == -1 || nums[i] > nums[last]) 
                    take = dp[i+1][i+1] + nums[i];
                    
                int skip = dp[i+1][last+1];
                
                dp[i][last+1] = max(take, skip);
            }
        }
        return dp[0][0];
    }
};
```

## 💡 Space Optimization:
- Reduce space complexity by using only two 1D arrays (current and next row).
- Only the previous row is needed to compute the current row.
- Time Complexity: O(n²)
- Space Complexity: O(n)

### 💻 Code:
```cpp
class Solution {
public:
    int maxSumIS(vector<int>& nums) {
        vector<int> next(nums.size()+2, 0), cur(nums.size()+1, 0);
        
        for(int i = nums.size()-1; i >= 0; i--) {
            for(int last = -1; last <= i-1; last++) {
                int take = 0;
                if(last == -1 || nums[i] > nums[last]) 
                    take = next[i+1] + nums[i];
                    
                int skip = next[last+1];
                
                cur[last+1] = max(take, skip);
            }
            next = cur;
        }
        return cur[0];
    }
};
```

---

## 💡 Most Intuitive Space Optimized Approach:
- Use a single array to store the maximum sum of increasing subsequence ending at each index.
- For each element, check all previous elements to find the maximum sum subsequence it can extend.
- Time Complexity: O(n²)
- Space Complexity: O(n)

### 💻 Code:
```cpp
class Solution {
public:
    int maxSumIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(nums.begin(), nums.end()); // dp[i] = max sum of increasing subsequence ending at i
        for(int i = 1; i < n; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + nums[i]);
                }
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

## 📌 Key Takeaways:
- The problem is a variation of the Longest Increasing Subsequence (LIS), but instead of length, we maximize the sum.
- Space optimization is possible by observing dependencies in the DP table.
- The most intuitive approach is to use a 1D DP array where each entry represents the maximum sum of an increasing subsequence ending at that index.

## 🎯 Example:
Suppose `nums = [1, 101, 2, 3, 100, 4, 5]`

- The maximum sum increasing subsequence is `[1, 2, 3, 100]` with sum `106` or `[1, 101]` with sum `102`.
- The answer is `106`.

```
Input: [1, 101, 2, 3, 100, 4, 5]
Output: 106
``` 