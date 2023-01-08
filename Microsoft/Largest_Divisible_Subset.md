# Largest Divisible Subset
## https://leetcode.com/problems/largest-divisible-subset/

Given a set of distinct positive integers nums, return the largest subset 
answer such that every pair (answer[i], answer[j]) of elements in this subset 
satisfies:
    answer[i] % answer[j] == 0, or
    answer[j] % answer[i] == 0
If there are multiple solutions, return any of them.
- Example 1:
Input: nums = [1,2,3]
Output: [1,2]
- Explanation: [1,3] is also accepted.
Example 2:
Input: nums = [1,2,4,8]
Output: [1,2,4,8]
*/


```cpp

/*

    This is the variation of the Longest Increasing Subsequence question
    Intuition : Get the longest series of numberb which are divisble in themselves, also the question is asking about the subset so we can think of sorting than applying LIS for checking the increasing order as well as we can check for divisiblity

*/
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        // To store the LIS and to track the elements backward after finding the series and the index of the last element
        vector<int>dp(nums.size(), 1), trackElements(nums.size());
        sort(nums.begin(), nums.end());
        int maxi = 1, lastIndex = 0;
        for(int i = 0 ; i < nums.size() ; i++){
            trackElements[i] = i;
            for(int j = 0 ; j < i ; j++){
                if(nums[i] % nums[j] == 0 && dp[i] < 1 + dp[j]){
                    // Divisible and 1 greater than previous : increasing 
                    dp[i] = 1 + dp[j];
                    
                    // Previous has contirbuted to the answer that means it must be present in the answer so add its index
                    trackElements[i] = j;
                }
            }
            if(dp[i] > maxi){
                // Put the longest streak till now and update the index upto which the maximum has occured
                maxi = dp[i];
                lastIndex = i;
            }
        }
        
        // Maxi elements will be present in the resut array
        vector<int>vv(maxi);
        vv[0] = nums[lastIndex];
        int itr = 1;
        
        while(trackElements[lastIndex] != lastIndex){
            lastIndex = trackElements[lastIndex];
            vv[itr] = nums[lastIndex];
            itr++;
        }
        return vv; 
    }
};
```