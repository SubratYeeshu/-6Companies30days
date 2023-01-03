# Bulls And Cows
## https://leetcode.com/problems/bulls-and-cows/
You are playing the Bulls and Cows game with your friend.
You write down a secret number and ask your friend to guess what the number is. When your friend makes a guess, you provide a hint with the following info:
    The number of "bulls", which are digits in the guess that are in the correct position.
    The number of "cows", which are digits in the guess that are in your secret number but are located in the wrong position. Specifically, the non-bull digits in the guess that could be rearranged such that they become bulls.
Given the secret number secret and your friend's guess guess, return the hint for your friend's guess.
The hint should be formatted as "xAyB", where x is the number of bulls and y is the number of cows. Note that both secret and guess may contain duplicate digits.
- Example 1:
Input: secret = "1807", guess = "7810"
Output: "1A3B"
- Explanation: Bulls are connected with a '|' and cows are underlined:
"1807"
  |
"7810"
Example 2:
Input: secret = "1123", guess = "0111"
Output: "1A1B"
Explanation: Bulls are connected with a '|' and cows are underlined:
"1123"        "1123"
  |      or     |
"0111"        "0111"
Note that only one of the two unmatched 1s is counted as a cow since the non-bull digits can only be rearranged to allow one 1 to be a bull.


### One Pass
```cpp
class Solution {
public:
    /*
        x is the number of bulls and y is the number of cows 
        Output: xAyB
        Bulls: Location Matches
        Cows: Contains 
    */
    
         string getHint(string secret, string guess) {
             unordered_map<char, int>s, g;
             int x = 0, y = 0;
             for(int i = 0 ; i < guess.size() ; i++){
                 if(secret[i] == guess[i])x++;
                 else{
                     if(s[guess[i]] > 0){
                         y++;
                         s[guess[i]]--;
                     }
                     else g[guess[i]]++;
                     if(g[secret[i]] > 0){
                         y++;
                         g[secret[i]]--;
                     }else s[secret[i]]++;
                 }
             }
            return (to_string(x)+"A"+to_string(y)+"B");
     }
 };
    
```

### Two Pass
```cpp
     string getHint(string secret, string guess) {
         unordered_map<char, int>um;
         int bulls_match_x = 0, cows_contains_y = 0;
        
         // Check for bulls
         for(int i = 0 ; i < secret.length() ; i++){
             if(secret[i] == guess[i])bulls_match_x++;
             else um[secret[i]]++;
         }
         
         // Check for cows using mapping
         for(int i = 0 ; i < secret.length() ; i++){
             if(secret[i] == guess[i])continue;
             else if(um[guess[i]]){
                 um[guess[i]]--;  
                 cows_contains_y++;
             } 
         }
         return (to_string(bulls_match_x)+"A"+to_string(cows_contains_y)+"B");
     }
 };
```
