# 🧾 Problem: [Special Array II](https://leetcode.com/problems/check-if-array-is-special/)

## 📝 Description
Given an integer array `nums` and a list of queries, each query is a pair `[l, r]` representing a subarray from index `l` to `r` (inclusive). An array is called **special** if every pair of adjacent elements has different parity (i.e., one is even and the other is odd). For each query, determine if the subarray `nums[l..r]` is special.

This problem is significant because it tests your ability to efficiently answer multiple range queries about a property that can be checked in O(1) time with preprocessing, a common pattern in competitive programming and interviews.



## 🧠 Brute Force Approach:
- For each query `[l, r]`, check every adjacent pair in the subarray `nums[l..r]` to see if their parity alternates.
- If all adjacent pairs have different parity, return `true` for that query; otherwise, return `false`.
- Time Complexity: O(q * n) where q is the number of queries and n is the length of the subarray (worst case, each query is O(n)).
- Space Complexity: O(1) (not counting output).

### 💻 Brute Force Code:
```cpp
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {
        vector<bool> ans;
        for (auto& query : queries) {
            int l = query[0], r = query[1];
            bool special = true;
            for (int i = l; i < r; ++i) {
                if (nums[i] % 2 == nums[i+1] % 2) {
                    special = false;
                    break;
                }
            }
            ans.push_back(special);
        }
        return ans;
    }
};
```

## 💡 Better Approach (Interval Grouping + Binary Search):
- Group the array into maximal intervals where parity alternates.
- For each query, use binary search to check if the query range is fully contained within a single interval.
- Advantages:
  - Reduces the number of checks per query to O(log n) (for binary search on intervals)
  - Preprocessing is O(n)
  - Efficient for large numbers of queries

### 💻 Code:
```cpp
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {
        vector<pair<int,int>> intervals;
        int a = 0, b = 0;
        for (int i = 1; i < nums.size(); i++) {
            if (nums[b] % 2 != nums[i] % 2) {
                b = i;
            } else {
                intervals.push_back({a, b});
                a = i;
                b = i;
            }
        }
        intervals.push_back({a, b});
        vector<bool> ans(queries.size(), false);
        for (int i = 0; i < queries.size(); i++) {
            int l = queries[i][0], r = queries[i][1];
            int low = 0, high = intervals.size() - 1;
            while (low <= high) {
                int mid = (low + high) / 2;
                auto [start, end] = intervals[mid];
                if (l >= start && r <= end) {
                    ans[i] = true;
                    break;
                } else if (l > end) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return ans;
    }
};
```

## 🔍 How It Works:
1. Traverse the array and group it into intervals where parity alternates.
2. For each query, use binary search to find if the query range is inside a single interval.
3. If yes, the subarray is special; otherwise, it is not.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(n + q log n) (n for interval grouping, q queries each with binary search)
- Space Complexity: O(n) (for storing intervals)



## 💡 Optimal Approach (Prefix Sum of Parity Transitions):
- Precompute an array `good` where `good[i] = 1` if `nums[i]` and `nums[i+1]` have different parity, else 0.
- Build a prefix sum array `pre` over `good`.
- For a query `[l, r]`, the subarray is special if and only if `pre[r] - pre[l] == r - l` (i.e., every adjacent pair in the range alternates parity).
- Advantages:
  - O(1) query time after O(n) preprocessing
  - Very efficient for large numbers of queries
  - Simple and elegant

### 💻 Code:
```cpp
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<int> good(n-1, 0);
        for (int i = 0; i < n-1; i++) {
            good[i] = (nums[i] % 2 != nums[i+1] % 2) ? 1 : 0;
        }
        vector<int> pre(n, 0);
        for (int i = 1; i < n; i++) {
            pre[i] = pre[i-1] + good[i-1];
        }
        vector<bool> ans;
        for (auto& query : queries) {
            int l = query[0], r = query[1];
            if (r == l) {
                ans.push_back(true);
            } else {
                int transitions = pre[r] - pre[l];
                ans.push_back(transitions == (r - l));
            }
        }
        return ans;
    }
};
```

#### 📊 Visual Example:

Suppose `nums = [3, 2, 5, 6, 7]` and query `[1, 3]` (subarray `[2, 5, 6]`):

```
nums:    3   2   5   6   7
         |___|___|___|
         1   1   1   1   <- good[]
pre:     0   1   2   3   4
             ^       ^
             l=1     r=3
```
- Number of adjacent pairs in [1,3]: r-l = 2
- Number of transitions: pre[3] - pre[1] = 3 - 1 = 2
- Since 2 == 2, the subarray is special.



## 🔍 How It Works:
1. For each adjacent pair in `nums`, mark if their parity alternates.
2. Build a prefix sum so you can count the number of parity transitions in any range in O(1).
3. For each query `[l, r]`, check if the number of transitions is exactly `r - l` (i.e., every adjacent pair alternates parity).
4. If so, the subarray is special; otherwise, it is not.

## ⏱️ Time & Space Complexity:
- Time Complexity: O(n + q) (O(n) preprocessing, O(1) per query)
- Space Complexity: O(n) (for prefix sum arrays)


## 📌 Key Takeaways:
- Preprocessing with prefix sums can reduce query time from O(n) to O(1) for range queries.
- Parity-based problems often benefit from transition arrays and prefix sums.
- Binary search on intervals is a useful technique for range queries when intervals are precomputed.
- Always look for ways to preprocess data to answer multiple queries efficiently.


