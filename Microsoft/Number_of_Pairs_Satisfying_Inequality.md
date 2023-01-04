# Number of Pairs Satisfying Inequality
## https://leetcode.com/problems/number-of-pairs-satisfying-inequality/

You are given two 0-indexed integer arrays nums1 and nums2, each of size n, and an integer diff. Find the number of pairs (i, j) such that:

    0 <= i < j <= n - 1 and
    nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff.

Return the number of pairs that satisfy the conditions.

 

- Example 1:
Input: nums1 = [3,2,5], nums2 = [2,2,1], diff = 1
Output: 3
Explanation:
There are 3 pairs that satisfy the conditions:
1. i = 0, j = 1: 3 - 2 <= 2 - 2 + 1. Since i < j and 1 <= 1, this pair satisfies the conditions.
2. i = 0, j = 2: 3 - 5 <= 2 - 1 + 1. Since i < j and -2 <= 2, this pair satisfies the conditions.
3. i = 1, j = 2: 2 - 5 <= 2 - 1 + 1. Since i < j and -3 <= 2, this pair satisfies the conditions.
Therefore, we return 3.

- Example 2:
Input: nums1 = [3,-1], nums2 = [-2,2], diff = -1
Output: 0
Explanation:
Since there does not exist any pair that satisfies the conditions, we return 0.


```cpp
/*
    Using Merge Sort
    nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff
    (nums1[i] - nums2[i]) - (nums1[j] - nums2[j]) <= diff
    nums[i] - nums[j] <= diff
    
    Simpy do a merge sort and while returning and merging the sorted arary first do the comparision and store it in res.
*/
class Solution {
public:
    long long res = 0;
    int dif = 0;
    void merge(vector<int> &nums,int start,int mid,int end){
        vector<int> temp(end - start + 1);
        int i = start; 
        int j = mid + 1;
        int idx = 0;
        
        // Couting the pairs
        while(i <= mid && j <= end){
            if(nums[i] <= dif + nums[j]){      
                res += end - j + 1;      
                i++;
            }
            else j++;
        }
        
        i = start;
        j = mid + 1;
        
        // Merge two sorted array
        while(i <= mid && j <= end){
            if(nums[i] <= nums[j]) temp[idx++] = nums[i++];
            else temp[idx++] = nums[j++];
        }
        
        // Merging the sorted array which is left out 
        while(i <= mid)temp[idx++] = nums[i++];
        while(j <= end)temp[idx++] = nums[j++];
        idx = 0;
        
        // Storing Back the array after it is sorted and the pairs are counted
        for(int itr = start; itr <= end ; itr++, idx++)nums[itr] = temp[idx];
        }
            
    void mergeSort(int start, int end, vector<int>&nums){
        if(start >= end)return;
        
        int mid = start + (end - start) / 2;
        mergeSort(start, mid, nums);
        mergeSort(mid + 1, end, nums);
        merge(nums, start, mid, end);
    }
    
    long long numberOfPairs(vector<int>& nums1, vector<int>& nums2, int diff) {
        dif = diff;
        int n = nums1.size();
        vector<int>nums(n);
        for(int i = 0 ; i < n ; i++)nums[i] = nums1[i] - nums2[i];
        mergeSort(0, n - 1, nums);
        return res;
    }
};
```
