# 🧾 Problem: [Two Sum](https://leetcode.com/problems/two-sum)

## 🧠 Brute Force:
- Try all pairs of elements to check if their sum is equal to the target.
- Time: O(n^2)

## 💡 Optimal Approach:
- Use a hashmap to store complements and check in a single pass.
- Time: O(n)

## 💻 Code:
```cpp
unordered_map<int, int> mp;
for(int i = 0; i < nums.size(); i++) {
    if(mp.count(target - nums[i]))
        return {mp[target - nums[i]], i};
    mp[nums[i]] = i;
}
```

## ⏱️ Time & Space Complexity:
- Time: O(n)
- Space: O(n)

## 🧪 Edge Cases:
- Empty input
- Same number used twice

## 📌 Key Takeaways:
- Hashmap is great for O(1) lookups.
