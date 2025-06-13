# 🧾 Problem: [Maximum product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## 📝 Description
[Given an integer array nums, find a subarray that has the largest product, and return the product.
The test cases are generated so that the answer will fit in a 32-bit integer.]

## 🎯 Applications
- Dynamic Programming problems
- Array manipulation
- Product calculation in subarrays
- Kadane's algorithm variations

## 🧠 Brute Force Approach:
- Iterate over all subarrays to find the product of the subarrays
- Maintain a answer variable to update the maximum
- Time Complexity: O(n^3)
- Space Complexity: O(1)

### 💻 Brute Force Code:
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        int maxProduct = INT_MIN;
        
        // Try all possible subarrays
        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                int product = 1;
                // Calculate product of current subarray
                for(int k = i; k <= j; k++) {
                    product *= nums[k];
                }
                maxProduct = max(maxProduct, product);
            }
        }
        return maxProduct;
    }
};
```

## 🧠 Better Approach:
- Instead of recalculating every subarray's product, use previously computed product to compute the product of current subarray
- Only the new element that is being added to the subarray is multiplied
- Time Complexity: O(n^2)
- Space Complexity: O(1)

### 💻 Better Approach Code:
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        int maxProduct = INT_MIN;
        
        // Try all possible subarrays
        for(int i = 0; i < n; i++) {
            int product = 1;
            // Calculate product of subarrays starting at i
            for(int j = i; j < n; j++) {
                product *= nums[j];
                maxProduct = max(maxProduct, product);
            }
        }
        return maxProduct;
    }
};
```

## 💡 Optimal Approach:
### 👁️ Observation
- If array has all positive or even number of negative elements max product is the product of entire array
- If array has odd number of negative elements, the answer will be either on the prefix product or suffix product of that element [removing one -ve element makes total product positive]
### Approach
- Find prefix and suffix simultaneous and update the max 
- If prefix or suffix becomes 0, initialize them to 1 so that we can calculate the product of subarray that doesn't include zero
- Advantages:
  - Single pass solution
  - Handles all edge cases (negative numbers, zeros)
  - Space efficient

### 💻 Optimal Code:
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int ans = INT_MIN;
        int pre = 1;
        int suf = 1;
        int n = nums.size();
        for(int i = 0; i < nums.size(); i++) {
            pre *= nums[i];
            suf *= nums[n-i-1];
            ans = max({ans, pre, suf});
            if(pre == 0) pre = 1;
            if(suf == 0) suf = 1;
        }
        return ans;
    }
};
```

## 🔍 How It Works:
1. Initialize prefix and suffix products as 1
2. Traverse array from both ends simultaneously
3. Calculate prefix product from left to right
4. Calculate suffix product from right to left
5. Update maximum product if current prefix or suffix is larger
6. Reset prefix/suffix to 1 if they become 0

## ⏱️ Time & Space Complexity:
- Time Complexity: O(n) - Single pass through the array
- Space Complexity: O(1) - Only using constant extra space

## 📌 Key Takeaways:
- Prefix and suffix products help handle negative numbers efficiently
- Resetting to 1 when encountering 0 helps handle zero values
- Single pass solution is possible by tracking both prefix and suffix
- Maximum product can be achieved by either including or excluding negative numbers
- Why prefix/suffix works:
  - For positive numbers: Product keeps increasing, so maximum will be at the end
  - For even negative numbers: Product remains positive, so maximum will be at the end
  - For odd negative numbers: Maximum will be either:
    * Prefix product up to the last negative number
    * Suffix product from the first negative number
  - For arrays with zeros: Maximum will be either:
    * Product before the zero
    * Product after the zero
    * Zero itself (if all other products are negative)
- The approach works because any subarray's maximum product must be either:
  1. A prefix product (from start to some point)
  2. A suffix product (from some point to end)
  3. A combination of prefix and suffix (which is handled by resetting to 1 at zeros)

## 🎯 Example:
```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
``` 