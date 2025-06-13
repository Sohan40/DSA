# 🧾 Problem: [0/1 Knapsack](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)

## 🧠 Brute Force:
- Recursively consider taking or not taking each item.
- Time: O(2^n)

## 💡 Optimal Approach:
- Use a 2D DP table to store solutions to subproblems.

## 💻 Code:
```cpp
int knapsack(int W, int wt[], int val[], int n) {
    int dp[n+1][W+1] = {0};
    for(int i=0;i<=n;i++) {
        for(int w=0;w<=W;w++) {
            if(i==0 || w==0) dp[i][w] = 0;
            else if(wt[i-1] <= w)
                dp[i][w] = max(val[i-1] + dp[i-1][w-wt[i-1]], dp[i-1][w]);
            else dp[i][w] = dp[i-1][w];
        }
    }
    return dp[n][W];
}
```

## ⏱️ Time & Space Complexity:
- Time: O(n*W)
- Space: O(n*W)

## 📌 Key Takeaways:
- Bottom-up DP approach avoids recursion overhead.
