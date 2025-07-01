# 🧾 Problem: [0-1 Knapsack](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)

## 📝 Description
Given weights and values of N items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. Each item can be included at most once (0-1 property). You cannot break an item, either pick the complete item or don't pick it (0-1 property).

- **Input:**
  - N: Number of items
  - W: Maximum capacity of knapsack
  - val[]: Array of values of items
  - wt[]: Array of weights of items
- **Output:**
  - Maximum value that can be put in a knapsack of capacity W

---

## 🧠 Brute Force Approach:
- Try all possible combinations of including or excluding each item.
- For each item, you have two choices: include it (if it fits) or exclude it, and recursively solve for the rest.
- This leads to a binary tree of choices.
- **Time Complexity:** O(2^N) (since each item has two choices)
- **Space Complexity:** O(N) (due to recursion stack)

### 💻 Brute Force Code:
```cpp
class Solution {

public:
    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        int n = val.size();

        function<int(int, int)> solve = [&](int i, int weight) -> int {
            if (i == n) return 0;

            int pick = 0;
            if (weight - wt[i] >= 0)
                pick = solve(i + 1, weight - wt[i]) + val[i];

            int notpick = solve(i + 1, weight);

            return max(pick, notpick);
        };

        return solve(0, W);
    }
};
```

---

## 💡 Memoization Approach:
- Use a 2D DP array to store results of subproblems to avoid recomputation.
- Store the result for each (i, weight) pair.
- **Advantages:**
  - Avoids redundant calculations by caching results.
  - Reduces time complexity from exponential to polynomial.
  - Easy to implement as a top-down approach.

### 💻 Code:
```cpp
class Solution {
  public:

    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        // code here
        vector<vector<int>>dp(1001,vector<int>(W+1,-1));
        
        function<int(int, int)> solve = [&](int i , int weight ) -> int{
                
            if(i>=val.size()) return 0;
            
            if(dp[i][weight]!=-1) return dp[i][weight];
            
            int pick = 0;
        
            if(weight-wt[i]>=0)
            pick = solve(i+1,weight-wt[i])+val[i];

            int notpick = solve(i+1,weight);
            
            return dp[i][weight]=max(pick,notpick);
        };   
        
        return solve(0,W);
    }
};
```

## 🔍 How It Works:
1. Start from the first item and the full capacity.
2. For each item, check if the result for (i, weight) is already computed.
3. If not, compute the result by considering both choices: pick or not pick.
4. Store the result in the DP table.
5. Return the result for (0, W).

## ⏱️ Time & Space Complexity:
- **Time Complexity:** O(N*W) - Each subproblem (i, weight) is solved only once.
- **Space Complexity:** O(N*W) - For the DP table and recursion stack.

---

## 💡 Tabulation Approach:
- Bottom-up DP: Build the solution iteratively.
- Use a 2D DP table where dp[i][w] represents the maximum value for items i...N-1 and capacity w.
- **Advantages:**
  - No recursion, so no stack overflow.
  - Easy to visualize and debug.
  - Can be optimized for space.

### 💻 Optimal Code:
```cpp
class Solution {
  public:

    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        // code here
        vector<vector<int>>dp(1001,vector<int>(W+1,0));
        
        for(int w = 0 ; w <= W;w++)dp[val.size()][w]=0;
        
        for(int i = val.size()-1 ; i>=0 ; i-- ){
            
            for(int w = 0 ; w<=W ; w++){
                
                int pick = 0;
                if(w-wt[i]>=0)
                pick = dp[i+1][w-wt[i]]+val[i];
                int notpick = dp[i+1][w];
                
                dp[i][w]=max(pick,notpick);
            }
        }
        
        return dp[0][W];
    }
};
```

## 🔍 How It Works:
1. Initialize the last row (base case) to 0 (no items left).
2. Iterate backwards over items.
3. For each item and weight, compute the best value by picking or not picking the item.
4. Store the result in the DP table.
5. The answer is in dp[0][W].

## ⏱️ Time & Space Complexity:
- **Time Complexity:** O(N*W) - Two nested loops over N and W.
- **Space Complexity:** O(N*W) - For the DP table.

---

## 💡 Space optimization Approach:
- Notice that to compute dp[i][w], we only need dp[i+1][w] (next row).
- Use two 1D arrays: current and next.
- **Advantages:**
  - Reduces space from O(N*W) to O(W).
  - Efficient for large N.
  - Maintains same time complexity.

### 💻 Code:
```cpp
class Solution {
  public:

    int knapsack(int W, vector<int> &val, vector<int> &wt) {
        // code here
        vector<int>cur(W+1,0),next(W+1,0);
        
        for(int w = 0 ; w <= W;w++)next[w]=0;
        
        for(int i = val.size()-1 ; i>=0 ; i-- ){
            
            for(int w = 0 ; w<=W ; w++){
                
                int pick = 0;
                if(w-wt[i]>=0)
                pick = next[w-wt[i]]+val[i];
                int notpick = next[w];
                
                cur[w]=max(pick,notpick);
            }
            next = cur;
        }
        
        return next[W];
    }
};
```

## 🔍 How It Works:
1. Initialize the next array to 0 (base case).
2. Iterate backwards over items.
3. For each item and weight, compute the best value using only the next array.
4. After processing an item, update next to be the current array.
5. The answer is in next[W].

## ⏱️ Time & Space Complexity:
- **Time Complexity:** O(N*W) - Two nested loops over N and W.
- **Space Complexity:** O(W) - Only two arrays of size W+1 are used.

---


## 🎯 Example:
Suppose:
- N = 3
- W = 4
- val = [1, 2, 3]
- wt = [4, 2, 3]

Let's fill the DP table step by step (tabulation):

| Item Index | Weight 0 | Weight 1 | Weight 2 | Weight 3 | Weight 4 |
|------------|----------|----------|----------|----------|----------|
| 3 (base)   |    0     |    0     |    0     |    0     |    0     |
| 2          |    0     |    0     |    0     |    3     |    3     |
| 1          |    0     |    0     |    2     |    3     |    3     |
| 0          |    0     |    0     |    2     |    3     |    3     |

- At each cell, we choose the best between picking or not picking the item.
- The answer is dp[0][4] = 3.


**Output:**
```
3
``` 