# Number of Substrings Containing All Three Characters
## https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/

Given a string s consisting only of characters a, b and c.
Return the number of substrings containing at least one occurrence of all these characters a, b and c.
- Example 1:
Input: s = "abcabc"
Output: 10
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" and "abc" (again). 
- Example 2:
Input: s = "aaacb"
Output: 3
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "aaacb", "aacb" and "acb". 
- Example 3:
Input: s = "abc"
Output: 1

### Approach 1
```cpp
/*
    Approach 1:
        The question is asking about number of substrings contatining atleast one occurence 
        Instead we will find the number of substrings containg atmost 3 distinct characters and 
        atmost 2 distinct characters and find their difference.
        
        
    Approach 2:
        Count += n - r, good window calculation
        
*/
class Solution {
public:
    int atmost(string s, int k){
        int j = 0, no_of_subarray = 0, mapsize = 0;
        unordered_map<int, int>mp;
        for(int i = 0 ; i < s.size() ; i++){
            if(mp[s[i]] == 0)mapsize++;
            mp[s[i]]++;
            while(mapsize > k){
                mp[s[j]]--;
                if(mp[s[j]] == 0)mapsize--;
                j++;
            }
            no_of_subarray += i - j + 1;
        }
        
        return no_of_subarray;
    }
    int numberOfSubstrings(string s) {
        int k = 3;
        return atmost(s, k) - atmost(s, k - 1);
    }
};
```

## Aproach 2
```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        unordered_map<char, int> mp;
        int l = 0, r = 0, cnt = 0;
        int n = s.size();
        while(r < n) {
            mp[s[r]]++;
            // Good window calculation of subarray
            while(mp['a'] && mp['b'] && mp['c']) {
                // Gives the number of subarray with ending (r) pointer 
                cnt += n - r;
                mp[s[l++]]--;
            }
            r++;
        }
        return cnt;
    }
};
```