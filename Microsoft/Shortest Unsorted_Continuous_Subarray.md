# Shortest Unsorted Continuous Subarray
## https://leetcode.com/problems/shortest-unsorted-continuous-subarray/

Given an integer array nums, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order.

Return the shortest such subarray and output its length.
- Example 1:
Input: nums = [2,6,4,8,10,9,15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
- Example 2:
Input: nums = [1,2,3,4]
Output: 0
- Example 3:
Input: nums = [1]
Output: 0

```cpp
class Solution {
public:
    /*
        Find the correct position of low and high using while loops and some condition then adjust for the edge case
    */
    
    int findUnsortedSubarray(vector<int>& nums) {
        int n = nums.size();
        int low = 0, high = n - 1, wMin = INT_MAX, wMax = INT_MIN;
        
        // Finding a good window
        while (low + 1 < n && nums[low] <= nums[low + 1]) low++;
        while (high - 1 >= 0 && nums[high - 1] <= nums[high]) high--;

        if (low == n - 1) return 0;
        
        // Checking if the least element in the window is smaller or not from the last element of starting and vice versa
        for (int i = low; i <= high; i++) {
            wMin = min(wMin, nums[i]);
            wMax = max(wMax, nums[i]);
        }
        
        // Adjust the window
        while (low - 1 >= 0 && nums[low - 1] > wMin) low--;
        while (high + 1 <= n - 1 && nums[high + 1] < wMax) high++;

        return high - low + 1;
    }
};
```