# 🧾 Problem: [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## 📝 Description
Given an integer array nums, return the length of the longest strictly increasing subsequence. A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements.

## 🎯 Applications
- Bioinformatics for DNA sequence analysis
- Pattern recognition in time series data
- Network routing optimization
- Stock market trend analysis

---

## 🧠 Brute Force Approach:
- Use recursion to try all possible subsequences
- For each element, we have two choices: take it or skip it
- Time Complexity: O(2^n)
- Space Complexity: O(n) for recursion stack

### 💻 Brute Force Code:
```cpp
class Solution {
private:
    int solve(vector<int>& nums, int i, int last) {
        if(i >= nums.size()) return 0;
        
        int take = 0;
        if(last == -1 || nums[i] > nums[last]) 
            take = solve(nums, i+1, i) + 1;
            
        int skip = solve(nums, i+1, last);
        
        return max(take, skip);
    }
public:
    int lengthOfLIS(vector<int>& nums) {
        return solve(nums, 0, -1);
    }
};
```

---

## 🧠 Memoization Approach:
- Add memoization to avoid recalculating same subproblems
- Use 2D DP array to store results of (index, last_taken_index)
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
            take = solve(nums, i+1, i, dp) + 1;
            
        int skip = solve(nums, i+1, last, dp);
        
        return dp[i][last+1] = max(take, skip);
    }
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<vector<int>> dp(nums.size()+1, vector<int>(nums.size()+2, -1));
        return solve(nums, 0, -1, dp);
    }
};
```

---

## 💡 Tabulation:
- Convert recursive solution to iterative using bottom-up approach
- Fill DP table from right to left
- Time Complexity: O(n²)
- Space Complexity: O(n²)

### 💻 Tabulation Code:
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<vector<int>> dp(nums.size()+1, vector<int>(nums.size()+2, 0));
        
        for(int last = -1; last <= nums.size(); last++) 
            dp[nums.size()][last+1] = 0;
            
        for(int i = nums.size()-1; i >= 0; i--) {
            for(int last = -1; last <= i-1; last++) {
                int take = 0;
                if(last == -1 || nums[i] > nums[last]) 
                    take = dp[i+1][i+1] + 1;
                    
                int skip = dp[i+1][last+1];
                
                dp[i][last+1] = max(take, skip);
            }
        }
        return dp[0][0];
    }
};
```

---

## 💡 Space Optimization:
- Reduce space complexity by using only two rows
- Keep track of current and next row
- Time Complexity: O(n²)
- Space Complexity: O(n)

### 💻 Code:
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> next(nums.size()+2, 0), cur(nums.size()+1, 0);
        
        for(int i = nums.size()-1; i >= 0; i--) {
            for(int last = -1; last <= i-1; last++) {
                int take = 0;
                if(last == -1 || nums[i] > nums[last]) 
                    take = next[i+1] + 1;
                    
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

## 💡 One more Space optimized Approach:
- Use a single array to store the length of LIS ending at each index
- For each element, check all previous elements to find the maximum LIS length
- Time Complexity: O(n²)
- Space Complexity: O(n)

### 💻 Code:
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int cur_max = 0;
        int ans = -1;
        vector<int> dp(nums.size(), 1);
        
        for(int i = 0; i < nums.size(); i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

---

## 💡 Optimal Approach:
- Use binary search to find the correct position for each element
- Maintain a sorted array of potential LIS elements
- For each element:
  * If it's larger than all elements in array, append it
  * Otherwise, replace the first element that's >= current element
- Time Complexity: O(n log n)
- Space Complexity: O(n)

### 💻 Code:
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> arr;
        arr.push_back(nums[0]);
        
        for(int i = 1; i < nums.size(); i++) {
            auto it = lower_bound(arr.begin(), arr.end(), nums[i]);
            if(it == arr.end()) 
                arr.push_back(nums[i]);
            else
                arr[it - arr.begin()] = nums[i];
        }
        return arr.size();
    }
};
```

---

## 🔍 How It Works:
1. Brute Force:
   - Try all possible subsequences using recursion
   - For each element, decide whether to take it or skip it
   - Return maximum length found

2. Memoization:
   - Store results of subproblems in DP array
   - Avoid recalculating same states
   - Use last taken index to ensure increasing order

3. Tabulation:
   - Fill DP table from bottom to top
   - For each position, consider taking or skipping current element
   - Use previous results to build solution

4. Space Optimization:
   - Use only two rows instead of full DP table
   - Update current row using next row
   - Swap rows after each iteration

5. One more Space Optimized Approach:
   - Use single array to store LIS lengths
   - For each element, check all previous elements
   - Update length if current element can extend previous LIS

6. Optimal Approach:
   - Maintain sorted array of potential LIS elements
   - Use binary search to find correct position
   - Replace or append elements to maintain increasing sequence
   - Final array length gives LIS length

## ⏱️ Time & Space Complexity:
- Brute Force: O(2^n) time, O(n) space
- Memoization: O(n²) time, O(n²) space
- Tabulation: O(n²) time, O(n²) space
- Space Optimized: O(n²) time, O(n) space
- One more Space Optimized: O(n²) time, O(n) space
- Optimal: O(n log n) time, O(n) space

## 📌 Key Takeaways:
- LIS is a classic DP problem with multiple solution approaches
- Space optimization is possible by using only necessary rows
- Binary search can be used to optimize time complexity
- The problem can be solved in O(n log n) time using patience sorting
- Understanding the state transitions is crucial for DP solutions
- The optimal approach uses binary search to maintain a potential LIS array
- The final array in optimal approach may not contain the actual LIS, but its length is correct

## 🎯 Example:
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,5,7,101], therefore the length is 4.

Input: nums = [0,1,0,3,2,3]
Output: 4
Explanation: The longest increasing subsequence is [0,1,2,3], therefore the length is 4.
``` 